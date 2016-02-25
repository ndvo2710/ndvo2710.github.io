---
layout: post
title: Data Exploration And Cleaning
excerpt: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2016-02-20
tags: [R, cleaning data, tutorial]
image:
  feature: sample-cleaning.jpg
  credit: 2bm.co.uk
  creditlink: http://www.2bm.co.uk/wp-content/uploads/2014/10/2bm-cleaning-banner-image-new.jpg
comments: true
---

<section id="table-of-contents" class="toc">
  <header>
    <h3>Overview</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->


# INTRODUCTION

Unless the data is small, _the presence of incorrect and inconsitent data is pervasive_. As a result, we should spend a lot of time cleaning data. This time is also, typically underestimated. 

By way of showing how data is messy in real life, I used for sale vehicle data which is harvested from Craiglist. Craiglist car for sale is where the the owners and dealers create a post for a vehicle providing information about model, year of the vehicle, fuel type, how many miles there are on the odometer. Here are the description of the variables in the data:

<br>

<sup> **id**  the (unique) identifier for the post </sup>

<sup> **title** a short text description for the post </sup>

<sup> **body**  the full free-form text for the post </sup>

<sup> **lat**, **long** the **longitude** and **latitude** associated with the post, i.e., that of the poster or of the location of the car, or both. </sup>

<sup> **posted**  the date the post was originally posted </sup>

<sup> **updated** the date of the most recent modification to the post </sup>

<sup> **drive** what type of drive the vehicle has, e.g., front-wheel drive. </sup>

<sup> **odometer**  the number of miles on the car's odometer </sup>

<sup> **type**  what type of vehicle is the post about, e.g., car, van, truck. </sup>

<sup> **header**  a shot description for the post, different from the body, and potentially different form the title. </sup>

<sup> **condition** a description of what condition the vehicle is in, e.g., excellent </sup>

<sup> **cylinders** the number of cylinders in the vehicle's engine. </sup>

<sup> **fuel**  the type of fuel the vehicle uses, e.g., gas </sup>

<sup> **size**  a categorical description of the size of the vehicle, e.g., compact, mid-size. </sup>

<sup> **transmission**  the transmission type of the vehicle, e.g., automatic, manual. </sup>

<sup> **byOwner** a logical value with TRUE indicating the vehicle is being sold by its owner, not a dealer </sup>

<sup> **city**  the city on whose bulletin board the post was submitted </sup>

<sup> **time**  the date and time when the post was posted </sup>

<sup> **description** a short description for the post </sup>

<sup> **location**  the user-supplied city/town for the post </sup>

<sup> **url** part of the URL for the post </sup>

<sup> **price** the price being sought for the vehicle </sup>

<sup> **year**  the year the car was manufactured </sup>

<sup> **maker** the name of the car manufacturer, e.g., ford, chevrolet. These are extracted from the header or title of the post. </sup>

<sup> **makerMethod** a number that identifies which method was used to identify the manufacturer </sup>


# SET UP


* Loading data into R
* Use **ls()** to list files. Data is named as `vehicle`

~~~r
load("~/Dropbox/Fall 2015/STAT 141/Assignment1/vehicles.rda")
ls()
[1] "vehicle"
~~~

* **Libraries** are used:

~~~r
library(lattice)
library(maps)
library(ggplot2)
library(gmodels)
library(RColorBrewer)
~~~


# DATA EXPLORATION AND CLEANING

First, we need to examine what is the **class** of `vehicle`

~~~r
> class(vehicle)
[1] "data.frame"
~~~

Since `vehicle` is a *data frame*, we need to know its dimension.

~~~r
> dim(vehicle)
[1] 34677    26
~~~

`vehicle` dataset contains *26 variables* and **34677 observations**.
Even though we have already known the decription of each variable above, we should carefully confirm the names of these variables and identify its class.

~~~r
> names(vehicle)
 [1] "id"           "title"        "body"         "lat"          "long"
 [6] "posted"       "updated"      "drive"        "odometer"     "type"
[11] "header"       "condition"    "cylinders"    "fuel"         "size"
[16] "transmission" "byOwner"      "city"         "time"         "description"
[21] "location"     "url"          "price"        "year"         "maker"
[26] "makerMethod"

> unlist( sapply(X = vehicle, FUN = class) )
          id        title         body          lat         long      posted1
 "character"  "character"  "character"    "numeric"    "numeric"    "POSIXct"
     posted2     updated1     updated2        drive     odometer         type
    "POSIXt"    "POSIXct"     "POSIXt"     "factor"    "integer"     "factor"
      header    condition    cylinders         fuel         size transmission
 "character"     "factor"    "integer"     "factor"     "factor"     "factor"
     byOwner         city        time1        time2  description     location
   "logical"     "factor"    "POSIXct"     "POSIXt"  "character"  "character"
         url        price         year        maker  makerMethod
 "character"    "integer"    "integer"  "character"    "numeric"
~~~

In this dataset, **price** is the most important variable to me. To get an in-depth look at the **price variable**, we should see the <u>distribution</u> of vehicle price. 

<figure>
  <a href="/images/data-cleaning-image/density-plot1.png" title="Distribution of Vehicle Prices"><img src="/images/data-cleaning-image/density-plot1.png"></a>
  <figcaption><center><b><i>Figure.</i> <u>Distribution Plot of Vehicle price</u></b></center></figcaption>
</figure>

=> <b>densityplot</b> indicates that there are a few extreme outliers in the right tail. Let's check them out.

~~~r
> tail( sort(vehicle$price), 50)
 [1]     95593     96590     97000     97500     97911     98000     99560
 [8]     99999    100000    100000    100000    100000    104800    105000
[15]    105500    107000    112000    116100    116491    120000    122950
[22]    123981    125000    129950    129990    138500    139000    139950
[29]    143000    143000    143950    147000    149890    149995    150000
[36]    152900    159000    169000    177588    202455    240000    286763
[43]    359000    400000    559500    569500   9999999  30002500 600030000
[50] 600030000
~~~

=> Obviously, the `price of a car > 9999999` is quite <u><b>unlikely</b></u>. We should look further to the `price of car from $100000 onward`.

~~~r
> idx = which( vehicle$price >=  100000 & !is.na(vehicle$price))
> length( idx )
[1] 42
~~~
There are <u>42 cars</u> <b> > $100,000</b> . Let's look at the actual data of these cars: 

~~~r
> vehicle[ idx, c("header", "price") ]
                                                      header     price
posted698                                        2008 BMW X5    177588
posted1460                          2010 CHEVROLET SILVERADO    359000
posted12461                                 2000 Mack RD688S    100000
posted22491                                 1969 Pontiac GTO 600030000
posted22621                                        2013 Ford    150000
posted23881                                 1969 Pontiac GTO 600030000
posted24081                           2003 lincoln navigator    100000
posted21402                            2009 CHEVROLET IMPALA    559500
posted21422                       2007 CHEVROLET MONTE CARLO    569500
posted6903                            2002 Caddy Seville sls  30002500
posted16934                                   2013 Isuzu NRR    129990
posted7225                           1967 chevrolet corvette    105500
posted13245                                 2010 ford fusion    105000
posted16005                                2001 Honda Accord   9999999
posted9976                               2004 Toyota Corolla    286763
posted11066                        2009 Lamborghini Gallardo    129950
posted18506                          2009 Mercedes-Benz SL65    139950
posted18546                           2013 Mercedes-Benz G63    122950
posted214110                           2011 Bentley Mulsanne    143950
posted6747                                    2004 Lexus 470    169000
posted20867                          2015 mercedes-benz s550    107000
posted5038   2012 Mercedes-Benz SLS AMG 2dr Roadster SLS AMG    149890
posted23788                                     2006 FORD GT    400000
posted12630                          2014 ferrari 458 italia    240000
posted37310                      2014 Audi RS 7 4.0T quattro    116491
posted163710                     2015 Mercedes-Benz  S-Class    143000
posted212510                               2014 Porsche  911    104800
posted231114                               2015 Porsche  911    152900
posted181511                                1965 porsche 911    100000
posted194311                              2005 TOYOTA AVALON    112000
posted220311                                2011 toyota rav4    159000
posted106214                   2014 Land Rover Range 5.0L V8    123981
posted121313                                2016 porsche 911    202455
posted129214                2007 Lamborghini Gallardo Spyder    149995
posted191712                             2015 Hyundai Sonata    138500
posted222912                          2015 Porsche  Panamera    116100
posted231215                     2015 Mercedes-Benz  S-Class    143000
posted95215                1988 porsche 911 Carrera Targa TL    120000
posted112215                                1976 Porsche 930    139000
posted212613                                     1941 willys    125000
posted238613                                2015 Porsche GT3    147000
posted245512                               1961 Maserati 151    100000
~~~

There are some very expensive cars such as Mercedes-Benz G63 AMG, Bentley Mulsanne, Maserati 3500 GT ... which are probably more than $100,000. However, there are some abnormal cars such as :

* <u><b>“2015 Hyundai Sonata Call/SMS 650.445.0890 (12) - $138500”</b></u> => has an extra zero at the end.

* <u><b>“2007 CHEVROLET MONTE CARLO LT - Easy Financing! Any Credit Auto Loans! BHPH! - $569500”</b></u> => has two extra zeros at the end.



Question 3
**<u> What is the average price of all the vehicles? the median price? and the deciles? Displays these on a plot of the distribution of vehicle prices.</u>**

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

#### Question 4
**<u> What are the different categories of vehicles, i.e. the type variable/column? What is the proportion for each category ?</u>**

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

#### Question 5
**<u> Display the relationship between fuel type and vehicle type. Does this depend on transmission type?</u>**

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

#### Question 6
**<u> How many different cities are represented in the dataset?</u>**

 There are 7 different cities: boston ,  chicago  ,denver  , lasvegas ,nyc    ,  sac  ,    sfbay 
 
~~~r
> unique(vposts$city)
[1] boston   chicago  denver   lasvegas nyc      sac      sfbay   

> length(unique(vposts$city))
[1] 7
~~~

**<u>#### Question 7: Visually display how the number/proportion of "for sale by owner" and "for sale by dealer" varies across city?</u>**

~~~r
> table(vposts$byOwner, vposts$city)
       
        boston chicago denver lasvegas  nyc  sac sfbay
  FALSE   2491    2491   2492     2489 2495 2483  2475
  TRUE    2467    2395   2487     2474 2488 2483  2467
~~~

**<u>#### Question 8: What is the largest price for a vehicle in this data set? Examine this and fix the value. Now examine the new highest value for price.</u>**

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

**<u>#### Question 9: What are the three most common makes of cars in each city for "sale by owner" and for "sale by dealer"? Are they similar or quite different?</u>**

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

**<u>#### Question 10: Visually compare the distribution of the age of cars for different cities and for "sale by owner" and "sale by dealer". Provide an interpretation of the plots, i.e., what are the key conclusions and insights?</u>**

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

**<u>#### Question 11: Plot the locations of the posts on a map? What do you notice?</u>**

~~~r
library(maps)
map('usa')
points(vposts$long, vposts$lat, col = "red", pch = ".")
~~~

![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/screenshot%2011.png)

**<u>#### Question 12: Summarize the distribution of fuel type, drive, transmission, and vehicle type. Find a good way to display this information.</u>**

Using the scatterplot of drive vs. vehicle type for each combination of fuel and transmission

~~~r
library(ggplot2)
qplot(drive, type, data = vposts, shape = transmission, color= transmission, facets = fuel~transmission, na.rm= TRUE)
~~~

![![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg)](https://dl.dropboxusercontent.com/u/27868566/screenshot%2010.png)




**<u>#### Question 13: Plot odometer reading and age of car? Is there a relationship? Similarly, plot odometer reading and price? Interpret the result(s). Are odometer reading and age of car related?</u>**

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

**<u>#### Question 14: Identify the "old" cars. What manufacturers made these? What is the price distribution for these?</u>**

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


**<u>#### Question 15: I have omitted one important variable in this data set. What do you think it is? Can we derive this from the other variables? If so, sketch possible ideas as to how we would compute this variable.</u>**

I find that we need a phone variable in this data set. We can extract the phone number from vposts$body. However, I haven't found the answer yet. I need more time to learn more about grep, sub, str_extract

**<u>#### Question 16: Display how condition and odometer are related. Also how condition and price are related. And condition and age of the car. Provide a brief interpretation of what you find.</u>**

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

######### Question 1
dim(vposts)

######### Question 2
names(vposts)
sapply(vposts,class)
~~~

~~~
######### Question 3
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
######### Question 4
unique(vposts$type)
prop_type = 100* table(vposts$type) /sum(table(vposts$type))
prop_type
~~~

~~~
######### Question 5
fv = with(vposts, table(fuel, type))
fv
with(vposts,plot(fuel,type,pch =".", main= "Relationship between Fuel and Vehicle Type", xlab = "Fuel", ylab = "Vehicle Type"))
ftt <- with(vposts,data.frame(fuel,type,transmission))
pairs(ftt)
~~~

~~~
######### Question 6 unique(vposts$city)
length(unique(vposts$city))

######### Question 7
table(vposts$byOwner, vposts$city)

######### Question 8
p = vposts$price
p[p == max(p,na.rm=TRUE) & !is.na(p)]
p[p == max(p,na.rm=TRUE) & !is.na(p)] = NA
p[p == max(p,na.rm=TRUE) & !is.na(p)] = NA
p[p == max(p,na.rm=TRUE) & !is.na(p)] = NA
p[p == max(p,na.rm=TRUE) & !is.na(p)] = NA
p[p == max(p,na.rm=TRUE) & !is.na(p)]
~~~

~~~
######### Question 9
owner = vposts[vposts$byOwner == TRUE,]
tapply(owner$maker, owner$city, function(x) sort(table(x),decreasing = TRUE)[1:3])


dealer = vposts[vposts$byOwner == FALSE,]
tapply(dealer$maker, dealer$city, function(x) sort(table(x),decreasing = TRUE)[1:3])
~~~

~~~
######### Question 10
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
######### Question 11
library(maps)
map('usa')
points(vposts$long, vposts$lat, col = "red", pch = ".")

######### Question 12
library(ggplot2)
qplot(drive, type, data = vposts, shape = transmission, color= transmission, facets = fuel~transmission, na.rm= TRUE)
~~~

~~~
######### Question 13
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
######### Question 14
vposts$maker = factor(vposts$maker)
levels(vposts$maker)
summary(vposts[which(vposts$odometer > 200000 | vposts$age > 20),"maker"])
hist(vposts[which(vposts$odometer > 200000 | vposts$age > 20),"price"])

######### Question 15
~~~

~~~
######### Question 16
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



