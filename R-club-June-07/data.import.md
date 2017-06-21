# Data.import
Veronica  
6/5/2017  



## Data Import

#### 11.2 Getting Started Exercises  

**1. What function would you use to read a file where fields were separated with
“|”?**  
read_delim() can be used to read files if delim = "|"

```r
read_delim("delim.txt", delim = "|")
```

```
## Parsed with column specification:
## cols(
##   a = col_integer(),
##   b = col_integer(),
##   c = col_integer(),
##   d = col_integer(),
##   f = col_integer()
## )
```

```
## # A tibble: 1 × 5
##       a     b     c     d     f
##   <int> <int> <int> <int> <int>
## 1     1     2     3     4     5
```

**2. Apart from file, skip, and comment, what other arguments do read_csv() and read_tsv() have in common?**  
Both commands share all of the same arguments.

**3. What are the most important arguments to read_fwf()?**  
path and col_positions are the most important arguments of read_fwf().

**4. Sometimes strings in a CSV file contain commas. To prevent them from causing problems they need to be surrounded by a quoting character, like " or '. By convention, read_csv() assumes that the quoting character will be ", and if you want to change it you’ll need to use read_delim() instead. What arguments do you need to specify to read the following text into a data frame?**  

```r
read_delim(delim = ",", quote = "'" , "x,y\n1,'a,b'")
```

```
## # A tibble: 1 × 2
##       x     y
##   <int> <chr>
## 1     1   a,b
```

**5. Identify what is wrong with each of the following inline CSV files. What happens when you run the code?**  

```r
#read_csv("a,b\n1,2,3\n4,5,6")
# add an c column
read_csv("a,b,c\n1,2,3\n4,5,6")
```

```
## # A tibble: 2 × 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2     3
## 2     4     5     6
```

```r
#read_csv("a,b,c\n1,2\n1,2,3,4")
#whats happening here?
read_csv("a,b,c\n1,2\n1,2,3,4")
```

```
## Warning: 2 parsing failures.
## row col  expected    actual
##   1  -- 3 columns 2 columns
##   2  -- 3 columns 4 columns
```

```
## # A tibble: 2 × 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2    NA
## 2     1     2     3
```

```r
#read_csv("a,b\n\"1")
#wrong coltype
read_csv(col_types = "cc", "a,b\n\"1")
```

```
## Warning: 2 parsing failures.
## row col                     expected    actual
##   1  a  closing quote at end of file          
##   1  -- 2 columns                    1 columns
```

```
## # A tibble: 1 × 2
##       a     b
##   <chr> <chr>
## 1     1  <NA>
```

```r
#read_csv("a,b\n1,2\na,b")
#not sure what should be changed
read_csv("a,b\n1,2\na,b")
```

```
## # A tibble: 2 × 2
##       a     b
##   <chr> <chr>
## 1     1     2
## 2     a     b
```

```r
#read_csv("a;b\n1;3")
#wrong delim, use read_csv2()
read_csv2("a;b\n1;3")
```

```
## # A tibble: 1 × 2
##       a     b
##   <int> <int>
## 1     1     3
```


#### Parsing a Vector Exercises

**1. What are the most important arguments to locale()?**  
The most important arguments for locale() seems dot be date_format, which sets the default input so that it is parsed as a date. Date_names, demical_mark and grouping mark could also be very important if collaborating outside of the US. 

**2. What happens if you try and set decimal_mark and grouping_mark to the same character? What happens to the default value of grouping_mark when you set decimal_mark to “,”? What happens to the default value of decimal_mark when you set the grouping_mark to “.”?**  

```r
#locale(demical_mark = ".", groping grouping_mark = ".")
#Setting decimal_mark and grouping_mark to the same character returns an error

locale(decimal_mark = ",")
```

```
## <locale>
## Numbers:  123.456,78
## Formats:  %AD / %AT
## Timezone: UTC
## Encoding: UTF-8
## <date_names>
## Days:   Sunday (Sun), Monday (Mon), Tuesday (Tue), Wednesday (Wed),
##         Thursday (Thu), Friday (Fri), Saturday (Sat)
## Months: January (Jan), February (Feb), March (Mar), April (Apr), May
##         (May), June (Jun), July (Jul), August (Aug), September
##         (Sep), October (Oct), November (Nov), December (Dec)
## AM/PM:  AM/PM
```

```r
#The grouping_mark has changed to ".".
default_locale()
```

```
## <locale>
## Numbers:  123,456.78
## Formats:  %AD / %AT
## Timezone: UTC
## Encoding: UTF-8
## <date_names>
## Days:   Sunday (Sun), Monday (Mon), Tuesday (Tue), Wednesday (Wed),
##         Thursday (Thu), Friday (Fri), Saturday (Sat)
## Months: January (Jan), February (Feb), March (Mar), April (Apr), May
##         (May), June (Jun), July (Jul), August (Aug), September
##         (Sep), October (Oct), November (Nov), December (Dec)
## AM/PM:  AM/PM
```

```r
locale(grouping_mark = ".")
```

```
## <locale>
## Numbers:  123.456,78
## Formats:  %AD / %AT
## Timezone: UTC
## Encoding: UTF-8
## <date_names>
## Days:   Sunday (Sun), Monday (Mon), Tuesday (Tue), Wednesday (Wed),
##         Thursday (Thu), Friday (Fri), Saturday (Sat)
## Months: January (Jan), February (Feb), March (Mar), April (Apr), May
##         (May), June (Jun), July (Jul), August (Aug), September
##         (Sep), October (Oct), November (Nov), December (Dec)
## AM/PM:  AM/PM
```

```r
#The decimal_mark has changed to ",".
default_locale()
```

```
## <locale>
## Numbers:  123,456.78
## Formats:  %AD / %AT
## Timezone: UTC
## Encoding: UTF-8
## <date_names>
## Days:   Sunday (Sun), Monday (Mon), Tuesday (Tue), Wednesday (Wed),
##         Thursday (Thu), Friday (Fri), Saturday (Sat)
## Months: January (Jan), February (Feb), March (Mar), April (Apr), May
##         (May), June (Jun), July (Jul), August (Aug), September
##         (Sep), October (Oct), November (Nov), December (Dec)
## AM/PM:  AM/PM
```

**3. I didn’t discuss the date_format and time_format options to locale(). What do they do? Construct an example that shows when they might be useful.**  
date_format and time_format change the default formats for their respective categories.  

```r
x <- c("02/03/04", "12/11/10")

#parse_date(x)
#parsing fails

parse_date(x, locale = locale(date_format = "%m/%d/%y"))
```

```
## [1] "2004-02-03" "2010-12-11"
```

```r
parse_date(x, locale = locale(date_format = "%y/%m/%d"))
```

```
## [1] "2002-03-04" "2012-11-10"
```

**4. If you live outside the US, create a new locale object that encapsulates the settings for the types of file you read most commonly.**

```r
locale("de", decimal_mark = ",", grouping_mark = " ")
```

```
## <locale>
## Numbers:  123 456,78
## Formats:  %AD / %AT
## Timezone: UTC
## Encoding: UTF-8
## <date_names>
## Days:   Sonntag (So.), Montag (Mo.), Dienstag (Di.), Mittwoch (Mi.),
##         Donnerstag (Do.), Freitag (Fr.), Samstag (Sa.)
## Months: Januar (Jan.), Februar (Feb.), März (März), April (Apr.), Mai
##         (Mai), Juni (Juni), Juli (Juli), August (Aug.), September
##         (Sep.), Oktober (Okt.), November (Nov.), Dezember (Dez.)
## AM/PM:  vorm./nachm.
```

```r
n <- c("100 000,3", "300 001,3", "444 444 444,4")
parse_number(n, locale = locale("de", decimal_mark = ",", grouping_mark = " "))
```

```
## [1]    100000.3    300001.3 444444444.4
```

**5. What’s the difference between read_csv() and read_csv2()?**  
read_csv() reads files here the feild separator is "," while read_csv2() read files that use ";"

**6. What are the most common encodings used in Europe? What are the most common encodings used in Asia? Do some googling to find out.**  
ISO 8859 seems to have individual encodings for most of the languages in Europe.  
Various chinese sites use GBK, UTF-8, and GB2312. Other Asian websites appear to favor UTF-8.
http://xahlee.info/w/what_encoding_do_chinese_websites_use.html

**7. Generate the correct format string to parse each of the following dates and times:**  

```r
d1 <- "January  1, 2010"
d2 <- "2015-Mar-07"
d3 <- "06-Jun-2017"
d4 <- c("August 19 (2015)", "July 1 (2015)")
d5 <- "12/30/14" # Dec 30, 2014
t1 <- "1705"
t2 <- "11:15:10.12 PM"

d1
```

```
## [1] "January  1, 2010"
```

```r
parse_date(d1, "%B %d, %Y")
```

```
## [1] "2010-01-01"
```

```r
d2
```

```
## [1] "2015-Mar-07"
```

```r
parse_date(d2, "%Y-%b-%d")
```

```
## [1] "2015-03-07"
```

```r
d3
```

```
## [1] "06-Jun-2017"
```

```r
parse_date(d3, "%d-%b-%Y")
```

```
## [1] "2017-06-06"
```

```r
d4
```

```
## [1] "August 19 (2015)" "July 1 (2015)"
```

```r
parse_date(d4, "%B %d (%Y)")
```

```
## [1] "2015-08-19" "2015-07-01"
```

```r
d5
```

```
## [1] "12/30/14"
```

```r
parse_date(d5, "%D")
```

```
## [1] "2014-12-30"
```

```r
t1
```

```
## [1] "1705"
```

```r
parse_time(t1, "%H%M")
```

```
## 17:05:00
```

```r
t2
```

```
## [1] "11:15:10.12 PM"
```

```r
parse_time(t2, "%H:%M:%OS %p")
```

```
## 23:15:10.12
```












