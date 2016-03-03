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
comments: True
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

~~~r
densityplot(vehicle$price, main = "Price", xlab = "Price")
~~~

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

=> Obviously, the `price of a car > 9999999` is quite <u><b>unlikely</b></u>. Let's take a look at them.

~~~r
> idx = which( vehicle$price >=  9999999 & !is.na(vehicle$price) )
> vehicle[ idx, c("header", "price", "maker", "year") ]
                            header     price    maker year
posted22491       1969 Pontiac GTO 600030000  pontiac 1969
posted23881       1969 Pontiac GTO 600030000  pontiac 1969
posted6903  2002 Caddy Seville sls  30002500 cadillac 2002
posted16005      2001 Honda Accord   9999999    honda 2001
~~~

It is realized that those are <b>pretty old cars</b> => Their high prices are probably __not legitimate__. Let's check and fix them:
  
  * With <b>Pontiac GTO</b> :

~~~r
    > idx = ( vehicle$maker == "pontiac" & vehicle$year %in% c(1968, 1969) &
+ vehicle$price < 9999999 & vehicle$price > 1 &
+ grepl(pattern = "GTO", x = vehicle$header, ignore.case = TRUE) &
+ !is.na(vehicle$maker) & !is.na(vehicle$price) & !is.na(vehicle$header) )
> dat = vehicle[ idx, c("header", "price", "maker", "year") ]
> dat[ order(dat$price), ]
                       header price   maker year
posted4991   1968 pontiac gto 15995 pontiac 1968
posted5371   1968 pontiac gto 15995 pontiac 1968
posted231214 1968 Pontiac GTO 24500 pontiac 1968
posted16497  1969 Pontiac GTO 25000 pontiac 1969
posted201111 1969 Pontiac GTO 25000 pontiac 1969
posted7355   1968 pontiac gto 30000 pontiac 1968
posted16701  1968 Pontiac gto 38500 pontiac 1968
posted40911          1968 GTO 38500 pontiac 1968
> round( mean(vehicle$price[idx]), digits = -3)
27000
~~~

I looked at the price of all <b>Pontiac GTO of the same year</b>. These prices <i>varies from $15,995 to $38,500</i>. => <b><u>Fix: Change the price to the avarage price</u></b> of all Pontiac GTO of the same year.

    ~~~r
    > vehicle$price[vehicle$price == 600030000 & !is.na(vehicle$price)] = 27000
    ~~~

  * <u><b>Comment: <i>We can do the same with`2002 Seville sls` or `2001 Honda Accord`. However they are just a few entries above `9999999` => We will spent tons of time doing this.</i></b></u>. <b><u>Is there any other solution for this?</u></b>

=> I will create a R function that could help us solve this problem automatically. 

Before doing that, Let's look at `price of car from $100000 onward`. 

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

> shortlist = vehicle[ idx, c("header", "price", "maker", "year") ]
~~~

There are some very expensive cars such as Mercedes-Benz G63 AMG, Bentley Mulsanne, Maserati 3500 GT ... which are probably more than $100,000. However, there are some abnormal cars such as :

* <u><b>“2015 Hyundai Sonata Call/SMS 650.445.0890 (12) - $138500”</b></u> => has an extra zero at the end.

* <u><b>“2007 CHEVROLET MONTE CARLO LT - Easy Financing! Any Credit Auto Loans! BHPH! - $569500”</b></u> => has two extra zeros at the end.

## Clean abnormal price of vehicle by sapply + function()

~~~r
filter_high_price = function(maker,year,header,price){
    idx = (vehicle$maker == maker & vehicle$year %in% c(year, year+1) &
          vehicle$price < 100000 & vehicle$price > 1 &
          grepl(pattern = gsub(maker,"",gsub("\\d+\\s","",header), ignore.case=TRUE),x = vehicle$header, ignore.case = TRUE) &
          !is.na(vehicle$maker) & !is.na(vehicle$price) & !is.na(vehicle$header))
  if(length(vehicle$price[idx])!= 0){
    newPrice = round( mean(vehicle$price[idx]), digits = -3)
    if(price > 1.5*newPrice) vehicle$price[vehicle$price == price & !is.na(vehicle$price)] <<- newPrice #return to global environment
  }
}

sapply(1:length(shortlist$maker), function(i) filter_high_price(shortlist$maker[i],shortlist$year[i],shortlist$header[i], shortlist$price[i]))
[[1]]
[1] 19000

[[2]]
[1] 26000
....
~~~

Let's check whether the shortlist get shortened

~~~r
> idx = which( vehicle$price >=  100000 & !is.na(vehicle$price) )
> vehicle[ idx, c("header", "price") ]
                                                      header  price
posted16934                                   2013 Isuzu NRR 129990
posted11066                        2009 Lamborghini Gallardo 129950
posted18506                          2009 Mercedes-Benz SL65 139950
posted18546                           2013 Mercedes-Benz G63 122950
posted214110                           2011 Bentley Mulsanne 143950
posted20867                          2015 mercedes-benz s550 107000
posted5038   2012 Mercedes-Benz SLS AMG 2dr Roadster SLS AMG 149890
posted12630                          2014 ferrari 458 italia 240000
posted37310                      2014 Audi RS 7 4.0T quattro 116491
posted163710                     2015 Mercedes-Benz  S-Class 143000
posted212510                               2014 Porsche  911 104800
posted231114                               2015 Porsche  911 152900
posted106214                   2014 Land Rover Range 5.0L V8 123981
posted129214                2007 Lamborghini Gallardo Spyder 149995
posted222912                          2015 Porsche  Panamera 116100
posted231215                     2015 Mercedes-Benz  S-Class 143000
posted95215                1988 porsche 911 Carrera Targa TL 120000
posted112215                                1976 Porsche 930 139000
posted238613                                2015 Porsche GT3 147000
~~~

Yup! 

Let's look at the density plot again. 

~~~r
> densityplot(vehicle$price, main = "Price", xlab = "Price")
~~~

<figure>
  <a href="/images/data-cleaning-image/density-plot2.png" title="Distribution of Vehicle Prices"><img src="/images/data-cleaning-image/density-plot2.png"></a>
  <figcaption><center><b><i>Figure.</i> <u>Distribution Plot of Vehicle price</u></b></center></figcaption>
</figure>

Much better! But we also note several other problems like lots of missing prices in general. These could be corrected one-by- one perhaps.
There are also lots of cars for sale at the price of $1. This is a common advertising tactic to post for the minimum price since most people sort prices lowest-to-highest and thus these ads get seen at the top more often. Most of these are misleading ads by dealers, some are for car parts, some are offers for car financing. 

~~~r
> sum( is.na(vehicle$price) )
[1] 3328
> sum( vehicle$price == 1 & !is.na(vehicle$price) )
[1] 612
~~~

The same holds for almost all ads less than $500. There is just too much data to clean up manually here so we will exclude them. SURELY we are throwing away some good data here and biasing our results if we are going to erase them out of the data. Fortunately, we can modify `filter_high_price` function to `filter_low_price` function.

~~~r
> filter_low_price = function(maker,year,header,price){
    idx = (vehicle$maker == maker & vehicle$year %in% c(year, year+1) &
          vehicle$price < 100000 & vehicle$price > 1 &
          grepl(pattern = gsub(maker,"",gsub("\\d+\\s","",header), ignore.case=TRUE),x = vehicle$header, ignore.case = TRUE) &
          !is.na(vehicle$maker) & !is.na(vehicle$price) & !is.na(vehicle$header))
  if(length(vehicle$price[idx])!= 0){
    newPrice = round( mean(vehicle$price[idx]), digits = -3)
    if(price < 1.5*newPrice) vehicle$price[vehicle$price == price & !is.na(vehicle$price)] <<- newPrice #return to global environment
  }
}

> idx = which( vehicle$price <= 500 & !is.na(vehicle$price))
> shortlist = vehicle[ idx, c("header", "price", "maker", "year") ]
> sapply(1:length(shortlist$maker), function(i) filter_low_price(shortlist$maker[i],shortlist$year[i],shortlist$header[i], shortlist$price[i]))
[1] Error in if(price < 1.5*newPrice) {: missing Value where TRUE/FALSE needed
~~~

Oops! We got error. So I use `traceback()` command to see where the error is:

~~~r
> traceback()
4: filter_low_price(shortlist$maker[i], shortlist$year[i], shortlist$header[i],
       shortlist$price[i]) at #1
3: FUN(X[[i]], ...) at #1
2: lapply(X = X, FUN = FUN, ...)
1: sapply(1:length(shortlist$maker), function(i) filter_low_price(shortlist$maker[i],
       shortlist$year[i], shortlist$header[i], shortlist$price[i]))
~~~

I guess that there are something wrong with the `shortlist` data frame. Thanks to <b>RStudio/DT</b> package, we can look at the data more directly. 

~~~r
# install from CRAN
> install.packages('DT')
> idx = which( vehicle$price <= 500 & !is.na(vehicle$price))
> shortlist = vehicle[ idx, c("header", "price", "maker", "year") ]
> datatable(shortlist)
~~~

<figure>
  <a href="/images/data-cleaning-image/data-table.png" title="Data Table of shortlist"><img src="/images/data-cleaning-image/data-table.png"></a>
  <figcaption><center><b><i>Figure.</i> <u>Data Table of shortlist</u></b></center></figcaption>
</figure>

Clearly, some <b>NA values</b> has caused the problem to `filter_low_price` function, since it doesn't detect the NA of the maker. So we remove the NA out of `shortlist$maker`, we will deal with them later.

~~~r
> idx = which( vehicle$price <= 500 & !is.na(vehicle$price) & !is.na(vehicle$maker))
> shortlist = vehicle[ idx, c("header", "price", "maker", "year") ]
> sapply(1:length(shortlist$maker), function(i) filter_low_price(shortlist$maker[i],shortlist$year[i],shortlist$header[i], shortlist$price[i]))
~~~

Let's check the data frame where the `vehicle$price <= 500` to see whether it works:

~~~r
> idx = which( vehicle$price <= 500 & !is.na(vehicle$price) & !is.na(vehicle$maker))
> vehicle[ idx, c("header", "price", "maker", "year") ]
                                            header price     maker year
posted1974                  2005 Mini Cooper Sport    19      mini 2005
posted17031               2014 Nissan Rogue SV AWD   340    nissan 2014
posted18911      2003 Mercury marquis gs park lane   165   mercury 2003
posted15112  2012 Audi Q5 3.2 Quattro Premium Plus   455      audi 2012
posted4082              2014 Ford Explorer Limited   365      ford 2014
posted21615                  1957 CHEVROLET BELAIR    70 chevrolet 1957
posted18374  2012 Audi Q5 3.2 Quattro Premium Plus   455      audi 2012
posted2555                      1997 Subaru legacy    80    subaru 1997
posted18929             2014 Ford Explorer Limited   365      ford 2014
posted9448                2012 Chrysler 300-Series   179  chrysler 2012
posted29613               1998 ford e150 econoline   375      ford 1998
posted128913                       1992 Saturn SL2    80    saturn 1992
posted229713   1961 chevrolet corvette convertible    53 chevrolet 1961
posted229813   1961 chevrolet corvette convertible    53 chevrolet 1961
posted229913   1961 chevrolet corvette convertible    53 chevrolet 1961
~~~

The data frame is much shorter. So, it works! Also we can erase all the data in this data frame because the price of all the entries in this data frame does not make sense and we can find the similar information of them in the `vehicle` data frame. Let's erase them:

~~~r
vehicle = vehicle[-idx,]
~~~

~~~r
> quantile(x = vehicle$price, probs = c(0.05,0.99), na.rm = TRUE)
   5%   99%
 1350 46000
~~~

=======================================================================

Price is not the only variable we should care about in this dataset. So let's check the list of variables again:

~~~r
> names(vehicle)
 [1] "id"           "title"        "body"         "lat"          "long"
 [6] "posted"       "updated"      "drive"        "odometer"     "type"
[11] "header"       "condition"    "cylinders"    "fuel"         "size"
[16] "transmission" "byOwner"      "city"         "time"         "description"
[21] "location"     "url"          "price"        "year"         "maker"
[26] "makerMethod"
~~~

<b>type</b> stands for types of vehicle. List all the different types of vehicle:

~~~r
> unique(vposts$type)
 [1] coupe       SUV         sedan       hatchback   wagon       van
 [7] <NA>        convertible pickup      truck       mini-van    other
[13] bus         offroad
~~~

or we can use <b><i>table</i></b> function instead of using <b><i>unique</i></b>

~~~r
> names( table(vehicle$type, useNA = "ifany") )
 [1] "bus"         "convertible" "coupe"       "hatchback"   "mini-van"
 [6] "offroad"     "other"       "pickup"      "sedan"       "SUV"
[11] "truck"       "van"         "wagon"       NA
~~~

To make use of the types of vehicle, we must know the proportion of these types of vehicle:

~~~r
> sort( round( x = prop.table( x = table(vehicle$type, useNA = "ifany") ), digits = 4) )

        bus     offroad    mini-van         van       wagon       other
     0.0006      0.0019      0.0131      0.0146      0.0161      0.0192
convertible   hatchback      pickup       truck       coupe         SUV
     0.0204      0.0236      0.0262      0.0347      0.0469      0.1213
      sedan        <NA>
     0.2031      0.4583
~~~

Let's plot the proportions. I use `dotplot` from <b>lattice</b> library.

~~~r
> t = prop.table( x = table(vehicle$type, useNA = "ifany") )
> dotplot(x = sort(t), xlim = c(-0.05, 1.05), cex = 1.5, main = "Proportions of Car Typ
+ es")
~~~

<figure>
  <a href="/images/data-cleaning-image/dot-plot1.png" title="Proportion of Vehicle Types"><img src="/images/data-cleaning-image/dot-plot1.png"></a>
  <figcaption><center><b><i>Figure.</i> <u>Proportion of Vehicle Types</u></b></center></figcaption>
</figure>

We made a mistake! Apparently, <b>NA</b> is missing from the chart. Or we could say that `lattice>dotplot does not plot NA at all`. So we can force it to do that by:

~~~r
> names(t)[ is.na(names(t)) ] = "NA"
> t
         bus  convertible        coupe    hatchback     mini-van      offroad
0.0006347008 0.0203681265 0.0469101610 0.0236281807 0.0130690670 0.0019041025
       other       pickup        sedan          SUV        truck          van
0.0192141250 0.0262246841 0.2030754140 0.1213432577 0.0346777451 0.0145981190
       wagon           NA
0.0160983209 0.4582539957

> dotplot(x = sort(t), xlim = c(-0.05, 1.05), cex = 1.5, main = "Proportions of Car Types")
~~~

<figure>
  <a href="/images/data-cleaning-image/dot-plot2.png" title="Proportion of Vehicle Types"><img src="/images/data-cleaning-image/dot-plot2.png"></a>
  <figcaption><center><b><i>Figure.</i> <u>Proportion of Vehicle Types</u></b></center></figcaption>
</figure>

=======================================================================


  Question #4 What are the different categories of vehicles,
i.e. the type variable/column? What is the proportion for each category ?
 # Different types.  Note the NA's
names( table(vehicle$type, useNA = "ifany") )
 ##  [1] "bus"
##  [6] "offroad"
## [11] "truck"
"convertible" "coupe"
"other"       "pickup"
"van"         "wagon"
"hatchback"   "mini-van"
"sedan"       "SUV"
NA
 # Proportions table, sorted
sort( round( x = prop.table( x = table(vehicle$type, useNA = "ifany") ), digits = 4) )
 ##
##         bus
##      0.0006
## convertible
  offroad
   0.0019
hatchback
   0.0236
     <NA>
   0.4583
mini-van
  0.0131
  pickup
  0.0262
   van
0.0146
 truck
0.0347
 wagon
0.0161
 coupe
0.0469
 other
0.0192
   SUV
0.1214
## ## ##
0.0204
 sedan
0.2030
  # Let's plot the proportions
# Base dotchart() will not show the label NA and Lattice dotplot() will not plot the
NA at all so force it
t = prop.table( x = table(vehicle$type, useNA = "ifany") )
names(t)[ is.na(names(t)) ] = "NA"
# dotchart(x = sort(t), xlim = c(0,1), main = "Proportions of Car Types")
dotplot(x = sort(t), xlim = c(-0.05, 1.05), cex = 1.5, main = "Proportions of Car Typ
es")
  # Let's look at proportions *among* the non-missing
t = prop.table( x = table(vehicle$type[ !is.na(vehicle$type) ], useNA = "ifany") )
dotplot(x = sort(t), xlim = c(-0.05, 1.05), cex = 1.5, main = "Proportions of Car Typ
es Among Non-Missing")
  Close to half of the data is missing the vehicle type. Among those not missing the vehicle type, about 40% are sedans.
Question #5 Display the relationship between fuel type and vehicle type. Does this depend on transmission type?
What we can see from the mosaic plots below overall and by transmission type and ignoring vehicle type other and fuel type other, is that gas vehicles dominate across vehicle types and across transmission types, with the notable exception that trucks have higher percent diesel than other vehicle types, as do buses with automatic transmissions - this is expected.
It might be easier to see these same relationships in dotplots than mosaicplots.
  # Overall relationship fuel type and vehicle type
tbl = table(vehicle$fuel, vehicle$type)
row.order = order( rowSums( tbl ), decreasing = TRUE )
col.order = order( colSums( tbl ), decreasing = TRUE )
tbl = tbl[ row.order, col.order ]
col.palette = colorRampPalette(brewer.pal(9,"Blues"))(length(col.order))
mosaicplot(tbl, las = 2, color = col.palette,
           main =  "Overall Fuel Type by Vehicle Type", cex = 0.7
)
 
  # Split by transmission type
fuelVehicleBytransmission = split( vehicle[ , c("fuel", "type")], vehicle$transmission)
invisible(
  lapply( 1:length(fuelVehicleBytransmission),
          FUN = function(x){
            tbl = table(fuelVehicleBytransmission[[x]]$fuel, fuelVehicleBytransmissio
n[[x]]$type)
) )
) }
row.order = order( rowSums( tbl ), decreasing = TRUE )
col.order = order( colSums( tbl ), decreasing = TRUE )
tbl = tbl[ row.order, col.order ]
# Get color palette and then reverse the order darkest to lightest
col.palette = colorRampPalette(brewer.pal(9,"Blues"))(length(col.order))
col.palette = col.palette[ length(col.palette):1 ]
mosaicplot(tbl, las = 2, color = col.palette,
           main = paste0("Fuel and Vehicle by Transmission = ",
                         names(fuelVehicleBytransmission)[x]), cex = 0.7
 
  
    dotplot(
  prop.table( table(vehicle$type , vehicle$transmission,  vehicle$fuel, useNA = "ifany")
,
1.2, pch=16),
  xlab = "Percent"
)
                   margin = c(1,2) ),
xlim = c(-0.05,1.05), auto.key = list(columns = 3), par.settings = simpleTheme(cex=
  Question #6 How many different cities are represented in the dataset?
 # levels() gives us the cities
levels(vehicle$city)
## [1] "boston"   "chicago"  "denver"   "lasvegas" "nyc"
## [7] "sfbay"
# length() tells us how many there are
length( levels(vehicle$city) )
"sac"
   ## [1] 7
 Question #7 Visually display how the number/proportion of “for sale by owner” and “for sale by dealer” varies across city?
It is a bit suspicious that all cities have roughly 5000 observations and that the percentages are almost perfectly 50/50 within each city. Smells like the data was filtered first...
Note that we also created a new variable called ownerDealer in the setup section.
 # Each city has ~5000 observations
 table(vehicle$city)
 ##
 ##   boston  chicago   denver lasvegas      nyc      sac    sfbay
 ##     4958     4886     4979     4963     4983     4966     4942
 # prop.table() with margin = 2 gives the column percentages
 prop.table(table(vehicle$ownerDealer, vehicle$city), margin = 2)
 ##
 ##             boston   chicago    denver  lasvegas       nyc       sac
 ##   Owner  0.4975797 0.4901760 0.4994979 0.4984888 0.4992976 0.5000000
 ##   Dealer 0.5024203 0.5098240 0.5005021 0.5015112 0.5007024 0.5000000
 ##
 ##              sfbay
 ##   Owner  0.4991906
 ##   Dealer 0.5008094
 # Plot the counts from table() (Note: almost identical graphs as percentages)
 plot( table(vehicle$city, vehicle$ownerDealer, useNA = "ifany"), main = "Owner/Dealer b
 y City",
       xlab = "City", ylab = "Owner or Dealer", col=c("darkblue","darkviolet"))
     
   # Plot the percentages from prop.table (Note the switch of the variables)
plot( prop.table(table(vehicle$city, vehicle$ownerDealer), margin = 2), main = "Owner/D
ealer by City",
      xlab = "City", ylab = "Owner or Dealer", col=c("darkblue","darkviolet"))
   # Use a dotplot since there are only two categories
tbl = prop.table( table(vehicle$ownerDealer, vehicle$city, useNA = "ifany"), margin = 2
)
dotplot( sort(tbl[ 1, ]), xlim = c(-0.05, 1.05), xlab = "% Owner for Sale by Owner",
ylab = "City",
         main = "Percent for Sale by Owner by City", cex = 1.5)
  The barplots basically capture the key information, but notice that the bar widths do not really represent anything meaningful. Since we are interested in the percentage for sale by owner within each city, a dotplot probably captures this the best since there are only two categories (byOwner = TRUE = owner, and byOwner = FALSE = dealer) and not missing data.
We can see from the tables and very clearly from the plots that the percentages of owner posting and dealer postings are almost perfectly 50/50 and this does not seem to vary across the different cities at all.



Question #8 What is the largest price for a vehicle in this data set? Examine this and fix the value. Now examine the new highest value for price.
We saw in question #3 above that there are lots of problems with the price data.
 # Being pedantic, let's get the max price
 max(vehicle$price, na.rm = TRUE)
 
  ## [1] 600030000
 # Let's remind ourselves of the few worst prices
idx = which( vehicle$price >=  9999999 & !is.na(vehicle$price) )
# Let's look at the actual data for those cars > $100,000
vehicle[ idx, c("header", "price", "maker", "year") ]
 ##
## posted22491
## posted23881
## posted6903  2002 Caddy Seville sls  30002500 cadillac 2002
## posted16005      2001 Honda Accord   9999999    honda 2001
          header     price    maker year
1969 Pontiac GTO 600030000  pontiac 1969
1969 Pontiac GTO 600030000  pontiac 1969
 # Let's fix the two records for the 1969 Pontiac GTO with price = 600030000
# We could remove it completely - maybe not the best approach.
# We could average the prices from the description, $6000 and $30,000 = $18,000 - not
bad approach.
# We could research this car online and try to find some average price - pretty good
approach.
# Let's see if we can't find a decent point estimate from within the dataset itself:
idx = ( vehicle$maker == "pontiac" & vehicle$year %in% c(1968, 1969) &
          vehicle$price < 9999999 & vehicle$price > 1 &
          grepl(pattern = "GTO", x = vehicle$header, ignore.case = TRUE) &
          !is.na(vehicle$maker) & !is.na(vehicle$price) & !is.na(vehicle$header) )
dat = vehicle[ idx, c("header", "price", "maker", "year") ]
dat[ order(dat$price), ]
 ##
## posted4991
## posted5371
## posted231214 1968 Pontiac GTO 24500 pontiac 1968
## posted16497  1969 Pontiac GTO 25000 pontiac 1969
## posted201111 1969 Pontiac GTO 25000 pontiac 1969
## posted7355   1968 pontiac gto 30000 pontiac 1968
## posted16701  1968 Pontiac gto 38500 pontiac 1968
## posted40911          1968 GTO 38500 pontiac 1968
          header price   maker year
1968 pontiac gto 15995 pontiac 1968
1968 pontiac gto 15995 pontiac 1968
   # Let's use a rounded mean price
 newPrice = round( mean(vehicle$price[idx]), digits = -3)
 vehicle$price[vehicle$price == 600030000 & !is.na(vehicle$price)] = newPrice
 # Now let's fix the 2002 Caddy Seville sls with price = 30002500
 # Let's see if we can't find a decent point estimate from within the dataset itself:
 idx = ( vehicle$maker == "cadillac" & vehicle$year %in% c(2002) &
           vehicle$price < 9999999 & vehicle$price > 1 &
           grepl(pattern = "Seville", x = vehicle$header, ignore.case = TRUE) &
           !is.na(vehicle$maker) & !is.na(vehicle$price) & !is.na(vehicle$header) )
 dat = vehicle[ idx, c("header", "price", "maker", "year") ]
 dat[ order(dat$price), ]
 ##                            header price    maker year
 ## posted20963 2002 cadillac seville   700 cadillac 2002
 ## posted20691 2002 Cadillac Seville  1500 cadillac 2002
 ## posted9323  2002 cadillac seville  2100 cadillac 2002
 ## posted16927 2002 cadillac seville  3495 cadillac 2002
 # Average price estimate is lower than the posting price of $2500 or $3000, so use th
 e lower $2500
 round( mean(vehicle$price[idx]), digits = -3)
## [1] 2000
 vehicle$price[vehicle$price == 30002500 & !is.na(vehicle$price)] = 2500
 # The 2001 Honda Accord with price = 9999999 is a junk post
 # "Selling my car for some lunch money. $20 OBO. Comes with complimentary Oboe."
 # Remove it completely.
 idx = which(vehicle$price == 9999999 & !is.na(vehicle$price))
 vehicle = vehicle[ -idx, ]
Again, SURELY there are more that need to be fixed, but that was enough pain for one day.
Question #9 What are the three most common makes of cars in each city for “sale by owner” and for “sale by dealer”? Are they similar or quite different?
    
  cities = levels(vehicle$city)
# We could do the inner function in one line, but it is hard to read so break it down
into steps.
#      names( head( sort( table(vehicle$maker[ vehicle$city == x & vehicle$byOwner == y
& !is.na(vehicle$city) ]),
#             decreasing = TRUE), 3) )
makeByCityByOwner = lapply(X = c(TRUE, FALSE), FUN = function(y){
  sapply(X = cities, FUN = function(x){
    t = table(vehicle$maker[ vehicle$city == x & vehicle$byOwner == y & !is.na(vehicle$ci
ty) ])
    s = sort(t, decreasing = TRUE)
    h = head(s, 3)
    n = names(h)
}) })
names(makeByCityByOwner) = c("Owner", "Dealer")
makeByCityByOwner
## $Owner
##      boston
## [1,] "ford"
## [2,] "honda"
## [3,] "chevrolet" "honda"
##      sfbay
## [1,] "toyota"
## [2,] "honda"
## [3,] "ford"
##
## $Dealer
##      boston
## [1,] "ford"
## [2,] "toyota"
## [3,] "chevrolet" "nissan"
##      sfbay
## [1,] "toyota"
## [2,] "ford"
## [3,] "bmw"
 chicago     denver      lasvegas    nyc      sac
"chevrolet" "ford"      "ford"      "nissan" "toyota"
"ford"
"chevrolet" "chevrolet" "toyota" "ford"
chicago
"chevrolet" "ford"
"ford"
"toyota"
"toyota"
lasvegas
            "ford"
"chevrolet" "nissan"
"honda"  "chevrolet"
nyc      sac
"nissan" "ford"
"toyota" "toyota"
denver
"dodge"     "chevrolet" "honda"  "chevrolet"
 # Check if the top in each city by owner and by dealer match
makeByCityByOwner$Owner[ 1, ] == makeByCityByOwner$Dealer[ 1, ]
##   boston  chicago   denver lasvegas      nyc      sac    sfbay
##     TRUE     TRUE     TRUE     TRUE     TRUE    FALSE     TRUE
 
 A quick visual scan shows that 2 of the top 3 makes by city for sale by owner are among the top 3 for sale by dealer within the same city. The top in each city by owner is the same as by dealer in all cities except SacTown. They are fairly similar.
Question #10 Visually compare the distribution of the age of cars for different cities and for “sale by owner” and “sale by dealer”. Provide an interpretation of the plots, i.e., what are the key conclusions and insights?
There are a few clear data errors with years in c(4, 1900, 2022), potentially others as well. We could fix them, or just ignore them since there are only a few data points that will not affect our analysis much.
The 2022 Honda Odyssey “has only 117102 miles” so it is probably a typo for 2002, so let’s fix it that way.
The year = 1900 ads are for wheels and buying back cars that fail the smog test so they are not cars at all. Remove them.
The Jeep with year = 4 is probably 2004 since it has a “AM/FM cassette player-muli CD player”, so let’s fix it that way.
 # Omitted here for brevity
 # vehicle[ vehicle$year %in% c(4, 1900, 2022) & !is.na(vehicle$year), ]
 # Clean up
 vehicle$year[ vehicle$year == 2022 & !is.na(vehicle$year) ] = 2002
 vehicle = vehicle[ -which(vehicle$year == 1900 & !is.na(vehicle$year)), ]
 vehicle$year[ vehicle$year == 4 & !is.na(vehicle$year) ] = 2004
 # Create an age variable
 vehicle$age = 2016 - vehicle$year
 # Overall by owner has older cars, dealers have newer cars
 histogram( ~ age | byOwner, data = vehicle, main = "Age by Owner/Dealer", col = "light
 blue")
 
   # Subset to zoom in a bit on the plots
idx = ( vehicle$age < 25 & !is.na(vehicle$age))
# Zoomed in
histogram( ~ age | byOwner, data = vehicle, subset = idx, main = "Age by Owner/Dealer
Zoomed",
           col = "lightblue")
   # Looking by city, there is not much difference at all across cities for owner vs. de
aler
histogram( ~ age | byOwner + city, data = vehicle, subset = idx, main = "Age by City b
y Owner/Dealer Zoomed",
           col = "lightblue")
 
 It does seem that cars for sale by owners tend to be older than cars for sale by dealers. However, this does not seem to vary much by city.
Question #11 Plot the locations of the posts on a map? What do you notice?
Remember that from question #6, the data was limited to just 7 major cities. Thus it would be a mistake to conclude something like almost all used cars on Craigslist (or from whatever website this data comes) sell in these 7 cities. We can conclude that the location of the people (and/or the cars themselves) who sell used cars in these major cities tend to be clustered fairly tightly around the major cities. Most buyers may be unlikely to travel far to look at, let alone, buy a used car.
There could be several explanations for the points far from one of the major cities. They could be travelling when they actaully posted the ad, for example. But in general, the location of the person posting the car is generally the same as the city in which they are attempting to sell the vehicle.
We also notice in the second plot of California that it seems that many people in Sac Town post their car on SF Bay Area, which we can see by the SF Bay points bleeding into the Sacramento area on the map. Perhaps they are trying to reach a bigger audience in the Bay.
We also notice that there are a few people in Reno / Tahoe posting ads in Sacramento and SF Bay. And there also appears to be somebody posting off the coast of Monterrey, but perhaps that is a data error :)
We could make one more improvement to this plot by using an alpha parameter to control the transparancy of the plotting points to better see density and bleeding into other regions.
  # Split the data by city to see if location of poster is clustered around posting cit
y
locationByCity = split(vehicle[ , c("long", "lat")], vehicle$city)
# Get color palette and then reverse the order darkest to lightest
col.palette = brewer.pal( length(locationByCity), "Purples")
col.palette = col.palette[ length(col.palette):1 ]
map('state', mar = c(0,0,0,0))
invisible(
  lapply( 1:length(locationByCity),
          FUN = function(x){
) )
    points(x = locationByCity[[ x ]]$long, y = locationByCity[[ x ]]$lat,
    pch = ".", cex = 5, col = col.palette[x]  )
}
legend("bottomleft", legend = names(locationByCity), col = col.palette, pch = 15, cex
= 0.9)
 
  map('state', region = 'california', mar = c(0,0,0,0))
 points(x = locationByCity[[ "sac" ]]$long, y = locationByCity[[ "sac" ]]$lat,
        pch = ".", cex = 4, col = col.palette[1] )
 points(x = locationByCity[[ "sfbay" ]]$long, y = locationByCity[[ "sfbay" ]]$lat,
        pch = ".", cex = 4, col = col.palette[5] )
 # reset margins
 par(mar = c(5, 4, 4, 2) + 0.1 )
Question #12 Summarize the distribution of fuel type, drive, transmission, and vehicle type. Find a good way to display this information.
Notice that in the dotplot below, that the distributions in the different panels are all almost identical excpet that the distribtion shows some variation in the middle column where fuel type = "gas" . Thus, we can essentially drop the fuel type from the plot, subset to just fuel type = "gas" and consider the relationship
  
 among the remaining three variables.
 # 9 panels;  circle colors are the vehicle type
# dotplot( table(vehicle$fuel, vehicle$drive, vehicle$transmission, vehicle$type))
# 15 panels
dotplot( table(vehicle$type, vehicle$fuel, vehicle$drive, vehicle$transmission),
         auto.key = list(columns = 3), par.settings = simpleTheme(cex=1.2, pch=16) )
 
   # fuel = "gas" for almost all the data
 table(vehicle$fuel, useNA = "ifany")
 ##
 ##   diesel electric      gas   hybrid    other     <NA>
 ##     1071       68    29222      350     1187     2771
Subsetting to just fuel == "gas" we see that automatic transmissions are the most common across all types of cars and across drive trains, followed by manual then other. Within the rear wheel drive vehicles, manual stick shifts do have a higher percentage than other verhicle types for hatchback, coupe and convertible, which makes sense since coupe and convertibles tend to be sports cars and hatchbacks tend to be cheaper, entry level models. Among 4 wheel ddrive, offroad vehicles have a higher
 idx = (vehicle$fuel == "gas" & !is.na(vehicle$fuel))
 dotplot(
   prop.table( table(vehicle$type[idx], vehicle$drive[idx], vehicle$transmission[idx], us
 eNA = "ifany"),
                      margin = c(1,2) ),
   xlim = c(-0.05,1.05), auto.key = list(columns = 3), par.settings = simpleTheme(cex=
 1.2, pch=16),
   xlab = "Percent"
)
  
  Question #13 Plot odometer reading and age of car? Is there a relationship? Similarly, plot odometer reading and price? Interpret the result(s). Are odometer reading and age of car related?
From question #3, we fixed some prices and we also concluded that we would subset price to those prices >= 500 and <= 50,000, which captures about 94% of the data.
We should spend some time cleaning up the odometer readings. For example, the max odometer reading of 1234567890 is just some annoying poster. But to keep things simple here, we see that 99th percentile of odometer readings is 2.610^{5}, thus we will trim the data at 500,000 to capture almost all of the distribtion.
As we can see from the smoothScatter plot below, there generally does seem to be an upward trend for the overwhelming majority of the data. However, notice the dense shading between about 5 years of age and 20 years of age that have low odometer readings. Also notice that once age of the vehicle gets beyond about 25 years or so that there is actually a downward trend in that subset of the data. This is likely because very old antique cars just sit in some old man’s garage getting polished and buffed and only taken out for a ride during antique car shows.
  quantile(vehicle$odometer, probs = 0.99, na.rm = TRUE)
  ##    99%
 ## 260000
 idx = ( vehicle$odometer < 500000 & !is.na(vehicle$odometer) & !is.na(vehicle$age) )
 smoothScatter(x = vehicle$age[idx], y = vehicle$odometer[idx], xlab = "Age of Vehicle",
               ylab = "Odometer Reading", main = "Odometer Readings by Age of Vehicle"
)
As we can see in the smoothscatter plot below, there is a general negative relationship between odometer reading and price, but note that there are some very expensive cars with low odometer readings, many of those are antique cars.
  
   idx = ( vehicle$odometer < 500000 & vehicle$price >= 500 & vehicle$price <= 100000 &
           !is.na(vehicle$odometer) & !is.na(vehicle$price) )
 smoothScatter(x = vehicle$odometer[idx], y = vehicle$price[idx], xlab = "Odometer Readi
 ng",
               ylab = "Price of Vehicle", main = "Price by Odometer Readings of Vehicl
e" )
Question #14 Identify the “old” cars. What manufacturers made these? What is the price distribution for these?
From the first smoothScatter plot below, it would seem that cars older than say 35 years are “old.”
From the table below, Chevy and Ford make up more than 50% of the “old” cars and contains most of the classic American car brands, as one might expect. In particular, there are not many “old” Japanese cars since the U.S. did not start importing Japanese cars on a large scale until the oil shocks of the 1970’s, whereas our cutoff for “old” is approximately 1970.
Comparing the price distribution of “old” cars compared to all cars, “old” cars seem have more density at higher prices with the majority of the prices less than $40,000 whereas the bulk of the overall data tends to be less than $20,000.
 
  # Check overall age and price relationship
idx = (vehicle$price >= 500 & vehicle$price <= 100000 &
          !is.na(vehicle$price) & !is.na(vehicle$age) )
smoothScatter(x = vehicle$age[idx], y = vehicle$price[idx], xlab = "Age of Vehicle",
              ylab = "Price of Vehicle", main = "Price by Age of Vehicle" )
  # Look at makers and price distribution for "old" cars
idx = (vehicle$age >= 35 & !is.na(vehicle$age))
# Look at the 10 largest makers of "old" cars
round( sort( tail( sort(table(vehicle$maker[ idx ])) / sum(table(vehicle$maker[ idx ]))
, 10)
             , decreasing = TRUE), digits = 3 )
##
##  chevrolet       ford volkswagen      dodge    pontiac   cadillac
 ## 0.315
## buick
## 0.027
0.213 gmc
   0.055      0.038      0.038      0.037
plymouth oldsmobile
0.026      0.025      0.024
   idx = (vehicle$age >= 35 & !is.na(vehicle$age) & !is.na(vehicle$price))
 smoothScatter(x = vehicle$age[idx], y = vehicle$price[idx], xlab = "Age of Vehicle",
               ylab = "Vehicle Price", main = "Price by Age of Vehicle" )
Question #15 I have omitted one important variable in this data set. What do you think it is? Can we derive this from the other variables? If so, sketch possible ideas as to how we would compute this variable.
When searching for cars on websites, the most important filters are usually year, make and model, in that order. Notice that year and make (i.e. maker) are stand-alone variables in the dataset, but model is not. However, notice that variable called header in the dataset is year, make, then model. Thus if we could parse the text string of each header to pull out the model, we could derive our own stand-alone variable for model. In fact, a future homework assignment will focus on using regular expressions to do just this type of string parsing.
 
  head(vehicle$header, 20)
  ##  [1] "2012 Chevrolet Camaro SS"
 ##  [2] "2013 Chevrolet Equinox LT"
 ##  [3] "2013 Nissan Altima 2.5 SV"
 ##  [4] "2009 Infiniti M35x X"
 ##  [5] "2013 Infiniti G37x X"
 ##  [6] "2012 Acura MDX 3.7L"
 ##  [7] "2010 Toyota Yaris"
 ##  [8] "2012 Acura RDX Base"
 ##  [9] "2014 Chevrolet Cruze 1LT Auto"
 ## [10] "2009 Lexus IS 250"
 ## [11] "2013 Toyota Camry SE"
 ## [12] "2014 Honda CR-Z"
 ## [13] "2008 BMW 335 xi"
 ## [14] "2013 Nissan Juke SL"
 ## [15] "2013 Dodge Durango SXT"
 ## [16] "2012 Toyota Camry 4dr Sedan I4 Automatic SE"
 ## [17] "2012 Toyota Venza LE"
 ## [18] "2014 Nissan Juke S"
 ## [19] "2012 Toyota Corolla"
 ## [20] "2012 Honda Pilot EX-L"
Question #16 Display how condition and odometer are related. Also how condition and price are related. And condition and age of the car. Provide a brief interpretation of what you find.
 # Print out conditions so we can cut and paste them into smaller
 # categories. There's really no way out of this since a human has to decide
 # what the new categories should be.
 conditions = levels(vehicle$condition)
 conditions = sprintf('"%s",\n', conditions)
  cat(conditions)
 
 ## "0used",
##  "207,400",
##  "ac/heater",
##  "carfax guarantee!!",
##  "certified",
##  "complete parts car, blown engine",
##  "excellent",
##  "fair",
##  "front side damage",
##  "good",
##  "hit and run :( gently",
##  "honnda",
##  "like new",
##  "mint",
##  "muscle car restore",
##  "needs bodywork",
##  "needs restoration!",
##  "needs restored",
##  "needs total restore",
##  "needs work",
##  "needs work/for parts",
##  "new",
##  "nice",
##  "nice rolling restoration",
##  "nice teuck",
##  "not running",
##  "parts",
##  "pre owned",
##  "pre-owned",
##  "preowned",
##  "preownes",
##  "project",
##  "project car",
##  "rebuildable project",
##  "restoration",
##  "restoration project",
##  "restore",
##  "restored",
##  "rough but runs",
##  "salvage",
##  "superb original",
##  "used",
##  "very good",
# We'll base the new categories on the most common existing categories.
sort(table(vehicle$condition))
 
 ##
##                            0used                          207,400
##                                1                                1
##                        ac/heater complete parts car, blown engine
##                                1                                1
##                front side damage            hit and run :( gently
##                                1                                1
##                           honnda                             mint
##                                1                                1
##               muscle car restore               needs restoration!
##                                1                                1
##              needs total restore                       needs work
##                                1                                1
##             needs work/for parts                             nice
##                                1                                1
##         nice rolling restoration                       nice teuck
##                                1                                1
##                      not running                        pre-owned
##                                1                                1
##                         preownes                      project car
##                                1                                1
##              rebuildable project                      restoration
##                                1                                1
##              restoration project                          restore
##                                1                                1
##                         restored                   rough but runs
##                                1                                1
##               carfax guarantee!!                   needs restored
##                                2                                2
##                        pre owned                  superb original
##                                2                                2
##                   needs bodywork                          project
##                                3                                3
##                            parts                         preowned
##                                5                               10
##                        very good                        certified
##                               10                               54
##
##
##
##
##
##
##
##
  salvage                              new
       61                              273
     fair                             used
      770                             1180
 like new                             good
     2898                             4663
excellent
     7543
 # Define new categories.
new_cats = list(
  excellent = c("excellent"),
  good = c("good", "very good"),
  "like new" = c("like new", "mint", "new", "pre owned", "pre-owned", "preowned", "pr
eownes"),
  used = c("0used", "used"),
  fair = c("fair", "nice", "nice teuck"),
  salvage = c("complete parts car, blown engine", "front side damage", "hit and run :
( gently",
stored",
ing restoration",
"muscle car restore", "needs bodywork", "needs restoration!", "needs re
"needs total restore", "needs work", "needs work/for parts", "nice roll
              "not running", "parts", "project", "project car", "rebuildable project"
, "restoration",
              "restoration project", "restore", "restored", "salvage", "rough but run
s"),
  other = c("207,400", "ac/heater", "carfax guarantee!!", "certified", "honnda", "sup
erb original" )
)
# Convert conditions to new categories.
vehicle$new_cond = vehicle$condition
levels(vehicle$new_cond) = c(levels(vehicle$new_cond), "other")
for (i in seq_along(new_cats)) {
  new_cat = names(new_cats)[[i]]
  vehicle$new_cond[vehicle$new_cond %in% new_cats[[i]]] = new_cat
}
vehicle$new_cond = factor(vehicle$new_cond)
# Restrict to odometer less than 5 million.
sane_odo = subset(vehicle, odometer < 5e6)
odo_by_cond = split(sane_odo$odometer, sane_odo$new_cond)
boxplot(odo_by_cond, col = "salmon")
title("Odometer Distribution by Condition", ylab = "Miles")
   # Make a second plot to show the distribution better.
boxplot(odo_by_cond, col = "skyblue1", ylim = c(0, 5e5))
title("Odometer Distribution by Condition", ylab = "Miles")
   # Now we can see that the highest odometer readings seem to be for the "fair"
# and "good" conditions, which is a little surprising. It's possible people
# overstate the condition of the car when the odometer is higher, to try to
# make it sound more appealing. The condition with the most spread-out
# distribution is "salvage," which makes sense because salvage cars could be
# very old, or they could be new cars that were damaged somehow.
sane_price = subset(vehicle, price < 2e5)
price_by_cond = split(sane_price$price, sane_price$new_cond)
boxplot(price_by_cond, col = "thistle")
title("Price Distribution by Condition", ylab = "Dollars")
   # Make an age column.
vehicle$age = 2015 - vehicle$year
sane_age = subset(vehicle, age < 200)
age_by_cond = split(sane_age$age, sane_age$new_cond)
boxplot(age_by_cond, col = "aquamarine")
title("Age Distribution by Condition", ylab = "Years")
   # The price and age distributions don't show anything too surprising. Price and
# condition appear to be directly related, with salvage cars having the lowest
# prices. Cars that are "like new" or "excellent" are sometimes offered for
# extremely high prices, whereas this is less common with cars in worse
# condition. Age and condition, are inversely related: older cars seem to have
# worse conditions. As with odometer and condition, salvage cars exhibit the
# weakest relationship.

