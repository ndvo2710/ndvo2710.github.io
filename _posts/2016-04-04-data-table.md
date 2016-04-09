---
layout: post
title: data.table
excerpt: "Just about everything you'll need to do with data.table."
modified: 2016-04-04
tags: [R, data.table, tutorial]
comments: true
image:
  feature: data-table/data-table.png
  credit: datacamp.
  creditlink: https://s3.amazonaws.com/assets.datacamp.com/img/blog/data+table+cheat+sheet.pdf
---


```r
# Create my_first_data_table
> my_first_data_table = data.table(x = c("a", "b", "c", "d", "e"),
                                 y = c(1, 2, 3, 4, 5))
>   
# Create a data.table using recycling
> DT = data.table(a = c(1L,2L,1L,2L), b = LETTERS[1:4])
>   
# Print the third row to the console
> DT[3]
   a b
1: 1 C
>   
# Print the second and third row to the console, but do not commas
> DT[2:3]
   a b
1: 2 B
2: 1 C
```

```r
> # Print the penultimate row of DT using .N
> DT[.N -1]
   a b
1: 1 C
> 
> # Print the column names of DT, and number of rows and number of columns
> names(DT)
[1] "a" "b"
> dim(DT)
[1] 4 2
> 
> # Select row 2 twice and row 3
> DT[c(2,2,3)]
   a b
1: 2 B
2: 2 B
3: 1 C
```


```r
> DT[,.(B)]
   B
1: a
2: b
3: c
4: d
5: e
> DT 
... 
   A B  C
1: 1 a  6
2: 2 b  7
3: 3 c  8
4: 4 d  9
5: 5 e 10
> DT[, B]
[1] "a" "b" "c" "d" "e"
```



```r
> DT
   A B  C
1: 1 a  6
2: 2 b  7
3: 3 c  8
4: 4 d  9
5: 5 e 10
> A = 4
> DT[,.(A)]
   A
1: 1
2: 2
3: 3
4: 4
5: 5
> # Subset rows 1 and 3, and columns B and C
> DT
   A B  C
1: 1 a  6
2: 2 b  7
3: 3 c  8
4: 4 d  9
5: 5 e 10
> DT[c(1,3),.(B,C)]
   B C
1: a 6
2: c 8
> 
> # Assign to ans the correct value
> DT
   A B  C
1: 1 a  6
2: 2 b  7
3: 3 c  8
4: 4 d  9
5: 5 e 10
> ans = DT[,.(B, val = A*C)]
> ans
   B val
1: a   6
2: b  14
3: c  24
4: d  36
5: e  50
> 
> # Fill in the blanks such that ans2 equals target
> target <- data.table(B = c("a", "b", "c", "d", "e", "a", "b", "c", "d", "e"), 
                     val = as.integer(c(6:10, 1:5)))
> ans2 <- DT[, .(B, val = target$val )]
> ans2
    B val
 1: a   6
 2: b   7
 3: c   8
 4: d   9
 5: e  10
 6: a   1
 7: b   2
 8: c   3
 9: d   4
10: e   5
> target
    B val
 1: a   6
 2: b   7
 3: c   8
 4: d   9
 5: e  10
 6: a   1
 7: b   2
 8: c   3
 9: d   4
10: e   5
> 
```



```r
> # Convert iris to a data.table: DT
> head(iris)
  Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1          5.1         3.5          1.4         0.2  setosa
2          4.9         3.0          1.4         0.2  setosa
3          4.7         3.2          1.3         0.2  setosa
4          4.6         3.1          1.5         0.2  setosa
5          5.0         3.6          1.4         0.2  setosa
6          5.4         3.9          1.7         0.4  setosa
> DT = data.table(iris)
> head(DT)
   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1:          5.1         3.5          1.4         0.2  setosa
2:          4.9         3.0          1.4         0.2  setosa
3:          4.7         3.2          1.3         0.2  setosa
4:          4.6         3.1          1.5         0.2  setosa
5:          5.0         3.6          1.4         0.2  setosa
6:          5.4         3.9          1.7         0.4  setosa
> 
> # For each Species, print the mean Sepal.Length
> DT[,mean(Sepal.Length), by = Species]
      Species    V1
1:     setosa 5.006
2: versicolor 5.936
3:  virginica 6.588
>   
> # Print mean Sepal.Length, grouping by first letter of Species
> DT[,mean(Sepal.Length), by = substr(Species,1,1)]
   substr    V1
1:      s 5.006
2:      v 6.262
```



```r
> DT <- as.data.table(iris)
> 
> # Group the specimens by Sepal area (to the nearest 10 cm2) and count how many occur in each group.
> DT[,.N, by=10*round(Sepal.Length*Sepal.Width/10)]
   round   N
1:    20 117
2:    10  29
3:    30   4
> 
> # Now name the output columns `Area` and `Count`
> DT[,.(Count=.N), by=.(Area=10*round(Sepal.Length*Sepal.Width/10))]  
   Area Count
1:   20   117
2:   10    29
3:   30     4
```



```r
> # Create the data.table DT
> set.seed(1L)
> DT <- data.table(A=rep(letters[2:1], each=4L), B=rep(1:4, each=2L), C=sample(8))
> DT
   A B C
1: b 1 3
2: b 1 8
3: b 2 4
4: b 2 5
5: a 3 1
6: a 3 7
7: a 4 2
8: a 4 6
> # Create the new data.table, DT2
> DT2 <- DT[, .(C=cumsum(C)), by=.(A,B)]
> DT2
   A B  C
1: b 1  3
2: b 1 11
3: b 2  4
4: b 2  9
5: a 3  1
6: a 3  8
7: a 4  2
8: a 4  8
> # Select from DT2 the last two values from C while you group by A
> DT2[, .(C=tail(C,2)), by=A] 
   A C
1: b 4
2: b 9
3: a 2
4: a 8
```



```r
 Next Exercise
script.R


1
2
3
4
5
6
7
8
9
# The data.table package has already been loaded
# Build DT
set.seed(1L)
DT <- data.table(A = rep(letters[2:1], each = 4L), B = rep(1:4, each = 2L), C 
= sample(8)) 
DT
# Use chaining:
# Cumsum of C while grouping by A and B, and then select last two values of C 
while grouping by A
DT[,.(C = cumsum(C)), by=.(A,B)][,.(C=tail(C,2)),by=A]
 Submit Answer
R Console
Slides
> # The data.table package has already been loaded
> 
> # Build DT
> set.seed(1L)
> DT <- data.table(A = rep(letters[2:1], each = 4L), B = rep(1:4, each = 2L), C = sample(8)) 
> DT
   A B C
1: b 1 3
2: b 1 8
3: b 2 4
4: b 2 5
5: a 3 1
6: a 3 7
7: a 4 2
8: a 4 6
> # Use chaining:
> # Cumsum of C while grouping by A and B, and then select last two values of C while grouping by A
> DT[,.(C = cumsum(C)), by=.(A,B)][,.(C=tail(C,2)),by=A]
   A C
1: b 4
2: b 9
3: a 2
4: a 8
> 
```



```r
> DT <- as.data.table(iris)
> # Perform chained operations on DT
> DT[, .( Sepal.Length=median(Sepal.Length), 
        Sepal.Width=median(Sepal.Width), 
        Petal.Length=median(Petal.Length),
        Petal.Width=median(Petal.Width)), by=Species][order(-Species)]
      Species Sepal.Length Sepal.Width Petal.Length Petal.Width
1:  virginica          6.5         3.0         5.55         2.0
2: versicolor          5.9         2.8         4.35         1.3
3:     setosa          5.0         3.4         1.50         0.2
> 
```



```r
> DT
   x  y  z
1: 2  1  2
2: 1  3  4
3: 2  5  6
4: 1  7  8
5: 2  9 10
6: 2 11 12
7: 1 13 14
> 
> # Mean of columns
> DT[, lapply(.SD, mean), by=x]
   x        y        z
1: 2 6.500000 7.500000
2: 1 7.666667 8.666667
> 
> # Median of columns
> DT[, lapply(.SD, median), by=x]
   x y z
1: 2 7 8
2: 1 7 8
> 
```



```r
> DT
   grp Q1 Q2 Q3 H1 H2
1:   6  2  5  2  3  5
2:   6  2  5  1  4  2
3:   8  3  4  4  5  4
4:   8  5  4  2  2  1
5:   8  2  1  4  4  2
> 
> # Calculate the sum of the Q columns
> DT[, lapply(.SD, sum), .SDcols=2:4]
   Q1 Q2 Q3
1: 14 19 13
> 
> # Calculate the sum of columns H1 and H2 
> DT[,lapply(.SD,sum), .SDcols=paste0("H",1:2)]
   H1 H2
1: 18 14
> 
> # Select all but the first row of groups 1 and 2, returning only the grp column and the Q columns. 
> DT[,.SD[-1], by=grp, .SDcols=paste0("Q",1:3)]
   grp Q1 Q2 Q3
1:   6  2  5  1
2:   8  5  4  2
3:   8  2  1  4
> 
```



```r
> DT
   x  y  z
1: 2  1  2
2: 1  3  4
3: 2  5  6
4: 1  7  8
5: 2  9 10
6: 2 11 12
7: 1 13 14
> # Sum of all columns and the number of rows
> DT[, c(lapply(.SD, sum), .N), by=x, .SDcols=c("x", "y", "z")]
   x x  y  z N
1: 2 8 26 30 4
2: 1 3 23 26 3
> 
> # Cumulative sum of column x and y while grouping by x and z > 8
> DT[, lapply(.SD, cumsum),by=.(by1=x, by2=z>8), .SDcols=c("x", "y")]
   by1   by2 x  y
1:   2 FALSE 2  1
2:   2 FALSE 4  6
3:   1 FALSE 1  3
4:   1 FALSE 2 10
5:   2  TRUE 2  9
6:   2  TRUE 4 20
7:   1  TRUE 1 13
> 
> # Chaining
> DT[, lapply(.SD, cumsum),by=.(by1=x, by2=z>8), .SDcols=1:2][, lapply(.SD, max), by=by1, .SDcols=c("x", "y")]
   by1 x  y
1:   2 4 20
2:   1 2 13
```



```r
 Submit Answer
R Console
Slides
> # The data.table DT
> DT <- data.table(A = letters[c(1, 1, 1, 2, 2)], B = 1:5)
> DT
   A B
1: a 1
2: a 2
3: a 3
4: b 4
5: b 5
> 
> # Add column by reference: Total
> DT[,Total:=sum(B),by=A]
   A B Total
1: a 1     6
2: a 2     6
3: a 3     6
4: b 4     9
5: b 5     9
> 
> # Add 1 to column B
> DT[c(2,4), B:=B+1L]
   A B Total
1: a 1     6
2: a 3     6
3: a 3     6
4: b 5     9
5: b 5     9
> 
> # Add a new column Total2
> DT[2:4, Total2:=sum(B),by=A]
   A B Total Total2
1: a 1     6     NA
2: a 3     6      6
3: a 3     6      6
4: b 5     9      5
5: b 5     9     NA
> 
> # Remove the Total column
> DT[, Total:=NULL]
   A B Total2
1: a 1     NA
2: a 3      6
3: a 3      6
4: b 5      5
5: b 5     NA
> 
> # Select the third column using `[[`
> DT[[3]] 
[1] NA  6  6  5 NA
```



```r
> # Set the seed
> set.seed(1)
> 
> # Check the DT that is made available to you
> DT
    A B C D
 1: 2 2 5 3
 2: 2 1 2 3
 3: 3 4 4 3
 4: 5 2 1 1
 5: 2 4 2 5
 6: 5 3 2 4
 7: 5 4 1 4
 8: 4 5 2 1
 9: 4 2 5 4
10: 1 4 2 3
> 
> # For loop with set
> for(i in 2:4)
  set(DT, sample(10,3), i, NA)
> 
> # Change the column names to lowercase
> setnames(DT, tolower(names(DT)))
> 
> # Print the resulting DT to the console
> DT
    a  b  c  d
 1: 2  2  5  3
 2: 2  1 NA  3
 3: 3 NA  4  3
 4: 5 NA  1  1
 5: 2 NA  2  5
 6: 5  3  2 NA
 7: 5  4  1  4
 8: 4  5 NA  1
 9: 4  2  5 NA
10: 1  4 NA NA
> 
```



```r
> DT <- data.table(a = letters[c(1, 1, 1, 2, 2)], b = 1)
> DT
   a b
1: a 1
2: a 1
3: a 1
4: b 1
5: b 1
> # Add a postfix "_2" to all column names
> setnames(DT, paste0(names(DT),"_2"))
> DT
   a_2 b_2
1:   a   1
2:   a   1
3:   a   1
4:   b   1
5:   b   1
> # Change column name "a_2" to "A2"
> setnames(DT, "a_2", "A2")
> DT
   A2 b_2
1:  a   1
2:  a   1
3:  a   1
4:  b   1
5:  b   1
> # Reverse the order of the columns
> setcolorder(DT, 2:1)
> DT
   b_2 A2
1:   1  a
2:   1  a
3:   1  a
4:   1  b
5:   1  b
```



```r
> # Convert iris to a data.table
> iris = as.data.table(iris)
> 
> # Species is "virginica"
> iris[Species == "virginica"]
    Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
 1:          6.3         3.3          6.0         2.5 virginica
 2:          5.8         2.7          5.1         1.9 virginica
 3:          7.1         3.0          5.9         2.1 virginica
 4:          6.3         2.9          5.6         1.8 virginica
 5:          6.5         3.0          5.8         2.2 virginica
 6:          7.6         3.0          6.6         2.1 virginica
 7:          4.9         2.5          4.5         1.7 virginica
 8:          7.3         2.9          6.3         1.8 virginica
 9:          6.7         2.5          5.8         1.8 virginica
10:          7.2         3.6          6.1         2.5 virginica
11:          6.5         3.2          5.1         2.0 virginica
12:          6.4         2.7          5.3         1.9 virginica
13:          6.8         3.0          5.5         2.1 virginica
14:          5.7         2.5          5.0         2.0 virginica
15:          5.8         2.8          5.1         2.4 virginica
16:          6.4         3.2          5.3         2.3 virginica
17:          6.5         3.0          5.5         1.8 virginica
18:          7.7         3.8          6.7         2.2 virginica
19:          7.7         2.6          6.9         2.3 virginica
20:          6.0         2.2          5.0         1.5 virginica
21:          6.9         3.2          5.7         2.3 virginica
22:          5.6         2.8          4.9         2.0 virginica
23:          7.7         2.8          6.7         2.0 virginica
24:          6.3         2.7          4.9         1.8 virginica
25:          6.7         3.3          5.7         2.1 virginica
26:          7.2         3.2          6.0         1.8 virginica
27:          6.2         2.8          4.8         1.8 virginica
28:          6.1         3.0          4.9         1.8 virginica
29:          6.4         2.8          5.6         2.1 virginica
30:          7.2         3.0          5.8         1.6 virginica
31:          7.4         2.8          6.1         1.9 virginica
32:          7.9         3.8          6.4         2.0 virginica
33:          6.4         2.8          5.6         2.2 virginica
34:          6.3         2.8          5.1         1.5 virginica
35:          6.1         2.6          5.6         1.4 virginica
36:          7.7         3.0          6.1         2.3 virginica
37:          6.3         3.4          5.6         2.4 virginica
38:          6.4         3.1          5.5         1.8 virginica
39:          6.0         3.0          4.8         1.8 virginica
40:          6.9         3.1          5.4         2.1 virginica
41:          6.7         3.1          5.6         2.4 virginica
42:          6.9         3.1          5.1         2.3 virginica
43:          5.8         2.7          5.1         1.9 virginica
44:          6.8         3.2          5.9         2.3 virginica
45:          6.7         3.3          5.7         2.5 virginica
46:          6.7         3.0          5.2         2.3 virginica
47:          6.3         2.5          5.0         1.9 virginica
48:          6.5         3.0          5.2         2.0 virginica
49:          6.2         3.4          5.4         2.3 virginica
50:          5.9         3.0          5.1         1.8 virginica
    Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
> 
> 
> # Species is either "virginica" or "versicolor"
> iris[Species %in% c("virginica","versicolor")]
     Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
  1:          6.3         3.3          6.0         2.5  virginica
  2:          5.8         2.7          5.1         1.9  virginica
  3:          7.1         3.0          5.9         2.1  virginica
  4:          6.3         2.9          5.6         1.8  virginica
  5:          6.5         3.0          5.8         2.2  virginica
  6:          7.6         3.0          6.6         2.1  virginica
  7:          4.9         2.5          4.5         1.7  virginica
  8:          7.3         2.9          6.3         1.8  virginica
  9:          6.7         2.5          5.8         1.8  virginica
 10:          7.2         3.6          6.1         2.5  virginica
 11:          6.5         3.2          5.1         2.0  virginica
 12:          6.4         2.7          5.3         1.9  virginica
 13:          6.8         3.0          5.5         2.1  virginica
 14:          5.7         2.5          5.0         2.0  virginica
 15:          5.8         2.8          5.1         2.4  virginica
 16:          6.4         3.2          5.3         2.3  virginica
 17:          6.5         3.0          5.5         1.8  virginica
 18:          7.7         3.8          6.7         2.2  virginica
 19:          7.7         2.6          6.9         2.3  virginica
 20:          6.0         2.2          5.0         1.5  virginica
 21:          6.9         3.2          5.7         2.3  virginica
 22:          5.6         2.8          4.9         2.0  virginica
 23:          7.7         2.8          6.7         2.0  virginica
 24:          6.3         2.7          4.9         1.8  virginica
 25:          6.7         3.3          5.7         2.1  virginica
 26:          7.2         3.2          6.0         1.8  virginica
 27:          6.2         2.8          4.8         1.8  virginica
 28:          6.1         3.0          4.9         1.8  virginica
 29:          6.4         2.8          5.6         2.1  virginica
 30:          7.2         3.0          5.8         1.6  virginica
 31:          7.4         2.8          6.1         1.9  virginica
 32:          7.9         3.8          6.4         2.0  virginica
 33:          6.4         2.8          5.6         2.2  virginica
 34:          6.3         2.8          5.1         1.5  virginica
 35:          6.1         2.6          5.6         1.4  virginica
 36:          7.7         3.0          6.1         2.3  virginica
 37:          6.3         3.4          5.6         2.4  virginica
 38:          6.4         3.1          5.5         1.8  virginica
 39:          6.0         3.0          4.8         1.8  virginica
 40:          6.9         3.1          5.4         2.1  virginica
 41:          6.7         3.1          5.6         2.4  virginica
 42:          6.9         3.1          5.1         2.3  virginica
 43:          5.8         2.7          5.1         1.9  virginica
 44:          6.8         3.2          5.9         2.3  virginica
 45:          6.7         3.3          5.7         2.5  virginica
 46:          6.7         3.0          5.2         2.3  virginica
 47:          6.3         2.5          5.0         1.9  virginica
 48:          6.5         3.0          5.2         2.0  virginica
 49:          6.2         3.4          5.4         2.3  virginica
 50:          5.9         3.0          5.1         1.8  virginica
 51:          7.0         3.2          4.7         1.4 versicolor
 52:          6.4         3.2          4.5         1.5 versicolor
 53:          6.9         3.1          4.9         1.5 versicolor
 54:          5.5         2.3          4.0         1.3 versicolor
 55:          6.5         2.8          4.6         1.5 versicolor
 56:          5.7         2.8          4.5         1.3 versicolor
 57:          6.3         3.3          4.7         1.6 versicolor
 58:          4.9         2.4          3.3         1.0 versicolor
 59:          6.6         2.9          4.6         1.3 versicolor
 60:          5.2         2.7          3.9         1.4 versicolor
 61:          5.0         2.0          3.5         1.0 versicolor
 62:          5.9         3.0          4.2         1.5 versicolor
 63:          6.0         2.2          4.0         1.0 versicolor
 64:          6.1         2.9          4.7         1.4 versicolor
 65:          5.6         2.9          3.6         1.3 versicolor
 66:          6.7         3.1          4.4         1.4 versicolor
 67:          5.6         3.0          4.5         1.5 versicolor
 68:          5.8         2.7          4.1         1.0 versicolor
 69:          6.2         2.2          4.5         1.5 versicolor
 70:          5.6         2.5          3.9         1.1 versicolor
 71:          5.9         3.2          4.8         1.8 versicolor
 72:          6.1         2.8          4.0         1.3 versicolor
 73:          6.3         2.5          4.9         1.5 versicolor
 74:          6.1         2.8          4.7         1.2 versicolor
 75:          6.4         2.9          4.3         1.3 versicolor
 76:          6.6         3.0          4.4         1.4 versicolor
 77:          6.8         2.8          4.8         1.4 versicolor
 78:          6.7         3.0          5.0         1.7 versicolor
 79:          6.0         2.9          4.5         1.5 versicolor
 80:          5.7         2.6          3.5         1.0 versicolor
 81:          5.5         2.4          3.8         1.1 versicolor
 82:          5.5         2.4          3.7         1.0 versicolor
 83:          5.8         2.7          3.9         1.2 versicolor
 84:          6.0         2.7          5.1         1.6 versicolor
 85:          5.4         3.0          4.5         1.5 versicolor
 86:          6.0         3.4          4.5         1.6 versicolor
 87:          6.7         3.1          4.7         1.5 versicolor
 88:          6.3         2.3          4.4         1.3 versicolor
 89:          5.6         3.0          4.1         1.3 versicolor
 90:          5.5         2.5          4.0         1.3 versicolor
 91:          5.5         2.6          4.4         1.2 versicolor
 92:          6.1         3.0          4.6         1.4 versicolor
 93:          5.8         2.6          4.0         1.2 versicolor
 94:          5.0         2.3          3.3         1.0 versicolor
 95:          5.6         2.7          4.2         1.3 versicolor
 96:          5.7         3.0          4.2         1.2 versicolor
 97:          5.7         2.9          4.2         1.3 versicolor
 98:          6.2         2.9          4.3         1.3 versicolor
 99:          5.1         2.5          3.0         1.1 versicolor
100:          5.7         2.8          4.1         1.3 versicolor
     Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
> 
```




```r
> # iris as a data.table
> iris <- as.data.table(iris)
> 
> # Remove the "Sepal." prefix
> setnames(iris, gsub("^Sepal[.]", "", names(iris)))
> 
> # Remove the two columns starting with "Petal"
> iris <- iris[, grep("^Petal",names(iris)) := NULL]
> 
> 
```



```r
> iris
     Length Width   Species
  1:    5.1   3.5    setosa
  2:    4.9   3.0    setosa
  3:    4.7   3.2    setosa
  4:    4.6   3.1    setosa
  5:    5.0   3.6    setosa
 ---                       
146:    6.7   3.0 virginica
147:    6.3   2.5 virginica
148:    6.5   3.0 virginica
149:    6.2   3.4 virginica
150:    5.9   3.0 virginica
> 
> # Rows for which area is greater than 20 sq cm
> iris[ Width*Length > 20 ]
    Length Width    Species
 1:    5.4   3.9     setosa
 2:    5.8   4.0     setosa
 3:    5.7   4.4     setosa
 4:    5.4   3.9     setosa
 5:    5.7   3.8     setosa
 6:    5.2   4.1     setosa
 7:    5.5   4.2     setosa
 8:    7.0   3.2 versicolor
 9:    6.4   3.2 versicolor
10:    6.9   3.1 versicolor
11:    6.3   3.3 versicolor
12:    6.7   3.1 versicolor
13:    6.7   3.0 versicolor
14:    6.0   3.4 versicolor
15:    6.7   3.1 versicolor
16:    6.3   3.3  virginica
17:    7.1   3.0  virginica
18:    7.6   3.0  virginica
19:    7.3   2.9  virginica
20:    7.2   3.6  virginica
21:    6.5   3.2  virginica
22:    6.8   3.0  virginica
23:    6.4   3.2  virginica
24:    7.7   3.8  virginica
25:    7.7   2.6  virginica
26:    6.9   3.2  virginica
27:    7.7   2.8  virginica
28:    6.7   3.3  virginica
29:    7.2   3.2  virginica
30:    7.2   3.0  virginica
31:    7.4   2.8  virginica
32:    7.9   3.8  virginica
33:    7.7   3.0  virginica
34:    6.3   3.4  virginica
35:    6.9   3.1  virginica
36:    6.7   3.1  virginica
37:    6.9   3.1  virginica
38:    6.8   3.2  virginica
39:    6.7   3.3  virginica
40:    6.7   3.0  virginica
41:    6.2   3.4  virginica
    Length Width    Species
> 
> # Add new boolean column is_large (area greater than 25 sq cm)
> iris[, is_large := Width*Length > 25]
     Length Width   Species is_large
  1:    5.1   3.5    setosa    FALSE
  2:    4.9   3.0    setosa    FALSE
  3:    4.7   3.2    setosa    FALSE
  4:    4.6   3.1    setosa    FALSE
  5:    5.0   3.6    setosa    FALSE
 ---                                
146:    6.7   3.0 virginica    FALSE
147:    6.3   2.5 virginica    FALSE
148:    6.5   3.0 virginica    FALSE
149:    6.2   3.4 virginica    FALSE
150:    5.9   3.0 virginica    FALSE
> 
> # Now select rows again where the area is greater than 25 square centimeters
> iris[is_large == TRUE]
   Length Width   Species is_large
1:    5.7   4.4    setosa     TRUE
2:    7.2   3.6 virginica     TRUE
3:    7.7   3.8 virginica     TRUE
4:    7.9   3.8 virginica     TRUE
> iris[(is_large)]
   Length Width   Species is_large
1:    5.7   4.4    setosa     TRUE
2:    7.2   3.6 virginica     TRUE
3:    7.7   3.8 virginica     TRUE
4:    7.9   3.8 virginica     TRUE
```



```r
> # The 'keyed' data.table DT
> DT <- data.table(A = letters[c(2, 1, 2, 3, 1, 2, 3)], 
                 B = c(5, 4, 1, 9,8 ,8, 6), 
                 C = 6:12)
> setkey(DT,A,B)
> 
> # Select the "b" group
> DT["b"] 
   A B  C
1: b 1  8
2: b 5  6
3: b 8 11
> 
> # "b" and "c" groups
> DT[c("b","c")]
   A B  C
1: b 1  8
2: b 5  6
3: b 8 11
4: c 6 12
5: c 9  9
> 
> # The first row of the "b" and "c" groups
> DT[c("b","c"), mult="first"]
   A B  C
1: b 1  8
2: c 6 12
> 
> # First and last row of the "b" and "c" groups
> DT[c("b","c"), .SD[c(1,.N)], by=.EACHI]
   A B  C
1: b 1  8
2: b 8 11
3: c 6 12
4: c 9  9
> 
> # Copy and extend code for instruction 4: add printout
> DT[c("b","c"), {print(.SD);.SD[c(1,.N)]}, by=.EACHI]
   B  C
1: 1  8
2: 5  6
3: 8 11
   B  C
1: 6 12
2: 9  9
   A B  C
1: b 1  8
2: b 8 11
3: c 6 12
4: c 9  9
> 
```


```r
 Next Exercise
script.R


1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
# Keyed data.table DT
DT <- data.table(A = letters[c(2, 1, 2, 3, 1, 2, 3)], 
                 B = c(5, 4, 1, 9, 8, 8, 6), 
                 C = 6:12, 
                 key= "A,B")
# Get the key of DT
key(DT)
# Row where A == "b" & B == 6
 DT[.("b",6)]
# Return the prevailing row
 DT[.("b",6), roll=TRUE]
# Return the nearest row
 DT[.("b",6), roll="nearest"]
 Submit Answer
R Console
Slides
> # Keyed data.table DT
> DT <- data.table(A = letters[c(2, 1, 2, 3, 1, 2, 3)], 
                 B = c(5, 4, 1, 9, 8, 8, 6), 
                 C = 6:12, 
                 key= "A,B")
> 
> # Get the key of DT
> key(DT)
[1] "A" "B"
> 
> # Row where A == "b" & B == 6
>  DT[.("b",6)]
   A B  C
1: b 6 NA
> 
> # Return the prevailing row
>  DT[.("b",6), roll=TRUE]
   A B C
1: b 6 6
> 
> # Return the nearest row
>  DT[.("b",6), roll="nearest"]
   A B C
1: b 6 6
> 
```



```r
 Next Exercise
script.R


1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
# Keyed data.table DT
DT <- data.table(A = letters[c(2, 1, 2, 3, 1, 2, 3)], 
                 B = c(5, 4, 1, 9, 8, 8, 6), 
                 C = 6:12, 
                 key= "A,B")
# Print out the sequence (-2):10 for the "b" group
DT[.("b",(-2):10)]
# Add code: carry the prevailing values forwards
DT[.("b",(-2):10),  roll=TRUE]
# Add code: carry the first observation backwards
DT[.("b",(-2):10), roll=TRUE, rollends=TRUE]
 Submit Answer
R Console
Slides
> # Keyed data.table DT
> DT <- data.table(A = letters[c(2, 1, 2, 3, 1, 2, 3)], 
                 B = c(5, 4, 1, 9, 8, 8, 6), 
                 C = 6:12, 
                 key= "A,B")
> 
> # Print out the sequence (-2):10 for the "b" group
> DT[.("b",(-2):10)]
    A  B  C
 1: b -2 NA
 2: b -1 NA
 3: b  0 NA
 4: b  1  8
 5: b  2 NA
 6: b  3 NA
 7: b  4 NA
 8: b  5  6
 9: b  6 NA
10: b  7 NA
11: b  8 11
12: b  9 NA
13: b 10 NA
> 
> # Add code: carry the prevailing values forwards
> DT[.("b",(-2):10),  roll=TRUE]
    A  B  C
 1: b -2 NA
 2: b -1 NA
 3: b  0 NA
 4: b  1  8
 5: b  2  8
 6: b  3  8
 7: b  4  8
 8: b  5  6
 9: b  6  6
10: b  7  6
11: b  8 11
12: b  9 11
13: b 10 11
> 
> # Add code: carry the first observation backwards
> DT[.("b",(-2):10), roll=TRUE, rollends=TRUE]
    A  B  C
 1: b -2  8
 2: b -1  8
 3: b  0  8
 4: b  1  8
 5: b  2  8
 6: b  3  8
 7: b  4  8
 8: b  5  6
 9: b  6  6
10: b  7  6
11: b  8 11
12: b  9 11
13: b 10 11
> 
```


```r

```


