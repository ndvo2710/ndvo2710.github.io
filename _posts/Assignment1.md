#STAT 141 

##Assignment 1
###PART I

* Loading data into R
* Use **ls()** to list files. Data is named as **vposts**

~~~r
> load("~/Dropbox/Fall 2015/STAT 141/Assignment1/vehicles.rda")
> ls()
[1] "vposts"
~~~  
  
**<u>Question 1: How many observations are there in the data set?</u>**

~~~r
> dim(vposts)
[1] 34677    26
~~~
I use  **dim** to check  the <u>*dimension*</u> of the data set.
***_Answer:_*** The number of observation is **34677** ( there are 26 variables in **vposts** data frame)

**<u>Question 2: What are the names of the variables? and what is the class of each variable?</u>**  
***_Answer:_*** 

* These are the names of variables: **id, title, body, lat, long, posted, updated, drive, odometer, type header, condition cylinders, fuel, size, transmission, byOwner, city, time, description, location, url, price, year, maker, makerMethod.**

~~~r
> names(vposts)
 [1] "id"           "title"        "body"         "lat"          "long"        
 [6] "posted"       "updated"      "drive"        "odometer"     "type"        
[11] "header"       "condition"    "cylinders"    "fuel"         "size"        
[16] "transmission" "byOwner"      "city"         "time"         "description" 
[21] "location"     "url"          "price"        "year"         "maker"       
[26] "makerMethod" 
~~~
 
* Consider each column as a list and use **sapply** to find the **class** of each variable

~~~r
> sapply(vposts,class)
$id
[1] "character"
$title
[1] "character"
$body
[1] "character"
...
~~~

Variables | Class |
----------|-------|
id | character |
title| character |
body| character |
lat| numeric |
long| numeric |
posted| POSIXct POSIXt  |
updated| POSIXct POSIXt  |
drive| factor |
odometer| integer |
type| factor |
header| character |
condition| factor |
cylinders| integer |
fuel| factor |
size| factor |
transmission| factor |
byOwner| logical |
city| factor |
time| POSIXct POSIXt  |
description| character |
location| character |
url| character |
price| integer |
year| integer |
maker| character |
makerMethod| numeric |

**<u>Question 3: What is the average price of all the vehicles? the median price? and the deciles? Displays these on a plot of the distribution of vehicle prices.</u>**

* The average price of all vehicles is **$49449.9**
* The median price of all vehicles is **$6700**
* The deciles are:


Decile | 0% | 10% | 20% | 30% | 40% | 50% | 60% | 70% | 80% | 90% | 100% | 
-------|----|-----|-----|-----|-----|-----|-----|-----|---|-----|-----|
**Price** | 1 | 1200 | 2499 | 3500 | 4995 | 6700 | 8900 | 11888 | 15490 | 21997 | 600030000 |

~~~r
> mean(vposts$price, na.rm = TRUE)
[1] 49449.9
> median(vposts$price, na.rm= TRUE)
[1] 6700
> quantile(vposts$price, prob = seq(0,1 , length =11), na.rm = TRUE)
       0%       10%       20%       30%       40%       50%       60%       70% 
        1      1200      2499      3500      4995      6700      8900     11888 
      80%       90%      100% 
    15490     21997 600030000 
~~~

* Use histogram to plot the distribution of vehicle prices:
    * Remove outliers of vposts$price data
    * Draw a histogram of the new data.

![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/Stat141_Assignment1_question3.png)

~~~r
> qt <- quantile(p, na.rm = TRUE)
> qt
       0%       25%       50%       75%      100% 
        1      2995      6700     13500 600030000 
> iqr <- qt[[4]] - qt[[2]]
> lb <- qt[[2]] -1.5*iqr
> ub <- qt[[4]] +1.5*iqr
> p_removed_outliers = p[p>lb & p<ub]
> hist(p_removed_outliers,main = "Historgram of Vehicle Price Distribution", xlab = "Price",col ="lightgreen")
~~~

**<u>Question 4: What are the different categories of vehicles, i.e. the type variable/column? What is the proportion for each category ?</u>**

_**Vehicle types:**_ "bus" ,"convertible", "coupe",       "hatchback",   "mini-van",    "offroad",     "other"       ,"pickup"      ,"sedan"      , "SUV"         ,"truck"       ,"van"        , "wagon" 

~~~r
> unique(vposts$type)
 [1] coupe       SUV         sedan       hatchback   wagon       van        
 [7] <NA>        convertible pickup      truck       mini-van    other      
[13] bus         offroad 

> prop_type = 100* table(vposts$type) /sum(table(vposts$type))
> prop_type

        bus convertible       coupe   hatchback    mini-van     offroad 
  0.1171147   3.7583178   8.6558424   4.3598616   2.4114985   0.3513442 
      other      pickup       sedan         SUV       truck         van 
  3.5453820   4.8389673  37.4767101  22.4168219   6.3987224   2.6989619 
      wagon 
  2.9704552 
~~~

**<u>Question 5: Display the relationship between fuel type and vehicle type. Does this depend on transmission type?</u>**

~~~r
> fv = with(vposts, table(fuel, type))
> fv
          type
fuel        bus convertible coupe hatchback mini-van offroad other pickup sedan
  diesel     10           0     3         7        0       1    24    109    52
  electric    0           0     2        20        0       0     0      0     9
  gas        12         657  1518       625      411      65   307    774  6166
  hybrid      0           0     1        93        1       0     2      0    78
  other       0           8    33        22        3       0   333     22   145
          type
fuel        SUV truck  van wagon
  diesel     38   325   54     9
  electric    0     1    0     0
  gas      3495   710  409   481
  hybrid     28     0    1     3
  other     171    23   22    20
~~~

~~~r
> with(vposts,plot(fuel,type,pch =".", main= "Relationship between Fuel and Vehicle Type",
+  xlab = "Fuel", ylab = "Vehicle Type"))
~~~

![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/Stat141%20Assignment1_question5.png)


~~~r
> ftt <- with(vposts,data.frame(fuel,type,transmission))
> pairs(ftt)
~~~
![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/Asgn1q5b.png)

**<u>Question 6: How many different cities are represented in the dataset?</u>**

 There are 7 different cities: boston ,  chicago  ,denver  , lasvegas ,nyc    ,  sac  ,    sfbay 
 
~~~r
> unique(vposts$city)
[1] boston   chicago  denver   lasvegas nyc      sac      sfbay   

> length(unique(vposts$city))
[1] 7
~~~

**<u>Question 7: Visually display how the number/proportion of "for sale by owner" and "for sale by dealer" varies across city?</u>**

~~~r
> table(vposts$byOwner, vposts$city)
       
        boston chicago denver lasvegas  nyc  sac sfbay
  FALSE   2491    2491   2492     2489 2495 2483  2475
  TRUE    2467    2395   2487     2474 2488 2483  2467
~~~

**<u>Question 8: What is the largest price for a vehicle in this data set? Examine this and fix the value. Now examine the new highest value for price.</u>**

~~~r
> p = vposts$price
> p[p == max(p,na.rm=TRUE) & !is.na(p)]
[1] 600030000 600030000
~~~

~~~r
> qt <- quantile(p, na.rm = TRUE)
> qt
       0%       25%       50%       75%      100% 
        1      2995      6700     13500 600030000 
> iqr <- qt[[4]] - qt[[2]]
> lb <- qt[[2]] -1.5*iqr
> ub <- qt[[4]] +1.5*iqr

> p[p == max(p,na.rm=TRUE) & !is.na(p)]
[1] 600030000 600030000
> p[p == max(p,na.rm=TRUE) & !is.na(p)] = NA
> p[p == max(p,na.rm=TRUE) & !is.na(p)] = NA
> p[p == max(p,na.rm=TRUE) & !is.na(p)] = NA
> p[p == max(p,na.rm=TRUE) & !is.na(p)] = NA
> p[p == max(p,na.rm=TRUE) & !is.na(p)]
[1] 30002500
~~~

**<u>Question 9: What are the three most common makes of cars in each city for "sale by owner" and for "sale by dealer"? Are they similar or quite different?</u>**

* By owner

~~~r
> owner = vposts[vposts$byOwner == TRUE,]
> tapply(owner$maker, owner$city, function(x) sort(table(x),decreasing = TRUE)[1:3])
$boston
     ford     honda chevrolet 
      353       263       226 
$chicago
chevrolet      ford     honda 
      365       331       180 
~~~
~~~r
$denver
     ford chevrolet    toyota 
      378       313       191 
$lasvegas
     ford chevrolet    toyota 
      394       306       193 
$nyc
nissan toyota  honda 
   308    274    260 
$sac
   toyota      ford chevrolet 
      340       305       299 
$sfbay
toyota  honda   ford 
   332    322    257 76 
~~~

* By dealer:

~~~r
> dealer = vposts[vposts$byOwner == FALSE,]
> tapply(dealer$maker, dealer$city, function(x) sort(table(x),decreasing = TRUE)[1:3])
$boston
     ford    toyota chevrolet 
      333       288       215 
$chicago
chevrolet      ford    nissan 
      305       305       208
$denver
     ford chevrolet     dodge 
      313       291       210 
$lasvegas
     ford    nissan chevrolet 
      307       249       238 
~~~
~~~r
$nyc
nissan toyota  honda 
   328    238    220 
$sac
     ford    toyota chevrolet 
      337       273       206 

$sfbay
toyota   ford    bmw 
   269    245    227
~~~

**<u>Question 10: Visually compare the distribution of the age of cars for different cities and for "sale by owner" and "sale by dealer". Provide an interpretation of the plots, i.e., what are the key conclusions and insights?</u>**

**_By Owner:_**

~~~r
which(owner$year <1900) #=3435
owner = owner[-3435,]
par(mfrow=c(4,2))
histogram(~(2016-year) |city, owner, xlab = "Age of Cars", 
main= "Histograms of Distribution of the age of cars for different cities")
~~~

![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/screenshot%205.png)

**_By Dealers:_**

~~~r
> par(mfrow=c(4,2))
> library(lattice)
> histogram(~(2016-year) |city, dealer, xlab = "Age of Cars", 
+ main= "Histograms of Distribution of the age of cars for different cities")
~~~

![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/screenshot%206.png)

**<u>Question 11: Plot the locations of the posts on a map? What do you notice?</u>**

~~~r
library(maps)
map('usa')
points(vposts$long, vposts$lat, col = "red", pch = ".")
~~~

![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/screenshot%2011.png)

**<u>Question 12: Summarize the distribution of fuel type, drive, transmission, and vehicle type. Find a good way to display this information.</u>**

Using the scatterplot of drive vs. vehicle type for each combination of fuel and transmission

~~~r
library(ggplot2)
qplot(drive, type, data = vposts, shape = transmission, color= transmission, facets = fuel~transmission, na.rm= TRUE)
~~~

![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/screenshot%2010.png)




**<u>Question 13: Plot odometer reading and age of car? Is there a relationship? Similarly, plot odometer reading and price? Interpret the result(s). Are odometer reading and age of car related?</u>**

~~~r
> vposts$age = 2015-vposts$year
> summary(vposts$odometer)
     Min.   1st Qu.    Median      Mean   3rd Qu.      Max.      NA's 
0.000e+00 4.053e+04 9.051e+04 1.509e+05 1.300e+05 1.235e+09     10421 
> qt_odometer = quantile(vposts$odometer, na.rm = TRUE)
> vposts[which(vposts$odometer > (qt_odometer[[4]]+ 1.5*(qt_odometer[[4]]-qt_odometer[[1]]))),"odometer"]= NA
> smoothScatter(vposts$age, vposts$odometer)
~~~

![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/screenshot%2012.png)

~~~r
> summary(vposts$odometer)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
      0   40250   90000   89460  129700  321000   10550 
> smoothScatter(vposts$age, vposts$odometer)
> summary(vposts$price)
     Min.   1st Qu.    Median      Mean   3rd Qu.      Max.      NA's 
        1      2995      6700     49450     13500 600000000      3328 
> qt_price = quantile(vposts$price, na.rm=TRUE)
> vposts[which(vposts$price > (qt_price[[4]]+ 1.5*(qt_price[[4]]-qt_price[[1]]))),"price"]= NA
> summary(vposts$price)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
      1    2900    6300    8565   12800   33720    4329 
~~~

![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/screenshot%2013.png)

**<u>Question 14: Identify the "old" cars. What manufacturers made these? What is the price distribution for these?</u>**

~~~r
> vposts$maker = factor(vposts$maker)
> summary(vposts[which(vposts$odometer > 200000 | vposts$age > 20),"maker"])
          acura      alfa romeo             amc    aston martin            audi         bentley 
             51               8               6               0               7               1 
            bmw        bricklin         bugatti           buick        cadillac       chevrolet 
             58               1               2              60              89             663 
       chrysler          daewoo          datsun          desoto           dodge           eagle 
             32               0              15               2             121               3
~~~
~~~r              
        ferrari            fiat            ford    freightliner             geo             gmc 
              0              10             624               3              16             100 
harley davidson           honda          hudson          hummer         hyundai        infiniti 
              2             203               5               1              11              14
~~~
~~~r 
  international           isuzu          jaguar            jeep             kia     lamborghini 
             35              10              11              84               4               1 
     land rover            leaf           lexus         lincoln            mack        maserati 
              2               0              47              46               3               1 
          mazda        mercedes         mercury              mg            mini      mitsubishi 
             50              92              36              18               0              18
~~~
~~~r              
         nissan      oldsmobile       peterbilt         peugeot        plymouth         pontiac 
             77              46               2               1              38              91 
        porsche     rolls royce            saab          saturn           scion          shelby 
             32               9               5               7               0               7 
          smart      studebaker          subaru          suzuki           tesla          toyota 
              0               5              25               7               0             286 
        triumph      volkswagen           volvo          willys         yerfdog             zap 
              7              96              20              12               0               0 
           NA's 
             92
> hist(vposts[which(vposts$odometer > 200000 | vposts$age > 20),"price"], main= "Price 
Distribution for Old Cars", xlab="Price", col ="red")
~~~

![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/screenshot%2014.png)


**<u>Question 15: I have omitted one important variable in this data set. What do you think it is? Can we derive this from the other variables? If so, sketch possible ideas as to how we would compute this variable.</u>**

I find that we need a phone variable in this data set. We can extract the phone number from vposts$body. However, I haven't found the answer yet. I need more time to learn more about grep, sub, str_extract

**<u>Question 16: Display how condition and odometer are related. Also how condition and price are related. And condition and age of the car. Provide a brief interpretation of what you find.</u>**

~~~r
> vposts$newcondition = NA
> new_index = which(vposts$condition == "excellent" | vposts$condition == "like new" | vposts$condition == "superb original" | vposts$condition == "new" | vposts$condition == "mint" )
> good_index = which(vposts$condition == "nice" | vposts$condition== "nice teuck" | vposts$condition == "fair" | vposts$condition == "good")
> vposts[which(!is.na(vposts$condition)), "newcondition"] = "Used/Salvage"
> vposts[new_index,"newcondition"] = "New"
> vposts[good_index,"newcondition"] = "Good"
> vposts$newcondition = factor(vposts$newcondition)
> with(vposts, plot(newcondition, odometer, pch = ".", col = c("Red","Green","White"), 
main = "Relationship between Condition and Odometer", xlab = "Condition", ylab ="Odometer"))
~~~

![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/screenshot%2015.png)


~~~r
> with(vposts, plot(newcondition, price, pch = ".", col = c("Red","Green","White"),
main = "Relationship between Condition and Price", xlab = "Condition", ylab ="Price"))
~~~

![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/screenshot%2016.png)

~~~r
with(vposts, plot(newcondition, age, pch = ".", col = c("Red","Green","White"), main = "Relationship between Condition and Age", xlab = "Condition", ylab ="Age"))
~~~

![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/screenshot%2017.png)


**_*Comment:*_** I had found that it is pretty strange with these 3 plots since there is an inverse relationship between condition and odometer, between condition and Price, or between condition and age.


##Appendix(R Code):

This is my R code in case that the text format of the code below is not right:



~~~r
load("~/Dropbox/Fall 2015/STAT 141/Assignment1/vehicles.rda")
library(ggplot2)

#####In some questions, I have cleaned the data of some variables, so I used that result to do the next question.

#####Question 1
dim(vposts)

#####Question 2
names(vposts)
sapply(vposts,class)
~~~

~~~
#####Question 3
mean(vposts$price, na.rm = TRUE)
median(vposts$price, na.rm= TRUE)
quantile(vposts$price, prob = seq(0,1 , length =11), na.rm = TRUE)
p = vposts$price
qt <- quantile(p, na.rm = TRUE)
qt
iqr <- qt[[4]] - qt[[2]]
lb <- qt[[2]] -1.5*iqr
ub <- qt[[4]] +1.5*iqr
~~~

~~~
#####Question 4
unique(vposts$type)
prop_type = 100* table(vposts$type) /sum(table(vposts$type))
prop_type
~~~

~~~
#####Question 5
fv = with(vposts, table(fuel, type))
fv
with(vposts,plot(fuel,type,pch =".", main= "Relationship between Fuel and Vehicle Type", xlab = "Fuel", ylab = "Vehicle Type"))
ftt <- with(vposts,data.frame(fuel,type,transmission))
pairs(ftt)
~~~

~~~
#####Question 6 unique(vposts$city)
length(unique(vposts$city))

#####Question 7
table(vposts$byOwner, vposts$city)

#####Question 8
p = vposts$price
p[p == max(p,na.rm=TRUE) & !is.na(p)]
p[p == max(p,na.rm=TRUE) & !is.na(p)] = NA
p[p == max(p,na.rm=TRUE) & !is.na(p)] = NA
p[p == max(p,na.rm=TRUE) & !is.na(p)] = NA
p[p == max(p,na.rm=TRUE) & !is.na(p)] = NA
p[p == max(p,na.rm=TRUE) & !is.na(p)]
~~~

~~~
#####Question 9
owner = vposts[vposts$byOwner == TRUE,]
tapply(owner$maker, owner$city, function(x) sort(table(x),decreasing = TRUE)[1:3])


dealer = vposts[vposts$byOwner == FALSE,]
tapply(dealer$maker, dealer$city, function(x) sort(table(x),decreasing = TRUE)[1:3])
~~~

~~~
#####Question 10
which(owner$year <1900) #=3435
owner = owner[-3435,]
library(lattice)
histogram(~(2016-year) |city, owner, xlab = "Age of Cars", 
main= "Histograms of Distribution of the age of cars for different cities")

which(dealer$year <1900) # integer(0) means no wrong data
library(lattice)
histogram(~(2016-year) |city, dealer, xlab = "Age of Cars", 
main= "Histograms of Distribution of the age of cars for different cities")
~~~

~~~
#####Question 11
library(maps)
map('usa')
points(vposts$long, vposts$lat, col = "red", pch = ".")

#####Question 12
library(ggplot2)
qplot(drive, type, data = vposts, shape = transmission, color= transmission, facets = fuel~transmission, na.rm= TRUE)
~~~

~~~
#####Question 13
vposts$age = 2015-vposts$year
vposts[which(vposts$age < 0 | vposts$age > 100 ),"age"] = NA
summary(vposts$odometer)
qt_odometer = quantile(vposts$odometer, na.rm = TRUE) # Show quantile of odometer
vposts[which(vposts$odometer > (qt_odometer[[4]]+ 1.5*(qt_odometer[[4]]-qt_odometer[[1]]))),"odometer"]= NA
smoothScatter(vposts$age, vposts$odometer)

summary(vposts$price)
qt_price = quantile(vposts$price, na.rm=TRUE)
vposts[which(vposts$price > (qt_price[[4]]+ 1.5*(qt_price[[4]]-qt_price[[1]]))),"price"]= NA
smoothScatter(vposts$price,vposts$age)
~~~

~~~
#####Question 14
vposts$maker = factor(vposts$maker)
levels(vposts$maker)
summary(vposts[which(vposts$odometer > 200000 | vposts$age > 20),"maker"])
hist(vposts[which(vposts$odometer > 200000 | vposts$age > 20),"price"])

#####Question 15
~~~

~~~
#####Question 16
vposts$newcondition = NA
new_index = which(vposts$condition == "excellent" | vposts$condition == "like new" | vposts$condition == "superb original" | vposts$condition == "new" | vposts$condition == "mint" )
good_index = which(vposts$condition == "nice" | vposts$condition== "nice teuck" | vposts$condition == "fair" | vposts$condition == "good")

vposts[which(!is.na(vposts$condition)), "newcondition"] = "Used/Salvage"
vposts[new_index,"newcondition"] = "New"
vposts[good_index,"newcondition"] = "Good"
vposts$newcondition = factor(vposts$newcondition)
with(vposts, plot(newcondition, odometer, pch = ".", col = c("Red","Green","White"), main = "Relationship between Condition and Odometer", xlab = "Condition", ylab ="Odometer"))
~~~

~~~
with(vposts, plot(newcondition, price, pch = ".", col = c("Red","Green","White"), main = "Relationship between Condition and Price", xlab = "Condition", ylab ="Price"))

with(vposts, plot(newcondition, age, pch = ".", col = c("Red","Green","White"), main = "Relationship between Condition and Age", xlab = "Condition", ylab ="Age"))

~~~



