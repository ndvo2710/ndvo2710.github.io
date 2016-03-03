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

Let's look at <u>proportions among the non-missing</u> (not NA) 

~~~r
> t = prop.table( x = table(vehicle$type[ !is.na(vehicle$type) ], useNA = "ifany") )
> dotplot(x = sort(t), xlim = c(-0.05, 1.05), cex = 1.5, main = "Proportions of Car Types Among Non-Missing")
~~~

<figure>
  <a href="/images/data-cleaning-image/dot-plot3.png" title="Proportions of Car Types Among Non-Missing"><img src="/images/data-cleaning-image/dot-plot3.png"></a>
  <figcaption><center><b><i>Figure.</i> <u>Proportions of Car Types Among Non-Missing</u></b></center></figcaption>
</figure>

=======================================================================























