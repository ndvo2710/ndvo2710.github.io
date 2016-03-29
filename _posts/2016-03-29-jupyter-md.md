---
layout: page
title: Jupyter MD file with jekyll
excerpt: "Just about everything you'll need to do with MD file."
modified: 2016-03-03
tags: [R, jekyll, jupyter]
comments: true
---


```R
load("~/Dropbox/Fall 2015/STAT 141/Assignment1/vehicles.rda")
```


```R
ls()
```




<ol class=list-inline>
	<li>'lungDeaths'</li>
	<li>'vposts'</li>
</ol>






```R
vehicle = vposts
```


```R
library(lattice)
library(ggplot2)
library(gmodels)
library(RColorBrewer)
```


```R
class(vehicle)
```




'data.frame'




```R
names(vehicle)
```




<ol class=list-inline>
	<li>'id'</li>
	<li>'title'</li>
	<li>'body'</li>
	<li>'lat'</li>
	<li>'long'</li>
	<li>'posted'</li>
	<li>'updated'</li>
	<li>'drive'</li>
	<li>'odometer'</li>
	<li>'type'</li>
	<li>'header'</li>
	<li>'condition'</li>
	<li>'cylinders'</li>
	<li>'fuel'</li>
	<li>'size'</li>
	<li>'transmission'</li>
	<li>'byOwner'</li>
	<li>'city'</li>
	<li>'time'</li>
	<li>'description'</li>
	<li>'location'</li>
	<li>'url'</li>
	<li>'price'</li>
	<li>'year'</li>
	<li>'maker'</li>
	<li>'makerMethod'</li>
</ol>





```R
unlist( sapply(X = vehicle, FUN = class) )
```




<dl class=dl-horizontal>
	<dt>id</dt>
		<dd>'character'</dd>
	<dt>title</dt>
		<dd>'character'</dd>
	<dt>body</dt>
		<dd>'character'</dd>
	<dt>lat</dt>
		<dd>'numeric'</dd>
	<dt>long</dt>
		<dd>'numeric'</dd>
	<dt>posted1</dt>
		<dd>'POSIXct'</dd>
	<dt>posted2</dt>
		<dd>'POSIXt'</dd>
	<dt>updated1</dt>
		<dd>'POSIXct'</dd>
	<dt>updated2</dt>
		<dd>'POSIXt'</dd>
	<dt>drive</dt>
		<dd>'factor'</dd>
	<dt>odometer</dt>
		<dd>'integer'</dd>
	<dt>type</dt>
		<dd>'factor'</dd>
	<dt>header</dt>
		<dd>'character'</dd>
	<dt>condition</dt>
		<dd>'factor'</dd>
	<dt>cylinders</dt>
		<dd>'integer'</dd>
	<dt>fuel</dt>
		<dd>'factor'</dd>
	<dt>size</dt>
		<dd>'factor'</dd>
	<dt>transmission</dt>
		<dd>'factor'</dd>
	<dt>byOwner</dt>
		<dd>'logical'</dd>
	<dt>city</dt>
		<dd>'factor'</dd>
	<dt>time1</dt>
		<dd>'POSIXct'</dd>
	<dt>time2</dt>
		<dd>'POSIXt'</dd>
	<dt>description</dt>
		<dd>'character'</dd>
	<dt>location</dt>
		<dd>'character'</dd>
	<dt>url</dt>
		<dd>'character'</dd>
	<dt>price</dt>
		<dd>'integer'</dd>
	<dt>year</dt>
		<dd>'integer'</dd>
	<dt>maker</dt>
		<dd>'character'</dd>
	<dt>makerMethod</dt>
		<dd>'numeric'</dd>
</dl>





```R
densityplot(vehicle$price, main = "Price", xlab = "Price")
```


![svg](output_7_0.svg)



```R
tail( sort(vehicle$price), 50)
```




<ol class=list-inline>
	<li>95593</li>
	<li>96590</li>
	<li>97000</li>
	<li>97500</li>
	<li>97911</li>
	<li>98000</li>
	<li>99560</li>
	<li>99999</li>
	<li>100000</li>
	<li>100000</li>
	<li>100000</li>
	<li>100000</li>
	<li>104800</li>
	<li>105000</li>
	<li>105500</li>
	<li>107000</li>
	<li>112000</li>
	<li>116100</li>
	<li>116491</li>
	<li>120000</li>
	<li>122950</li>
	<li>123981</li>
	<li>125000</li>
	<li>129950</li>
	<li>129990</li>
	<li>138500</li>
	<li>139000</li>
	<li>139950</li>
	<li>143000</li>
	<li>143000</li>
	<li>143950</li>
	<li>147000</li>
	<li>149890</li>
	<li>149995</li>
	<li>150000</li>
	<li>152900</li>
	<li>159000</li>
	<li>169000</li>
	<li>177588</li>
	<li>202455</li>
	<li>240000</li>
	<li>286763</li>
	<li>359000</li>
	<li>400000</li>
	<li>559500</li>
	<li>569500</li>
	<li>9999999</li>
	<li>30002500</li>
	<li>600030000</li>
	<li>600030000</li>
</ol>





```R
idx = which( vehicle$price >=  9999999 & !is.na(vehicle$price) )
```


```R
vehicle[ idx, c("header", "price", "maker", "year") ]
```




<table>
<thead><tr><th></th><th scope=col>header</th><th scope=col>price</th><th scope=col>maker</th><th scope=col>year</th></tr></thead>
<tbody>
	<tr><th scope=row>posted22491</th><td>1969 Pontiac GTO</td><td>600030000</td><td>pontiac</td><td>1969</td></tr>
	<tr><th scope=row>posted23881</th><td>1969 Pontiac GTO</td><td>600030000</td><td>pontiac</td><td>1969</td></tr>
	<tr><th scope=row>posted6903</th><td>2002 Caddy Seville sls</td><td>30002500</td><td>cadillac</td><td>2002</td></tr>
	<tr><th scope=row>posted16005</th><td>2001 Honda Accord</td><td>9999999</td><td>honda</td><td>2001</td></tr>
</tbody>
</table>





```R
idx = ( vehicle$maker == "pontiac" & vehicle$year %in% c(1968, 1969) &
+ vehicle$price < 9999999 & vehicle$price > 1 &
+ grepl(pattern = "GTO", x = vehicle$header, ignore.case = TRUE) &
+ !is.na(vehicle$maker) & !is.na(vehicle$price) & !is.na(vehicle$header) )
```


```R
dat = vehicle[ idx, c("header", "price", "maker", "year") ]
```


```R
dat[ order(dat$price), ]
```




<table>
<thead><tr><th></th><th scope=col>header</th><th scope=col>price</th><th scope=col>maker</th><th scope=col>year</th></tr></thead>
<tbody>
	<tr><th scope=row>posted4991</th><td>1968 pontiac gto</td><td>15995</td><td>pontiac</td><td>1968</td></tr>
	<tr><th scope=row>posted5371</th><td>1968 pontiac gto</td><td>15995</td><td>pontiac</td><td>1968</td></tr>
	<tr><th scope=row>posted231214</th><td>1968 Pontiac GTO</td><td>24500</td><td>pontiac</td><td>1968</td></tr>
	<tr><th scope=row>posted16497</th><td>1969 Pontiac GTO</td><td>25000</td><td>pontiac</td><td>1969</td></tr>
	<tr><th scope=row>posted201111</th><td>1969 Pontiac GTO</td><td>25000</td><td>pontiac</td><td>1969</td></tr>
	<tr><th scope=row>posted7355</th><td>1968 pontiac gto</td><td>30000</td><td>pontiac</td><td>1968</td></tr>
	<tr><th scope=row>posted16701</th><td>1968 Pontiac gto</td><td>38500</td><td>pontiac</td><td>1968</td></tr>
	<tr><th scope=row>posted40911</th><td>1968 GTO</td><td>38500</td><td>pontiac</td><td>1968</td></tr>
</tbody>
</table>





```R
round( mean(vehicle$price[idx]), digits = -3)
```




27000




```R
vehicle$price[vehicle$price == 600030000 & !is.na(vehicle$price)] = 27000
```


```R
idx = which( vehicle$price >=  100000 & !is.na(vehicle$price))
length( idx )
```




40




```R
vehicle[ idx, c("header", "price") ]
```




<table>
<thead><tr><th></th><th scope=col>header</th><th scope=col>price</th></tr></thead>
<tbody>
	<tr><th scope=row>posted698</th><td>2008 BMW X5</td><td>177588</td></tr>
	<tr><th scope=row>posted1460</th><td>2010 CHEVROLET SILVERADO</td><td>359000</td></tr>
	<tr><th scope=row>posted12461</th><td>2000 Mack RD688S</td><td>1e+05</td></tr>
	<tr><th scope=row>posted22621</th><td>2013 Ford</td><td>150000</td></tr>
	<tr><th scope=row>posted24081</th><td>2003 lincoln navigator</td><td>1e+05</td></tr>
	<tr><th scope=row>posted21402</th><td>2009 CHEVROLET IMPALA</td><td>559500</td></tr>
	<tr><th scope=row>posted21422</th><td>2007 CHEVROLET MONTE CARLO</td><td>569500</td></tr>
	<tr><th scope=row>posted6903</th><td>2002 Caddy Seville sls</td><td>30002500</td></tr>
	<tr><th scope=row>posted16934</th><td>2013 Isuzu NRR</td><td>129990</td></tr>
	<tr><th scope=row>posted7225</th><td>1967 chevrolet corvette</td><td>105500</td></tr>
	<tr><th scope=row>posted13245</th><td>2010 ford fusion</td><td>105000</td></tr>
	<tr><th scope=row>posted16005</th><td>2001 Honda Accord</td><td>9999999</td></tr>
	<tr><th scope=row>posted9976</th><td>2004 Toyota Corolla</td><td>286763</td></tr>
	<tr><th scope=row>posted11066</th><td>2009 Lamborghini Gallardo</td><td>129950</td></tr>
	<tr><th scope=row>posted18506</th><td>2009 Mercedes-Benz SL65</td><td>139950</td></tr>
	<tr><th scope=row>posted18546</th><td>2013 Mercedes-Benz G63</td><td>122950</td></tr>
	<tr><th scope=row>posted214110</th><td>2011 Bentley Mulsanne</td><td>143950</td></tr>
	<tr><th scope=row>posted6747</th><td>2004 Lexus 470</td><td>169000</td></tr>
	<tr><th scope=row>posted20867</th><td>2015 mercedes-benz s550</td><td>107000</td></tr>
	<tr><th scope=row>posted5038</th><td>2012 Mercedes-Benz SLS AMG 2dr Roadster SLS AMG</td><td>149890</td></tr>
	<tr><th scope=row>posted23788</th><td>2006 FORD GT</td><td>4e+05</td></tr>
	<tr><th scope=row>posted12630</th><td>2014 ferrari 458 italia</td><td>240000</td></tr>
	<tr><th scope=row>posted37310</th><td>2014 Audi RS 7 4.0T quattro</td><td>116491</td></tr>
	<tr><th scope=row>posted163710</th><td>2015 Mercedes-Benz  S-Class</td><td>143000</td></tr>
	<tr><th scope=row>posted212510</th><td>2014 Porsche  911</td><td>104800</td></tr>
	<tr><th scope=row>posted231114</th><td>2015 Porsche  911</td><td>152900</td></tr>
	<tr><th scope=row>posted181511</th><td>1965 porsche 911</td><td>1e+05</td></tr>
	<tr><th scope=row>posted194311</th><td>2005 TOYOTA AVALON</td><td>112000</td></tr>
	<tr><th scope=row>posted220311</th><td>2011 toyota rav4</td><td>159000</td></tr>
	<tr><th scope=row>posted106214</th><td>2014 Land Rover Range 5.0L V8</td><td>123981</td></tr>
	<tr><th scope=row>posted121313</th><td>2016 porsche 911</td><td>202455</td></tr>
	<tr><th scope=row>posted129214</th><td>2007 Lamborghini Gallardo Spyder</td><td>149995</td></tr>
	<tr><th scope=row>posted191712</th><td>2015 Hyundai Sonata</td><td>138500</td></tr>
	<tr><th scope=row>posted222912</th><td>2015 Porsche  Panamera</td><td>116100</td></tr>
	<tr><th scope=row>posted231215</th><td>2015 Mercedes-Benz  S-Class</td><td>143000</td></tr>
	<tr><th scope=row>posted95215</th><td>1988 porsche 911 Carrera Targa TL</td><td>120000</td></tr>
	<tr><th scope=row>posted112215</th><td>1976 Porsche 930</td><td>139000</td></tr>
	<tr><th scope=row>posted212613</th><td>1941 willys</td><td>125000</td></tr>
	<tr><th scope=row>posted238613</th><td>2015 Porsche GT3</td><td>147000</td></tr>
	<tr><th scope=row>posted245512</th><td>1961 Maserati 151</td><td>1e+05</td></tr>
</tbody>
</table>





```R
shortlist = vehicle[ idx, c("header", "price", "maker", "year") ]
```


```R
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
```




<ol>
	<li>19000</li>
	<li>26000</li>
	<li>NULL</li>
	<li>19000</li>
	<li>6000</li>
	<li>7000</li>
	<li>7000</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>41000</li>
	<li>9000</li>
	<li>3000</li>
	<li>5000</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>19000</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>14000</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>6000</li>
	<li>8000</li>
	<li>17000</li>
	<li>NULL</li>
	<li>97000</li>
	<li>NULL</li>
	<li>20000</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>20000</li>
	<li>NULL</li>
	<li>6000</li>
</ol>





```R
idx = which( vehicle$price >=  100000 & !is.na(vehicle$price) )
vehicle[ idx, c("header", "price") ]
```




<table>
<thead><tr><th></th><th scope=col>header</th><th scope=col>price</th></tr></thead>
<tbody>
	<tr><th scope=row>posted6903</th><td>2002 Caddy Seville sls</td><td>30002500</td></tr>
	<tr><th scope=row>posted16934</th><td>2013 Isuzu NRR</td><td>129990</td></tr>
	<tr><th scope=row>posted11066</th><td>2009 Lamborghini Gallardo</td><td>129950</td></tr>
	<tr><th scope=row>posted18506</th><td>2009 Mercedes-Benz SL65</td><td>139950</td></tr>
	<tr><th scope=row>posted18546</th><td>2013 Mercedes-Benz G63</td><td>122950</td></tr>
	<tr><th scope=row>posted214110</th><td>2011 Bentley Mulsanne</td><td>143950</td></tr>
	<tr><th scope=row>posted20867</th><td>2015 mercedes-benz s550</td><td>107000</td></tr>
	<tr><th scope=row>posted5038</th><td>2012 Mercedes-Benz SLS AMG 2dr Roadster SLS AMG</td><td>149890</td></tr>
	<tr><th scope=row>posted12630</th><td>2014 ferrari 458 italia</td><td>240000</td></tr>
	<tr><th scope=row>posted37310</th><td>2014 Audi RS 7 4.0T quattro</td><td>116491</td></tr>
	<tr><th scope=row>posted163710</th><td>2015 Mercedes-Benz  S-Class</td><td>143000</td></tr>
	<tr><th scope=row>posted212510</th><td>2014 Porsche  911</td><td>104800</td></tr>
	<tr><th scope=row>posted231114</th><td>2015 Porsche  911</td><td>152900</td></tr>
	<tr><th scope=row>posted106214</th><td>2014 Land Rover Range 5.0L V8</td><td>123981</td></tr>
	<tr><th scope=row>posted129214</th><td>2007 Lamborghini Gallardo Spyder</td><td>149995</td></tr>
	<tr><th scope=row>posted222912</th><td>2015 Porsche  Panamera</td><td>116100</td></tr>
	<tr><th scope=row>posted231215</th><td>2015 Mercedes-Benz  S-Class</td><td>143000</td></tr>
	<tr><th scope=row>posted95215</th><td>1988 porsche 911 Carrera Targa TL</td><td>120000</td></tr>
	<tr><th scope=row>posted112215</th><td>1976 Porsche 930</td><td>139000</td></tr>
	<tr><th scope=row>posted238613</th><td>2015 Porsche GT3</td><td>147000</td></tr>
</tbody>
</table>





```R
densityplot(vehicle$price, main = "Price", xlab = "Price")
```


![svg](output_21_0.svg)



```R
sum( is.na(vehicle$price) )
```




3328




```R
sum( vehicle$price == 1 & !is.na(vehicle$price) )
```




612




```R
filter_low_price = function(maker,year,header,price){
    idx = (vehicle$maker == maker & vehicle$year %in% c(year, year+1) &
          vehicle$price < 100000 & vehicle$price > 1 &
          grepl(pattern = gsub(maker,"",gsub("\\d+\\s","",header), ignore.case=TRUE),x = vehicle$header, ignore.case = TRUE) &
          !is.na(vehicle$maker) & !is.na(vehicle$price) & !is.na(vehicle$header))
  if(length(vehicle$price[idx])!= 0){
    newPrice = round( mean(vehicle$price[idx]), digits = -3)
    if(price < 1.5*newPrice) vehicle$price[vehicle$price == price & !is.na(vehicle$price)] <<- newPrice #return to global environment
  }
}



idx = which( vehicle$price <= 500 & !is.na(vehicle$price))
shortlist = vehicle[ idx, c("header", "price", "maker", "year") ]
sapply(1:length(shortlist$maker), function(i) filter_low_price(shortlist$maker[i],shortlist$year[i],shortlist$header[i], shortlist$price[i]))
```


    Error in if (price < 1.5 * newPrice) vehicle$price[vehicle$price == price & : missing value where TRUE/FALSE needed




```R
library(shiny)
library(DT)
```


```R
idx = which( vehicle$price <= 500 & !is.na(vehicle$price))
shortlist = vehicle[ idx, c("header", "price", "maker", "year") ]
```


```R
head(shortlist,n=50)
```




<table>
<thead><tr><th></th><th scope=col>header</th><th scope=col>price</th><th scope=col>maker</th><th scope=col>year</th></tr></thead>
<tbody>
	<tr><th scope=row>posted1965</th><td>2004 Jeep Wrangler X</td><td>23</td><td>jeep</td><td>2004</td></tr>
	<tr><th scope=row>posted1974</th><td>2005 Mini Cooper Sport</td><td>19</td><td>mini</td><td>2005</td></tr>
	<tr><th scope=row>posted2129</th><td>1998 DODGE CARAVAN</td><td>400</td><td>dodge</td><td>1998</td></tr>
	<tr><th scope=row>posted2138</th><td>1995 ford escort wagon</td><td>300</td><td>ford</td><td>1995</td></tr>
	<tr><th scope=row>posted7210</th><td>1995 Subaru Legacy</td><td>499</td><td>subaru</td><td>1995</td></tr>
	<tr><th scope=row>posted19510</th><td>2015 BRIWAY</td><td>210</td><td>NA</td><td>2015</td></tr>
	<tr><th scope=row>posted24110</th><td>2005 Bridgestone dueler</td><td>400</td><td>NA</td><td>2005</td></tr>
	<tr><th scope=row>posted2498</th><td>1964 Pontiac GTO</td><td>30</td><td>pontiac</td><td>1964</td></tr>
	<tr><th scope=row>posted2771</th><td>2001 Chevrolet Malibu</td><td>400</td><td>chevrolet</td><td>2001</td></tr>
	<tr><th scope=row>posted3931</th><td>1986 Mazda RX-7</td><td>395</td><td>mazda</td><td>1986</td></tr>
	<tr><th scope=row>posted5361</th><td>2008 Honda Accord</td><td>12</td><td>honda</td><td>2008</td></tr>
	<tr><th scope=row>posted6871</th><td>2009 chevrolet equinox</td><td>8</td><td>chevrolet</td><td>2009</td></tr>
	<tr><th scope=row>posted7461</th><td>2012 cadillac srx</td><td>6</td><td>cadillac</td><td>2012</td></tr>
	<tr><th scope=row>posted7471</th><td>2008 Mazda 6</td><td>2</td><td>mazda</td><td>2008</td></tr>
	<tr><th scope=row>posted7491</th><td>2006 saab 9-3</td><td>2</td><td>saab</td><td>2006</td></tr>
	<tr><th scope=row>posted7501</th><td>2004 chevrolet trailblazer ext</td><td>3</td><td>chevrolet</td><td>2004</td></tr>
	<tr><th scope=row>posted7811</th><td>2000 trailer</td><td>300</td><td>NA</td><td>2000</td></tr>
	<tr><th scope=row>posted8091</th><td>2006 Hyndai Tucson</td><td>4</td><td>hyundai</td><td>2006</td></tr>
	<tr><th scope=row>posted8531</th><td>1994 acura    legend</td><td>300</td><td>acura</td><td>1994</td></tr>
	<tr><th scope=row>posted8961</th><td>2000 Fhfhfhffj</td><td>200</td><td>NA</td><td>2000</td></tr>
	<tr><th scope=row>posted9421</th><td>2000 honda accord 4-door sedan</td><td>450</td><td>honda</td><td>2000</td></tr>
	<tr><th scope=row>posted9451</th><td>1993 ford mustang</td><td>100</td><td>ford</td><td>1993</td></tr>
	<tr><th scope=row>posted9771</th><td>2002 ford taurus x</td><td>2</td><td>ford</td><td>2002</td></tr>
	<tr><th scope=row>posted9791</th><td>2002 TOYOTA COROLLA</td><td>3</td><td>toyota</td><td>2002</td></tr>
	<tr><th scope=row>posted10071</th><td>2001 bobcat v 623 loadall</td><td>55</td><td>NA</td><td>2001</td></tr>
	<tr><th scope=row>posted10161</th><td>1989 case 1835 c diesel</td><td>11</td><td>NA</td><td>1989</td></tr>
	<tr><th scope=row>posted11291</th><td>2016 Variety</td><td>2</td><td>NA</td><td>2016</td></tr>
	<tr><th scope=row>posted11961</th><td>1997 chevy</td><td>495</td><td>chevrolet</td><td>1997</td></tr>
	<tr><th scope=row>posted12091</th><td>2012 cadillac srx</td><td>6</td><td>cadillac</td><td>2012</td></tr>
	<tr><th scope=row>posted12111</th><td>2008 Mazda 6</td><td>2</td><td>mazda</td><td>2008</td></tr>
	<tr><th scope=row>posted12401</th><td>1969 repair manual</td><td>60</td><td>NA</td><td>1969</td></tr>
	<tr><th scope=row>posted12781</th><td>1972 Chevrolet C10</td><td>10</td><td>chevrolet</td><td>1972</td></tr>
	<tr><th scope=row>posted12851</th><td>2008 gmc 2500 sierra</td><td>120</td><td>gmc</td><td>2008</td></tr>
	<tr><th scope=row>posted13201</th><td>2013 PowerStation</td><td>80</td><td>NA</td><td>2013</td></tr>
	<tr><th scope=row>posted13391</th><td>2009 Ford F-150</td><td>50</td><td>ford</td><td>2009</td></tr>
	<tr><th scope=row>posted14011</th><td>1986 Mazda RX-7</td><td>395</td><td>mazda</td><td>1986</td></tr>
	<tr><th scope=row>posted14031</th><td>1999 toyota supra</td><td>50</td><td>toyota</td><td>1999</td></tr>
	<tr><th scope=row>posted14411</th><td>2013 tral</td><td>50</td><td>NA</td><td>2013</td></tr>
	<tr><th scope=row>posted16351</th><td>1985 300 D</td><td>400</td><td>mercedes</td><td>1985</td></tr>
	<tr><th scope=row>posted16581</th><td>2003 ford windstar van</td><td>4</td><td>ford</td><td>2003</td></tr>
	<tr><th scope=row>posted16781</th><td>1997 Ford pick up</td><td>175</td><td>ford</td><td>1997</td></tr>
	<tr><th scope=row>posted17031</th><td>2014 Nissan Rogue SV AWD</td><td>340</td><td>nissan</td><td>2014</td></tr>
	<tr><th scope=row>posted17171</th><td>2008 cadillac srx</td><td>100</td><td>cadillac</td><td>2008</td></tr>
	<tr><th scope=row>posted17791</th><td>2003 Snowbear</td><td>200</td><td>NA</td><td>2003</td></tr>
	<tr><th scope=row>posted17931</th><td>2015 Tire</td><td>15</td><td>NA</td><td>2015</td></tr>
	<tr><th scope=row>posted18031</th><td>2010 truck</td><td>60</td><td>NA</td><td>2010</td></tr>
	<tr><th scope=row>posted18431</th><td>1997 Chevy Cavalier</td><td>450</td><td>chevrolet</td><td>1997</td></tr>
	<tr><th scope=row>posted18611</th><td>1999 Ford Ranger</td><td>175</td><td>ford</td><td>1999</td></tr>
	<tr><th scope=row>posted18631</th><td>2001 honda accord 2-door coupe</td><td>400</td><td>honda</td><td>2001</td></tr>
	<tr><th scope=row>posted18911</th><td>2003 Mercury marquis gs park lane</td><td>165</td><td>mercury</td><td>2003</td></tr>
</tbody>
</table>





```R
idx = which( vehicle$price <= 500 & !is.na(vehicle$price) & !is.na(vehicle$maker))
shortlist = vehicle[ idx, c("header", "price", "maker", "year") ]
sapply(1:length(shortlist$maker), function(i) filter_low_price(shortlist$maker[i],shortlist$year[i],shortlist$header[i], shortlist$price[i]))
```




<ol>
	<li>8000</li>
	<li>NULL</li>
	<li>1000</li>
	<li>NULL</li>
	<li>2000</li>
	<li>NULL</li>
	<li>2000</li>
	<li>2000</li>
	<li>10000</li>
	<li>6000</li>
	<li>16000</li>
	<li>6000</li>
	<li>6000</li>
	<li>3000</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>3000</li>
	<li>5000</li>
	<li>6000</li>
	<li>4000</li>
	<li>3000</li>
	<li>20000</li>
	<li>7000</li>
	<li>7000</li>
	<li>17000</li>
	<li>19000</li>
	<li>2000</li>
	<li>19000</li>
	<li>5000</li>
	<li>2000</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>7000</li>
	<li>1000</li>
	<li>3000</li>
	<li>3000</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>3000</li>
	<li>6000</li>
	<li>5000</li>
	<li>14000</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>6000</li>
	<li>4000</li>
	<li>7000</li>
	<li>16000</li>
	<li>29000</li>
	<li>NULL</li>
	<li>13000</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>18000</li>
	<li>21000</li>
	<li>15000</li>
	<li>8000</li>
	<li>11000</li>
	<li>NULL</li>
	<li>16000</li>
	<li>11000</li>
	<li>33000</li>
	<li>17000</li>
	<li>15000</li>
	<li>16000</li>
	<li>6000</li>
	<li>4000</li>
	<li>NULL</li>
	<li>4000</li>
	<li>13000</li>
	<li>NULL</li>
	<li>18000</li>
	<li>10000</li>
	<li>10000</li>
	<li>10000</li>
	<li>6000</li>
	<li>16000</li>
	<li>10000</li>
	<li>12000</li>
	<li>NULL</li>
	<li>10000</li>
	<li>NULL</li>
	<li>11000</li>
	<li>7000</li>
	<li>14000</li>
	<li>18000</li>
	<li>4000</li>
	<li>4000</li>
	<li>6000</li>
	<li>4000</li>
	<li>4000</li>
	<li>4000</li>
	<li>4000</li>
	<li>4000</li>
	<li>12000</li>
	<li>4000</li>
	<li>4000</li>
	<li>4000</li>
	<li>20000</li>
	<li>4000</li>
	<li>4000</li>
	<li>4000</li>
	<li>4000</li>
	<li>8000</li>
	<li>4000</li>
	<li>18000</li>
	<li>23000</li>
	<li>13000</li>
	<li>9000</li>
	<li>8000</li>
	<li>36000</li>
	<li>15000</li>
	<li>32000</li>
	<li>12000</li>
	<li>12000</li>
	<li>13000</li>
	<li>6000</li>
	<li>5000</li>
	<li>NULL</li>
	<li>2000</li>
	<li>5000</li>
	<li>17000</li>
	<li>5000</li>
	<li>NULL</li>
	<li>6000</li>
	<li>4000</li>
	<li>3000</li>
	<li>NULL</li>
	<li>2000</li>
	<li>2000</li>
	<li>2000</li>
	<li>19000</li>
	<li>13000</li>
	<li>3000</li>
	<li>3000</li>
	<li>1000</li>
	<li>3000</li>
	<li>3000</li>
	<li>1000</li>
	<li>4000</li>
	<li>3000</li>
	<li>NULL</li>
	<li>1000</li>
	<li>NULL</li>
	<li>16000</li>
	<li>5000</li>
	<li>NULL</li>
	<li>2000</li>
	<li>NULL</li>
	<li>19000</li>
	<li>4000</li>
	<li>2000</li>
	<li>11000</li>
	<li>11000</li>
	<li>13000</li>
	<li>13000</li>
	<li>NULL</li>
	<li>12000</li>
	<li>15000</li>
	<li>13000</li>
	<li>30000</li>
	<li>14000</li>
	<li>NULL</li>
	<li>9000</li>
	<li>9000</li>
	<li>21000</li>
	<li>2000</li>
	<li>NULL</li>
	<li>4000</li>
	<li>5000</li>
	<li>1000</li>
	<li>13000</li>
	<li>NULL</li>
	<li>8000</li>
	<li>5000</li>
	<li>18000</li>
	<li>2000</li>
	<li>32000</li>
	<li>18000</li>
	<li>NULL</li>
	<li>2000</li>
	<li>2000</li>
	<li>16000</li>
	<li>NULL</li>
	<li>2000</li>
	<li>2000</li>
	<li>5000</li>
	<li>NULL</li>
	<li>17000</li>
	<li>4000</li>
	<li>11000</li>
	<li>5000</li>
	<li>2000</li>
	<li>2000</li>
	<li>21000</li>
	<li>20000</li>
	<li>26000</li>
	<li>10000</li>
	<li>17000</li>
	<li>4000</li>
	<li>11000</li>
	<li>19000</li>
	<li>3000</li>
	<li>2000</li>
	<li>2000</li>
	<li>13000</li>
	<li>2000</li>
	<li>3000</li>
	<li>17000</li>
	<li>5000</li>
	<li>8000</li>
	<li>2000</li>
	<li>5000</li>
	<li>5000</li>
	<li>4000</li>
	<li>7000</li>
	<li>4000</li>
	<li>NULL</li>
	<li>22000</li>
	<li>5000</li>
	<li>12000</li>
	<li>15000</li>
	<li>7000</li>
	<li>5000</li>
	<li>13000</li>
	<li>9000</li>
	<li>19000</li>
	<li>22000</li>
	<li>2000</li>
	<li>5000</li>
	<li>4000</li>
	<li>5000</li>
	<li>4000</li>
	<li>5000</li>
	<li>13000</li>
	<li>2000</li>
	<li>3000</li>
	<li>5000</li>
	<li>7000</li>
	<li>6000</li>
	<li>6000</li>
	<li>5000</li>
	<li>5000</li>
	<li>2000</li>
	<li>5000</li>
	<li>8000</li>
	<li>7000</li>
	<li>5000</li>
	<li>2000</li>
	<li>5000</li>
	<li>22000</li>
	<li>2000</li>
	<li>2000</li>
	<li>2000</li>
	<li>7000</li>
	<li>13000</li>
	<li>9000</li>
	<li>5000</li>
	<li>2000</li>
	<li>5000</li>
	<li>3000</li>
	<li>2000</li>
	<li>2000</li>
	<li>2000</li>
	<li>5000</li>
	<li>8000</li>
	<li>5000</li>
	<li>2000</li>
	<li>3000</li>
	<li>8000</li>
	<li>19000</li>
	<li>4000</li>
	<li>8000</li>
	<li>5000</li>
	<li>23000</li>
	<li>2000</li>
	<li>2000</li>
	<li>2000</li>
	<li>5000</li>
	<li>2000</li>
	<li>6000</li>
	<li>4000</li>
	<li>5000</li>
	<li>9000</li>
	<li>7000</li>
	<li>4000</li>
	<li>6000</li>
	<li>5000</li>
	<li>7000</li>
	<li>2000</li>
	<li>8000</li>
	<li>2000</li>
	<li>5000</li>
	<li>2000</li>
	<li>2000</li>
	<li>2000</li>
	<li>2000</li>
	<li>13000</li>
	<li>3000</li>
	<li>22000</li>
	<li>2000</li>
	<li>5000</li>
	<li>5000</li>
	<li>3000</li>
	<li>3000</li>
	<li>1000</li>
	<li>6000</li>
	<li>4000</li>
	<li>6000</li>
	<li>4000</li>
	<li>4000</li>
	<li>3000</li>
	<li>15000</li>
	<li>4000</li>
	<li>13000</li>
	<li>10000</li>
	<li>1000</li>
	<li>6000</li>
	<li>6000</li>
	<li>9000</li>
	<li>12000</li>
	<li>22000</li>
	<li>6000</li>
	<li>3000</li>
	<li>2000</li>
	<li>17000</li>
	<li>2000</li>
	<li>8000</li>
	<li>2000</li>
	<li>6000</li>
	<li>11000</li>
	<li>5000</li>
	<li>8000</li>
	<li>NULL</li>
	<li>6000</li>
	<li>3000</li>
	<li>11000</li>
	<li>21000</li>
	<li>5000</li>
	<li>1000</li>
	<li>NULL</li>
	<li>4000</li>
	<li>2000</li>
	<li>18000</li>
	<li>4000</li>
	<li>4000</li>
	<li>5000</li>
	<li>14000</li>
	<li>22000</li>
	<li>13000</li>
	<li>3000</li>
	<li>6000</li>
	<li>5000</li>
	<li>3000</li>
	<li>5000</li>
	<li>8000</li>
	<li>5000</li>
	<li>6000</li>
	<li>3000</li>
	<li>10000</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>12000</li>
	<li>2000</li>
	<li>2000</li>
	<li>2000</li>
	<li>2000</li>
	<li>2000</li>
	<li>2000</li>
	<li>2000</li>
	<li>3000</li>
	<li>22000</li>
	<li>28000</li>
	<li>9000</li>
	<li>8000</li>
	<li>12000</li>
	<li>13000</li>
	<li>10000</li>
	<li>NULL</li>
	<li>28000</li>
	<li>30000</li>
	<li>24000</li>
	<li>16000</li>
	<li>22000</li>
	<li>10000</li>
	<li>NULL</li>
	<li>5000</li>
	<li>8000</li>
	<li>NULL</li>
	<li>18000</li>
	<li>3000</li>
	<li>2000</li>
	<li>13000</li>
	<li>29000</li>
	<li>12000</li>
	<li>18000</li>
	<li>22000</li>
	<li>22000</li>
	<li>29000</li>
	<li>32000</li>
	<li>27000</li>
	<li>31000</li>
	<li>33000</li>
	<li>35000</li>
	<li>29000</li>
	<li>21000</li>
	<li>22000</li>
	<li>22000</li>
	<li>32000</li>
	<li>12000</li>
	<li>13000</li>
	<li>13000</li>
	<li>14000</li>
	<li>14000</li>
	<li>16000</li>
	<li>18000</li>
	<li>13000</li>
	<li>15000</li>
	<li>15000</li>
	<li>14000</li>
	<li>16000</li>
	<li>17000</li>
	<li>17000</li>
	<li>22000</li>
	<li>15000</li>
	<li>17000</li>
	<li>15000</li>
	<li>18000</li>
	<li>24000</li>
	<li>14000</li>
	<li>15000</li>
	<li>15000</li>
	<li>15000</li>
	<li>16000</li>
	<li>20000</li>
	<li>15000</li>
	<li>15000</li>
	<li>28000</li>
	<li>31000</li>
	<li>15000</li>
	<li>NULL</li>
	<li>13000</li>
	<li>10000</li>
	<li>10000</li>
	<li>10000</li>
	<li>21000</li>
	<li>19000</li>
	<li>11000</li>
	<li>22000</li>
	<li>18000</li>
	<li>25000</li>
	<li>25000</li>
	<li>16000</li>
	<li>23000</li>
	<li>32000</li>
	<li>17000</li>
	<li>36000</li>
	<li>16000</li>
	<li>NULL</li>
	<li>14000</li>
	<li>17000</li>
	<li>25000</li>
	<li>22000</li>
	<li>18000</li>
	<li>4000</li>
	<li>29000</li>
	<li>7000</li>
	<li>10000</li>
	<li>15000</li>
	<li>12000</li>
	<li>15000</li>
	<li>9000</li>
	<li>23000</li>
	<li>33000</li>
	<li>34000</li>
	<li>20000</li>
	<li>18000</li>
	<li>25000</li>
	<li>33000</li>
	<li>2000</li>
	<li>14000</li>
	<li>22000</li>
	<li>18000</li>
	<li>35000</li>
	<li>13000</li>
	<li>9000</li>
	<li>10000</li>
	<li>14000</li>
	<li>10000</li>
	<li>22000</li>
	<li>24000</li>
	<li>18000</li>
	<li>10000</li>
	<li>17000</li>
	<li>15000</li>
	<li>19000</li>
	<li>17000</li>
	<li>3000</li>
	<li>5000</li>
	<li>17000</li>
	<li>17000</li>
	<li>14000</li>
	<li>16000</li>
	<li>3000</li>
	<li>29000</li>
	<li>34000</li>
	<li>17000</li>
	<li>5000</li>
	<li>18000</li>
	<li>4000</li>
	<li>17000</li>
	<li>17000</li>
	<li>33000</li>
	<li>34000</li>
	<li>17000</li>
	<li>13000</li>
	<li>5000</li>
	<li>16000</li>
	<li>27000</li>
	<li>17000</li>
	<li>5000</li>
	<li>3000</li>
	<li>23000</li>
	<li>23000</li>
	<li>34000</li>
	<li>17000</li>
	<li>15000</li>
	<li>14000</li>
	<li>10000</li>
	<li>18000</li>
	<li>18000</li>
	<li>17000</li>
	<li>16000</li>
	<li>11000</li>
	<li>23000</li>
	<li>20000</li>
	<li>25000</li>
	<li>18000</li>
	<li>10000</li>
	<li>22000</li>
	<li>10000</li>
	<li>12000</li>
	<li>9000</li>
	<li>9000</li>
	<li>18000</li>
	<li>18000</li>
	<li>17000</li>
	<li>21000</li>
	<li>24000</li>
	<li>24000</li>
	<li>13000</li>
	<li>24000</li>
	<li>20000</li>
	<li>20000</li>
	<li>30000</li>
	<li>43000</li>
	<li>16000</li>
	<li>NULL</li>
	<li>25000</li>
	<li>27000</li>
	<li>25000</li>
	<li>25000</li>
	<li>10000</li>
	<li>18000</li>
	<li>19000</li>
	<li>33000</li>
	<li>14000</li>
	<li>5000</li>
	<li>10000</li>
	<li>8000</li>
	<li>5000</li>
	<li>8000</li>
	<li>24000</li>
	<li>10000</li>
	<li>22000</li>
	<li>8000</li>
	<li>8000</li>
	<li>33000</li>
	<li>19000</li>
	<li>5000</li>
	<li>10000</li>
	<li>14000</li>
	<li>22000</li>
	<li>12000</li>
	<li>10000</li>
	<li>26000</li>
	<li>24000</li>
	<li>21000</li>
	<li>14000</li>
	<li>12000</li>
	<li>24000</li>
	<li>22000</li>
	<li>22000</li>
	<li>12000</li>
	<li>22000</li>
	<li>15000</li>
	<li>18000</li>
	<li>13000</li>
	<li>23000</li>
	<li>34000</li>
	<li>13000</li>
	<li>19000</li>
	<li>17000</li>
	<li>20000</li>
	<li>4000</li>
	<li>4000</li>
	<li>30000</li>
	<li>18000</li>
	<li>17000</li>
	<li>18000</li>
	<li>18000</li>
	<li>15000</li>
	<li>30000</li>
	<li>14000</li>
	<li>14000</li>
	<li>34000</li>
	<li>30000</li>
	<li>36000</li>
	<li>23000</li>
	<li>22000</li>
	<li>35000</li>
	<li>17000</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>14000</li>
	<li>18000</li>
	<li>18000</li>
	<li>27000</li>
	<li>27000</li>
	<li>24000</li>
	<li>30000</li>
	<li>14000</li>
	<li>32000</li>
	<li>10000</li>
	<li>10000</li>
	<li>10000</li>
	<li>8000</li>
	<li>10000</li>
	<li>10000</li>
	<li>20000</li>
	<li>20000</li>
	<li>16000</li>
	<li>26000</li>
	<li>4000</li>
	<li>8000</li>
	<li>8000</li>
	<li>8000</li>
	<li>8000</li>
	<li>15000</li>
	<li>1000</li>
	<li>NULL</li>
	<li>8000</li>
	<li>8000</li>
	<li>NULL</li>
	<li>4000</li>
	<li>11000</li>
	<li>3000</li>
	<li>NULL</li>
	<li>14000</li>
	<li>3000</li>
	<li>24000</li>
	<li>12000</li>
	<li>12000</li>
	<li>1000</li>
	<li>1000</li>
	<li>20000</li>
	<li>3000</li>
	<li>10000</li>
	<li>10000</li>
	<li>2000</li>
	<li>13000</li>
	<li>13000</li>
	<li>25000</li>
	<li>11000</li>
	<li>7000</li>
	<li>9000</li>
	<li>9000</li>
	<li>25000</li>
	<li>2000</li>
	<li>6000</li>
	<li>11000</li>
	<li>10000</li>
	<li>7000</li>
	<li>15000</li>
	<li>2000</li>
	<li>15000</li>
	<li>22000</li>
	<li>10000</li>
	<li>3000</li>
	<li>15000</li>
	<li>15000</li>
	<li>6000</li>
	<li>14000</li>
	<li>22000</li>
	<li>10000</li>
	<li>3000</li>
	<li>5000</li>
	<li>5000</li>
	<li>1000</li>
	<li>14000</li>
	<li>14000</li>
	<li>16000</li>
	<li>NULL</li>
	<li>14000</li>
	<li>10000</li>
	<li>5000</li>
	<li>9000</li>
	<li>20000</li>
	<li>8000</li>
	<li>5000</li>
	<li>18000</li>
	<li>13000</li>
	<li>NULL</li>
	<li>13000</li>
	<li>13000</li>
	<li>13000</li>
	<li>10000</li>
	<li>14000</li>
	<li>14000</li>
	<li>NULL</li>
	<li>11000</li>
	<li>11000</li>
	<li>10000</li>
	<li>12000</li>
	<li>14000</li>
	<li>30000</li>
	<li>22000</li>
	<li>16000</li>
	<li>20000</li>
	<li>8000</li>
	<li>6000</li>
	<li>12000</li>
	<li>8000</li>
	<li>7000</li>
	<li>7000</li>
	<li>9000</li>
	<li>NULL</li>
	<li>4000</li>
	<li>23000</li>
	<li>4000</li>
	<li>13000</li>
	<li>1000</li>
	<li>5000</li>
	<li>1000</li>
	<li>3000</li>
	<li>3000</li>
	<li>4000</li>
	<li>5000</li>
	<li>1000</li>
	<li>6000</li>
	<li>16000</li>
	<li>2000</li>
	<li>1000</li>
	<li>6000</li>
	<li>4000</li>
	<li>4000</li>
	<li>4000</li>
	<li>7000</li>
	<li>1000</li>
	<li>6000</li>
	<li>20000</li>
	<li>16000</li>
	<li>1000</li>
	<li>7000</li>
	<li>2000</li>
	<li>11000</li>
	<li>10000</li>
	<li>12000</li>
	<li>NULL</li>
	<li>18000</li>
	<li>NULL</li>
	<li>1000</li>
	<li>4000</li>
	<li>1000</li>
	<li>27000</li>
	<li>8000</li>
	<li>1000</li>
	<li>18000</li>
	<li>4000</li>
	<li>6000</li>
	<li>4000</li>
	<li>2000</li>
	<li>2000</li>
	<li>5000</li>
	<li>6000</li>
	<li>NULL</li>
	<li>2000</li>
	<li>NULL</li>
	<li>2000</li>
	<li>13000</li>
	<li>20000</li>
	<li>14000</li>
	<li>12000</li>
	<li>18000</li>
	<li>19000</li>
	<li>19000</li>
	<li>13000</li>
	<li>14000</li>
	<li>3000</li>
	<li>4000</li>
	<li>1000</li>
	<li>10000</li>
	<li>1000</li>
	<li>5000</li>
	<li>NULL</li>
	<li>16000</li>
	<li>1000</li>
	<li>9000</li>
	<li>5000</li>
	<li>NULL</li>
	<li>14000</li>
	<li>9000</li>
	<li>6000</li>
	<li>11000</li>
	<li>5000</li>
	<li>1000</li>
	<li>1000</li>
	<li>4000</li>
	<li>1000</li>
	<li>1000</li>
	<li>16000</li>
	<li>15000</li>
	<li>23000</li>
	<li>4000</li>
	<li>19000</li>
	<li>1000</li>
	<li>1000</li>
	<li>NULL</li>
	<li>10000</li>
	<li>8000</li>
	<li>2000</li>
	<li>NULL</li>
	<li>4000</li>
	<li>9000</li>
	<li>7000</li>
	<li>3000</li>
	<li>8000</li>
	<li>2000</li>
	<li>4000</li>
	<li>25000</li>
	<li>3000</li>
	<li>3000</li>
	<li>12000</li>
	<li>8000</li>
	<li>5000</li>
	<li>4000</li>
	<li>19000</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>NULL</li>
	<li>15000</li>
	<li>10000</li>
	<li>4000</li>
</ol>





```R
idx = which( vehicle$price <= 500 & !is.na(vehicle$price) & !is.na(vehicle$maker))
vehicle[ idx, c("header", "price", "maker", "year") ]
```




<table>
<thead><tr><th></th><th scope=col>header</th><th scope=col>price</th><th scope=col>maker</th><th scope=col>year</th></tr></thead>
<tbody>
	<tr><th scope=row>posted1974</th><td>2005 Mini Cooper Sport</td><td>19</td><td>mini</td><td>2005</td></tr>
	<tr><th scope=row>posted17031</th><td>2014 Nissan Rogue SV AWD</td><td>340</td><td>nissan</td><td>2014</td></tr>
	<tr><th scope=row>posted18911</th><td>2003 Mercury marquis gs park lane</td><td>165</td><td>mercury</td><td>2003</td></tr>
	<tr><th scope=row>posted15112</th><td>2012 Audi Q5 3.2 Quattro Premium Plus</td><td>455</td><td>audi</td><td>2012</td></tr>
	<tr><th scope=row>posted4082</th><td>2014 Ford Explorer Limited</td><td>365</td><td>ford</td><td>2014</td></tr>
	<tr><th scope=row>posted21615</th><td>1957 CHEVROLET BELAIR</td><td>70</td><td>chevrolet</td><td>1957</td></tr>
	<tr><th scope=row>posted18374</th><td>2012 Audi Q5 3.2 Quattro Premium Plus</td><td>455</td><td>audi</td><td>2012</td></tr>
	<tr><th scope=row>posted2555</th><td>1997 Subaru legacy</td><td>80</td><td>subaru</td><td>1997</td></tr>
	<tr><th scope=row>posted18929</th><td>2014 Ford Explorer Limited</td><td>365</td><td>ford</td><td>2014</td></tr>
	<tr><th scope=row>posted9448</th><td>2012 Chrysler 300-Series</td><td>179</td><td>chrysler</td><td>2012</td></tr>
	<tr><th scope=row>posted29613</th><td>1998 ford e150 econoline</td><td>375</td><td>ford</td><td>1998</td></tr>
	<tr><th scope=row>posted128913</th><td>1992 Saturn SL2</td><td>80</td><td>saturn</td><td>1992</td></tr>
	<tr><th scope=row>posted229713</th><td>1961 chevrolet corvette convertible</td><td>53</td><td>chevrolet</td><td>1961</td></tr>
	<tr><th scope=row>posted229813</th><td>1961 chevrolet corvette convertible</td><td>53</td><td>chevrolet</td><td>1961</td></tr>
	<tr><th scope=row>posted229913</th><td>1961 chevrolet corvette convertible</td><td>53</td><td>chevrolet</td><td>1961</td></tr>
</tbody>
</table>





```R
vehicle = vehicle[-idx,]
```


```R
quantile(x = vehicle$price, probs = c(0.05,0.99), na.rm = TRUE)
```




<dl class=dl-horizontal>
	<dt>5%</dt>
		<dd>1350</dd>
	<dt>99%</dt>
		<dd>46000</dd>
</dl>





```R

```
