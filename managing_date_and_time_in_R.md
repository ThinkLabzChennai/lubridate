---
title: "Managing date and time using R"
author:    
    -  "RSN_Thinklabz_Chennai"

output:
  html_document:
    keep_md: true
    toc: yes
    df_print: paged

---






![time](https://user-images.githubusercontent.com/92445009/146159157-1e512564-b830-40a3-8a39-f249b322c95c.jpg)



# Preamble
In this notes we shall discuss the methods for handling date and time information in R. For this we can make use of **`base`** package and **lubridate** package which is part of Tidyverse package, to know more please go through [Introduction to Tidyverse](https://github.com/ThinkLabzChennai/Tidyverse_intro/blob/main/Introduction-to-Tidyverse.md).   
Our goal is to get familiar with date time objects in R and learn to perform operations available with those objects.


# lubridate
lubridate is a package in R that is dedicated to date and time information. It is good in handling  time related peculiarities. Time related information changes based on region and also for some regions it has day light saving, number of days in an year also changes during leap years, this is mentioned here as peculiarities(which may not happen for other types of data ). The methods available in lubridate for date time handling are robust to time zones, leap days and daylight saving time. Before diving into lubridate's functions let us get to know about date time class in R

# Date and time class in R

 ![clock](https://user-images.githubusercontent.com/92445009/146159171-98d49299-9bb0-4832-9ef3-a9ce4f92cc37.jpg)

   
   - In R programming Date is represented as "yyyy-mm-dd"
   
   - Dates are represented as the number of days since 1970-01-01, with negative values for earlier dates. This is called as Internal form. They are always printed according to current Gregorian calendar.   

   - Time is represented in R as "yyyy-mm-dd hour:minute:second   deviation from GMT"   
   
   - A week starts with Sunday   
   
   - Date variables mostly have data type "double" and object class "date".   
   
   - Date and time along with time zone is stored in R  as POSIX class.



# Loading packages and data needed
Let us load the package

```r
library(lubridate)
```

We shall use flights data from "nycflights13" package

```r
library(nycflights13)
data("flights")
```

Let us look at first five rows of the data

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["year"],"name":[1],"type":["int"],"align":["right"]},{"label":["month"],"name":[2],"type":["int"],"align":["right"]},{"label":["day"],"name":[3],"type":["int"],"align":["right"]},{"label":["dep_time"],"name":[4],"type":["int"],"align":["right"]},{"label":["sched_dep_time"],"name":[5],"type":["int"],"align":["right"]},{"label":["dep_delay"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["arr_time"],"name":[7],"type":["int"],"align":["right"]},{"label":["sched_arr_time"],"name":[8],"type":["int"],"align":["right"]},{"label":["arr_delay"],"name":[9],"type":["dbl"],"align":["right"]},{"label":["carrier"],"name":[10],"type":["chr"],"align":["left"]},{"label":["flight"],"name":[11],"type":["int"],"align":["right"]},{"label":["tailnum"],"name":[12],"type":["chr"],"align":["left"]},{"label":["origin"],"name":[13],"type":["chr"],"align":["left"]},{"label":["dest"],"name":[14],"type":["chr"],"align":["left"]},{"label":["air_time"],"name":[15],"type":["dbl"],"align":["right"]},{"label":["distance"],"name":[16],"type":["dbl"],"align":["right"]},{"label":["hour"],"name":[17],"type":["dbl"],"align":["right"]},{"label":["minute"],"name":[18],"type":["dbl"],"align":["right"]},{"label":["time_hour"],"name":[19],"type":["dttm"],"align":["right"]}],"data":[{"1":"2013","2":"1","3":"1","4":"517","5":"515","6":"2","7":"830","8":"819","9":"11","10":"UA","11":"1545","12":"N14228","13":"EWR","14":"IAH","15":"227","16":"1400","17":"5","18":"15","19":"2013-01-01 05:00:00"},{"1":"2013","2":"1","3":"1","4":"533","5":"529","6":"4","7":"850","8":"830","9":"20","10":"UA","11":"1714","12":"N24211","13":"LGA","14":"IAH","15":"227","16":"1416","17":"5","18":"29","19":"2013-01-01 05:00:00"},{"1":"2013","2":"1","3":"1","4":"542","5":"540","6":"2","7":"923","8":"850","9":"33","10":"AA","11":"1141","12":"N619AA","13":"JFK","14":"MIA","15":"160","16":"1089","17":"5","18":"40","19":"2013-01-01 05:00:00"},{"1":"2013","2":"1","3":"1","4":"544","5":"545","6":"-1","7":"1004","8":"1022","9":"-18","10":"B6","11":"725","12":"N804JB","13":"JFK","14":"BQN","15":"183","16":"1576","17":"5","18":"45","19":"2013-01-01 05:00:00"},{"1":"2013","2":"1","3":"1","4":"554","5":"600","6":"-6","7":"812","8":"837","9":"-25","10":"DL","11":"461","12":"N668DN","13":"LGA","14":"ATL","15":"116","16":"762","17":"6","18":"0","19":"2013-01-01 06:00:00"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

The following table gives an introduction about variables in the data and their type   

<table class="table table-bordered" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Variable </th>
   <th style="text-align:left;"> Description </th>
   <th style="text-align:left;"> Type </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> year </td>
   <td style="text-align:left;"> year of departure </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> month </td>
   <td style="text-align:left;"> month of departure </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> day </td>
   <td style="text-align:left;"> day of departure </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dep_time </td>
   <td style="text-align:left;"> actual departure time </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sched_dep_time </td>
   <td style="text-align:left;"> scheduled departure time </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dep_delay </td>
   <td style="text-align:left;"> departure delay in minutes </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> arr_time </td>
   <td style="text-align:left;"> actual arrival time </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sched_arr_time </td>
   <td style="text-align:left;"> scheduled arrival time </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> arr_delay </td>
   <td style="text-align:left;"> arrival delay in minutes </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> carrier </td>
   <td style="text-align:left;"> airline carrier abbreviation </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> flight </td>
   <td style="text-align:left;"> Flight number </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> tailnum </td>
   <td style="text-align:left;"> Plain tail number </td>
   <td style="text-align:left;"> string </td>
  </tr>
  <tr>
   <td style="text-align:left;"> origin </td>
   <td style="text-align:left;"> Flight origin </td>
   <td style="text-align:left;"> string </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dest </td>
   <td style="text-align:left;"> Flight destination </td>
   <td style="text-align:left;"> string </td>
  </tr>
  <tr>
   <td style="text-align:left;"> air_time </td>
   <td style="text-align:left;"> Time spent in air in minutes </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> distance </td>
   <td style="text-align:left;"> distance between airports in miles </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hour </td>
   <td style="text-align:left;"> scheduled departure hour </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> minute </td>
   <td style="text-align:left;"> scheduled departure minute </td>
   <td style="text-align:left;"> Numeric </td>
  </tr>
  <tr>
   <td style="text-align:left;"> time_hour </td>
   <td style="text-align:left;"> Scheduled date and hour of the flight as a POSIXct date </td>
   <td style="text-align:left;"> POSIXct </td>
  </tr>
</tbody>
</table>

We shall use some variables from this data to learn lubridate

# Managing date and time objects in R

## Current system date and time extraction 
**`today()`** can be used to extract current date

```r
today()
```

```
## [1] "2021-12-15"
```


   **`now()`** can be used to obtain current system date and time

```r
now()
```

```
## [1] "2021-12-15 15:12:10 IST"
```


## Assignment of Date objects
Date objects can be assigned easily using functions **`ymd`**, **`mdy`**, **`dmy`**. These functions transform dates stored in character and numeric vectors to Date objects. These functions recognize arbitrary non-digit separators as well as no separator. As long as the order of formats is correct, these functions will parse dates correctly even when the input vectors contain differently formatted dates.   
   
   Let us form a new variable by joining 'year', 'month' and 'date' variables For this we use **`unite()`** function from **`tidyr`** package to know more about this package please go through [Data structuring with tidyr](https://rpubs.com/Thinklabz/tidyr) 

```r
library(tidyr)
data=unite(data=flights[c("year","month","day")],col=date,sep=" ",remove=F)
```

Let us look at first few rows of data
<table class="table table-bordered" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> date </th>
   <th style="text-align:right;"> year </th>
   <th style="text-align:right;"> month </th>
   <th style="text-align:right;"> day </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 2013 1 1 </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013 1 1 </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013 1 1 </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013 1 1 </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013 1 1 </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013 1 1 </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
</tbody>
</table>

The function **`ymd()`** is used to convert string or numeric object in to date object.


```r
date_converted=ymd(data$date)
##Let us look at the date
head(date_converted)
```

```
## [1] "2013-01-01" "2013-01-01" "2013-01-01" "2013-01-01" "2013-01-01"
## [6] "2013-01-01"
```

```r
class(date_converted)
```

```
## [1] "Date"
```

**Note : There are extremely weird cases when one of the separators is "" and some of the formats are not in double digits might not be parsed correctly**
   
Example :

```r
# 2021 march 1
ymd('202131')
```

```
## [1] NA
```

```r
# 2021 january 12
ymd(2021112)
```

```
## [1] "0202-11-12"
```

We can overcome this by adding a separator 


```r
ymd("2021/1/31")
```

```
## [1] "2021-01-31"
```

similarly we can convert dates in format 'month date year' or 'date month year'. Let us see some examples


```r
mdy(c('febr 1 2021','jun 30 2021'))
```

```
## [1] "2021-02-01" "2021-06-30"
```

```r
dmy(c('1 1 21',"4 2 21"))
```

```
## [1] "2021-01-01" "2021-02-04"
```

## Assignment of Date Time objects

Date time objects can be assigned using **`ymd_hms()`**, **`mdy_hms()`**, **`dmy_hms()`** . This function transforms dates stored as character or numeric vectors to POSIXct objects. 
   
   For example we shall use the following data

```r
data2=unite(flights[c("day","month","year","hour","minute")],
col=dt,sep="-",remove = F)
#First few rows of data
head(data2)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["dt"],"name":[1],"type":["chr"],"align":["left"]},{"label":["day"],"name":[2],"type":["int"],"align":["right"]},{"label":["month"],"name":[3],"type":["int"],"align":["right"]},{"label":["year"],"name":[4],"type":["int"],"align":["right"]},{"label":["hour"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["minute"],"name":[6],"type":["dbl"],"align":["right"]}],"data":[{"1":"1-1-2013-5-15","2":"1","3":"1","4":"2013","5":"5","6":"15"},{"1":"1-1-2013-5-29","2":"1","3":"1","4":"2013","5":"5","6":"29"},{"1":"1-1-2013-5-40","2":"1","3":"1","4":"2013","5":"5","6":"40"},{"1":"1-1-2013-5-45","2":"1","3":"1","4":"2013","5":"5","6":"45"},{"1":"1-1-2013-6-0","2":"1","3":"1","4":"2013","5":"6","6":"0"},{"1":"1-1-2013-5-58","2":"1","3":"1","4":"2013","5":"5","6":"58"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

The function **`dmy_hm()`** is used to convert string or numeric object in to date time(POSIXct) object. we can specify the timezone using **`tz`** argument **Please note that the timezone availability differs system to system**


```r
date_time=dmy_hm(data2$dt,tz="EST")
```

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> x </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 2013-01-01 05:15:00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013-01-01 05:29:00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013-01-01 05:40:00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013-01-01 05:45:00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013-01-01 06:00:00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013-01-01 05:58:00 </td>
  </tr>
</tbody>
</table>

We can change date format as in the below examples

```r
# month date year hour minute
mdy_hm(c("2/22/2021/12/5","6/29/2021/19/51"),tz="UTC")
```

```
## [1] "2021-02-22 12:05:00 UTC" "2021-06-29 19:51:00 UTC"
```

```r
# month date year hour 
mdy_h("12-23-2021-10-pm")
```

```
## [1] "2021-12-23 22:00:00 UTC"
```

```r
# month date year hour minute second
mdy_hms(c("2/22/2021/12/5/22","6/29/2021/19/51/59"),tz="UTC")
```

```
## [1] "2021-02-22 12:05:22 UTC" "2021-06-29 19:51:59 UTC"
```

```r
# year date month hour minute second
ydm_hms("2021 30 5 3 35 22 pm",tz="EST")
```

```
## [1] "2021-05-30 15:35:22 EST"
```

The following may be used to get a quick view of available options
<table>
<caption>Ready reckoner for lubridate</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Order_of_elements_in_date_time </th>
   <th style="text-align:left;"> Parse_function </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> year, month day </td>
   <td style="text-align:left;"> ymd() </td>
  </tr>
  <tr>
   <td style="text-align:left;"> year, day, month </td>
   <td style="text-align:left;"> ydm() </td>
  </tr>
  <tr>
   <td style="text-align:left;"> month, day, year </td>
   <td style="text-align:left;"> mdy() </td>
  </tr>
  <tr>
   <td style="text-align:left;"> day, month, year </td>
   <td style="text-align:left;"> dmy() </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hour, minute </td>
   <td style="text-align:left;"> hm() </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hour, minute,second </td>
   <td style="text-align:left;"> hms() </td>
  </tr>
  <tr>
   <td style="text-align:left;"> year, month, day, hour, minute, second </td>
   <td style="text-align:left;"> ymd_hms() </td>
  </tr>
  <tr>
   <td style="text-align:left;"> year, month, day, hour, minute </td>
   <td style="text-align:left;"> ymd_hm() </td>
  </tr>
  <tr>
   <td style="text-align:left;"> year, month, day, hour </td>
   <td style="text-align:left;"> ymd_h() </td>
  </tr>
</tbody>
</table>
   
## Merging data to form date
  We can merge different columns of data having year, month and day in to date using **`ISOdate()`** . **`year`**, **`month`**, **`day`**, **`hour`**, **`min`**, **`sec`** and **`tz`**  arguments are used to specify which column is which part of date. 

```r
ISOdate(year = flights$year, 
        month =flights$month,
        day =flights$day,
        hour=flights$hour,
        min=flights$minute )->t
```

   The resulting data will look like this
<table class="table table-bordered" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> x </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 2013-01-01 05:15:00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013-01-01 05:29:00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013-01-01 05:40:00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013-01-01 05:45:00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013-01-01 06:00:00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2013-01-01 05:58:00 </td>
  </tr>
</tbody>
</table>

## Number to date
Numbers in Internal form (as integer) can be converted in to date using **`as_date()`**.   
We specify number of days from origin and obtain date


```r
# default origin
as_date(18808)
```

```
## [1] "2021-06-30"
```

```r
# specific origin (can be given as string)
as_date(c(234,22),origin="2020-12-31")
```

```
## [1] "2021-08-22" "2021-01-22"
```

**`as_datetime()`** can be used to obtain time from origin using Number of seconds 

```r
as_datetime(60)
```

```
## [1] "1970-01-01 00:01:00 UTC"
```

```r
as_datetime(60*60*24)
```

```
## [1] "1970-01-02 UTC"
```

## Decimal form of date in a year
**`decimal_date()`** is used to convert a date into a decimal number of year. Digits in decimal represent the position of our date in the year    
example 

```r
decimal_date(dmy(30062021))
```

```
## [1] 2021.493
```

**`date_decimal()`** is used to convert decimal form to date

```r
date_decimal(2021.493)
```

```
## [1] "2021-06-29 22:40:47 UTC"
```

## Parsing date time
**`parse_date_time()`** parses an input vector into POSIXct date-time object. There are two advantages of using this function.  First, it allows specification of the order in which the formats occur without the need to include separators and the % prefix using **`orders`** arguent. Second, it allows the user to specify several format-orders to handle heterogeneous date-time character representations.    
Example


```r
x <- c("21-01-01", "07-01-02", "09-01-03")
parse_date_time(x,orders =  "ymd")
```

```
## [1] "2021-01-01 UTC" "2007-01-02 UTC" "2009-01-03 UTC"
```


```r
# heterogeneous date times
x <- c("20-01-01", "240102", "15-01 03", "90-01-03 12:02")
parse_date_time(x,orders =  c("ymd", "ymd HM"))
```

```
## [1] "2020-01-01 00:00:00 UTC" "2024-01-02 00:00:00 UTC"
## [3] "2015-01-03 00:00:00 UTC" "1990-01-03 12:02:00 UTC"
```


```r
#  truncated time-dates 
x <- c("2011-12-31 12:59:59", "2010-01-01 12:11",
       "2010-01-01 12", "2010-01-01")
parse_date_time(x, "Ymd HMS", truncated = 3)
```

```
## [1] "2011-12-31 12:59:59 UTC" "2010-01-01 12:11:00 UTC"
## [3] "2010-01-01 12:00:00 UTC" "2010-01-01 00:00:00 UTC"
```

## Extracting and modifying components of date time objects
We can either extract or modify various components of date time objects using following functions.   
For this let us use the following data


```r
datan=ymd_hms(c("2021 12 21 08 42 22 ",
                "2021 10 2 23 22 30 ",
                "2021 3 11 16 12 2 "))

datan
```

```
## [1] "2021-12-21 08:42:22 UTC" "2021-10-02 23:22:30 UTC"
## [3] "2021-03-11 16:12:02 UTC"
```

**`date()`** used to extract day of month

```r
date(datan)
```

```
## [1] "2021-12-21" "2021-10-02" "2021-03-11"
```

**`month()`** is used to extract month of the year. Default is number,  

```r
month(datan)
```

```
## [1] 12 10  3
```
**`label`** argument is used to obtain month names. 

```r
month(datan,label=T)
```

```
## [1] Dec Oct Mar
## 12 Levels: Jan < Feb < Mar < Apr < May < Jun < Jul < Aug < Sep < ... < Dec
```
By default these are abbreviated, to get full name we use **`abbr`** argument

```r
month(datan,label=T,abbr=F)
```

```
## [1] December October  March   
## 12 Levels: January < February < March < April < May < June < ... < December
```


**`year`** is used to extract year from date object. **`isoyear()`** returns years according to the ISO 8601 week calendar.  **`epiyear()`** returns years according to the epidemiological week calendars.

```r
year(datan)
```

```
## [1] 2021 2021 2021
```

day of the month can be extracted using **`day()`**

```r
day(datan)
```

```
## [1] 21  2 11
```


We can find which day of the week given date is using **`wday()`**

```r
wday(datan)
```

```
## [1] 3 7 5
```

**`label`** argument is used to obtain day names.

```r
wday(datan,label=T)
```

```
## [1] Tue Sat Thu
## Levels: Sun < Mon < Tue < Wed < Thu < Fri < Sat
```

By default these are abbreviated, to get full name we use **`abbr`** argument

```r
wday(datan,label=T,abbr=F)
```

```
## [1] Tuesday  Saturday Thursday
## 7 Levels: Sunday < Monday < Tuesday < Wednesday < Thursday < ... < Saturday
```

We can find which day of the quarter using **`qday()`**

```r
qday(datan)
```

```
## [1] 82  2 70
```

For finding out which week the date occurs **`week()`**

```r
week(datan)
```

```
## [1] 51 40 10
```



**`hour()`** is used to extract hour from date

```r
hour(datan)
```

```
## [1]  8 23 16
```

**`minute()`** is used to extract minute component from the date

```r
minute(datan)
```

```
## [1] 42 22 12
```

**`second()`** is used to extract second component from the date

```r
second(datan)
```

```
## [1] 22 30  2
```

**`semester`** is used to find which semester the date belongs

```r
semester(datan)
```

```
## [1] 2 2 1
```

We can use the components to assign/ change the value in the date one example is shown below

```r
data12=ymd_hms(c("2019 12 2 2 34 22","2020 05 14 9 8 7"))
data12
```

```
## [1] "2019-12-02 02:34:22 UTC" "2020-05-14 09:08:07 UTC"
```

```r
# Changing hour component
hour(data12)=c(1,2)
data12
```

```
## [1] "2019-12-02 01:34:22 UTC" "2020-05-14 02:08:07 UTC"
```
In similar way we change set other components also

**`am() / pm()`** used to check which time is recorded

```r
am(datan)
```

```
## [1]  TRUE FALSE FALSE
```

```r
pm(datan)
```

```
## [1] FALSE  TRUE  TRUE
```

**`leap_year()`** can be used to check whether it is leap year or not

```r
leap_year(datan)
```

```
## [1] FALSE FALSE FALSE
```


## Approximating dates 
Date time objects can be approximated in different ways as below

### Floor
**`floor()`** can be used to round the date time to lower nearest value. **`unit`** argument is used to specify which component we consider for rounding

```r
datan
```

```
## [1] "2021-12-21 08:42:22 UTC" "2021-10-02 23:22:30 UTC"
## [3] "2021-03-11 16:12:02 UTC"
```

```r
floor_date(datan,unit='month')
```

```
## [1] "2021-12-01 UTC" "2021-10-01 UTC" "2021-03-01 UTC"
```

### Round
**`round()`** is used to round to nearest date

```r
round_date(datan,unit="day")
```

```
## [1] "2021-12-21 UTC" "2021-10-03 UTC" "2021-03-12 UTC"
```

### Ceiling
**`ceiling_date`** is used to round to higher nearest date

```r
ceiling_date(datan,unit="hour")
```

```
## [1] "2021-12-21 09:00:00 UTC" "2021-10-03 00:00:00 UTC"
## [3] "2021-03-11 17:00:00 UTC"
```

### Rollback
**`rollback`** is used to change the month to previous month and date as last date of month

```r
rollback(datan)
```

```
## [1] "2021-11-30 08:42:22 UTC" "2021-09-30 23:22:30 UTC"
## [3] "2021-02-28 16:12:02 UTC"
```

**`roll_to_first`** argument is used to go to first day of previous month
**`preserve_hms`** argument is used to keep the time as it is 

```r
rollback(datan,roll_to_first = T,preserve_hms = T)
```

```
## [1] "2021-12-01 08:42:22 UTC" "2021-10-01 23:22:30 UTC"
## [3] "2021-03-01 16:12:02 UTC"
```

## Timezone handling
We can change time saved in one timezone to another using **`with_tz()`**. **`tzone`** is used to specify time zone.

```r
# Original time
datan
```

```
## [1] "2021-12-21 08:42:22 UTC" "2021-10-02 23:22:30 UTC"
## [3] "2021-03-11 16:12:02 UTC"
```

```r
# Converted time
with_tz(datan,tzone="EST")
```

```
## [1] "2021-12-21 03:42:22 EST" "2021-10-02 18:22:30 EST"
## [3] "2021-03-11 11:12:02 EST"
```

Without converting time we can change timezone using **`force_tz()`**

```r
force_tz(datan,tzone='EST')
```

```
## [1] "2021-12-21 08:42:22 EST" "2021-10-02 23:22:30 EST"
## [3] "2021-03-11 16:12:02 EST"
```

## Stamp date times
**`stamp()`** can be used to create template from example string and we can apply it to new date time

```r
eg=stamp("The example is created for Thursday 1 june,2021 9:20")
eg(datan)
```

```
## [1] "The example is created for Tuesday 21 December,2021 08:42"
## [2] "The example is created for Saturday 02 October,2021 23:22"
## [3] "The example is created for Thursday 11 March,2021 16:12"
```


## Daylight saving
Day light saving is a practice of advancing clocks by one hour during warmer months i.e during late winter or early spring so that darkness falls at a later clock time and set clocks back by one hour in autumn.  

### Check for daylight saving
logical. TRUE if DST is in force, FALSE if not, NA if unknown.

```r
dst(datan)
```

```
## [1] FALSE FALSE FALSE
```

## Some mathematical operations with date time objects

We can perform some mathematical operations using date time objects to obtain other date. Remember we have mentioned dates are saved in Internal form as integers. so we can add or subtract integers to dates to obtain other date   

example 
We can move forward to a date using number of days using '+'

```r
data1=ymd(c('2021 3 2','2020 12 24'))
# 30 days from our dates
data11=data1+31
```

Difference between two dates can be found using '-'

```r
data11-data1
```

```
## Time differences in days
## [1] 31 31
```

We can go back to number of days using '-' operator

```r
data1-20
```

```
## [1] "2021-02-10" "2020-12-04"
```



```r
# examples with time zones
x <- ymd_hms("2015-09-22 01:00:00", tz = "US/Eastern")
y <- ymd_hms("2015-09-22 01:00:00", tz = "US/Pacific")
z <- ymd_hms("2019-09-22 21:00:00", tz = "US/Pacific")

# moving 3 days 19 hours 
x+days(4)-hours(5)
```

```
## [1] "2015-09-25 20:00:00 EDT"
```

```r
# Checking for equality
y == x
```

```
## [1] FALSE
```

```r
# Difference between two different timezones
y - x
```

```
## Time difference of 3 hours
```

```r
# same timezone
z-y
```

```
## Time difference of 1461.833 days
```

## Sequence generation using hours
Creating sequences with time is very similar to sequences with numbers. However, we need to make sure it is date object. This is done by **`seq()`** and **`by`** argument is used to specify difference between elements of the sequence.

```r
# Using days
seq(ymd("2015-1-1"), 
    ymd("2015-1-15"),
    by = "2 days")
```

```
## [1] "2015-01-01" "2015-01-03" "2015-01-05" "2015-01-07" "2015-01-09"
## [6] "2015-01-11" "2015-01-13" "2015-01-15"
```


```r
# Using hours to generate sequence
seq(ymd_hms("2015 1 1 4 30 22",tz='EST'),
    ymd_hms("2015 1 1 7 31 00",tz='EST'),
    by= "hours")
```

```
## [1] "2015-01-01 04:30:22 EST" "2015-01-01 05:30:22 EST"
## [3] "2015-01-01 06:30:22 EST" "2015-01-01 07:30:22 EST"
```


```r
# Using months 
seq(ymd("2025-1-1"), ymd("2025-12-15"), by = "4 months")
```

```
## [1] "2025-01-01" "2025-05-01" "2025-09-01"
```

## Periods
Periods track changes in clock times, this ignore time line irregularities. we can define a period using Following commands
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Function </th>
   <th style="text-align:left;"> Period.created </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> years(x ) </td>
   <td style="text-align:left;"> x years </td>
  </tr>
  <tr>
   <td style="text-align:left;"> months(x) </td>
   <td style="text-align:left;"> x months </td>
  </tr>
  <tr>
   <td style="text-align:left;"> weeks(x) </td>
   <td style="text-align:left;"> x weeks </td>
  </tr>
  <tr>
   <td style="text-align:left;"> days(x ) </td>
   <td style="text-align:left;"> x days </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hours(x ) </td>
   <td style="text-align:left;"> x hours </td>
  </tr>
  <tr>
   <td style="text-align:left;"> minutes(x ) </td>
   <td style="text-align:left;"> x minutes </td>
  </tr>
  <tr>
   <td style="text-align:left;"> seconds(x ) </td>
   <td style="text-align:left;"> x seconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> milliseconds(x ) </td>
   <td style="text-align:left;"> x milliseconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> microseconds(x ) </td>
   <td style="text-align:left;"> x microseconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nanoseconds(x ) </td>
   <td style="text-align:left;"> x nanoseconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> picoseconds(x ) </td>
   <td style="text-align:left;"> x picoseconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> x represents a number </td>
   <td style="text-align:left;">  </td>
  </tr>
</tbody>
</table>


for example 

```r
seconds(2)
```

```
## [1] "2S"
```

```r
months(2)
```

```
## [1] "2m 0d 0H 0M 0S"
```

These periods have some drawbacks, This happens because of daylight saving time and leap year, consider the following dates

A normal day

```r
normal <- ymd_hms("2018-01-01 01:30:00",tz="US/Eastern")
```

The start of daylight savings (spring forward)

```r
gap_day <- ymd_hms("2018-03-11 01:30:00",tz="US/Eastern")
```

The end of daylight savings (fall back)

```r
lap_day <- ymd_hms("2018-11-04 00:30:00",tz="US/Eastern")
```

Leap years and leap seconds

```r
leap_day <- ymd("2023-03-01")
```

The following example holds correct since it is normal day

```r
normal + minutes(90)
```

```
## [1] "2018-01-01 03:00:00 EST"
```

In day light saving start day there will be a skip of one hour in clock and this needs to be taken into account. 

```r
gap_day
```

```
## [1] "2018-03-11 01:30:00 EST"
```

```r
gap_day+minutes(90)
```

```
## [1] "2018-03-11 03:00:00 EDT"
```
We must be getting 4 pm because of daylight saving in the region. but this period doesn't consider it
    
  At the end of daylight saving period we need to take in to account of clock changing back to 1 hour

```r
lap_day
```

```
## [1] "2018-11-04 00:30:00 EDT"
```

```r
lap_day+minutes(90)
```

```
## [1] "2018-11-04 02:00:00 EST"
```

And leap year calculation  

```r
leap_day+years(1)
```

```
## [1] "2024-03-01"
```

The above miscalculations can be handled by using 'durations' available in lubridate

## Durations   

Durations simply measure the time span between start and end dates. They track the passage of
physical time, which deviates from
clock time when irregularities occur. they can be obtained using 
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Function </th>
   <th style="text-align:left;"> Duration </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> dyears(x) </td>
   <td style="text-align:left;"> 31536000x seconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dmonths(x) </td>
   <td style="text-align:left;"> 2629800x seconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dweeks(x) </td>
   <td style="text-align:left;"> 604800x seconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ddays(x) </td>
   <td style="text-align:left;"> 86400x seconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dhours(x) </td>
   <td style="text-align:left;"> 3600x seconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dminutes(x) </td>
   <td style="text-align:left;"> 60x seconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dseconds(x) </td>
   <td style="text-align:left;"> x seconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dmilliseconds(x) </td>
   <td style="text-align:left;"> x 10^-3 Seconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dmicroseconds(x) </td>
   <td style="text-align:left;"> x 10^-6 Seconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dnanoseconds(x) </td>
   <td style="text-align:left;"> x 10^-9 Seconds </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dpicoseconds(x) </td>
   <td style="text-align:left;"> x 10^-12 Seconds </td>
  </tr>
</tbody>
</table>


```r
normal + dminutes(90)
```

```
## [1] "2018-01-01 03:00:00 EST"
```


```r
gap_day + dminutes(90)
```

```
## [1] "2018-03-11 04:00:00 EDT"
```


```r
lap_day+dminutes(90)
```

```
## [1] "2018-11-04 01:00:00 EST"
```


```r
leap_day + dyears(1)
```

```
## [1] "2024-02-29 06:00:00 UTC"
```

We can see that problems discussed with period have been solved.

## Intervals 
They represent specific intervals
of the timeline, bounded by start and end date-times.
**`interval(a,b)`** is used to create interval from a to b

```r
i=interval(normal, normal + minutes(90))
i
```

```
## [1] 2018-01-01 01:30:00 EST--2018-01-01 03:00:00 EST
```

```r
j=interval(datan,leap_day)
j
```

```
## [1] 2021-12-21 08:42:22 UTC--2023-03-01 UTC
## [2] 2021-10-02 23:22:30 UTC--2023-03-01 UTC
## [3] 2021-03-11 16:12:02 UTC--2023-03-01 UTC
```

**`a %within% b`** used to check whether interval or date-time a fall within interval b


```r
i  %within%  j
```

```
## [1] FALSE FALSE FALSE
```

# Final Note
We have seen how we can handle date and time objects in R using base and lubridate packages. We have learnt how to 
   
   + Assignment
   + Conversion
   + Components of date time objects
   + Daylight saving handling
   + Approximations
   + Timezone handling
   + Using stamps
   
We have given introduction to almost all the important tools to handle date time components in R. We have used **`ISOdate()`** and **`seq()`** from base package, **`unite()`** from tidyr package and all other functions are from lubridate package.


