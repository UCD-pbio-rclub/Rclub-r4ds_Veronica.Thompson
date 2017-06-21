# Tidy.Data1
Veronica  
6/19/2017  



## Tidy Data

#### 12.2 Tidy Data

**1. Using prose, describe how the variables and observations are organised in each of the sample tables.**  

```r
table1
```

```
## # A tibble: 6 × 4
##       country  year  cases population
##         <chr> <int>  <int>      <int>
## 1 Afghanistan  1999    745   19987071
## 2 Afghanistan  2000   2666   20595360
## 3      Brazil  1999  37737  172006362
## 4      Brazil  2000  80488  174504898
## 5       China  1999 212258 1272915272
## 6       China  2000 213766 1280428583
```

```r
#each row contains one obersvation and each column contains one variable.
table2
```

```
## # A tibble: 12 × 4
##        country  year       type      count
##          <chr> <int>      <chr>      <int>
## 1  Afghanistan  1999      cases        745
## 2  Afghanistan  1999 population   19987071
## 3  Afghanistan  2000      cases       2666
## 4  Afghanistan  2000 population   20595360
## 5       Brazil  1999      cases      37737
## 6       Brazil  1999 population  172006362
## 7       Brazil  2000      cases      80488
## 8       Brazil  2000 population  174504898
## 9        China  1999      cases     212258
## 10       China  1999 population 1272915272
## 11       China  2000      cases     213766
## 12       China  2000 population 1280428583
```

```r
#this resembles a melted data set. Each row contains one variable from one observation. The columns include "type" or variable, and "count"", or value.
table3
```

```
## # A tibble: 6 × 3
##       country  year              rate
## *       <chr> <int>             <chr>
## 1 Afghanistan  1999      745/19987071
## 2 Afghanistan  2000     2666/20595360
## 3      Brazil  1999   37737/172006362
## 4      Brazil  2000   80488/174504898
## 5       China  1999 212258/1272915272
## 6       China  2000 213766/1280428583
```

```r
#Each row contains one variable but the variables are combined in one comun such that each cell contains two observed values.
table4a
```

```
## # A tibble: 3 × 3
##       country `1999` `2000`
## *       <chr>  <int>  <int>
## 1 Afghanistan    745   2666
## 2      Brazil  37737  80488
## 3       China 212258 213766
```

```r
table4b
```

```
## # A tibble: 3 × 3
##       country     `1999`     `2000`
## *       <chr>      <int>      <int>
## 1 Afghanistan   19987071   20595360
## 2      Brazil  172006362  174504898
## 3       China 1272915272 1280428583
```

```r
#one variable is represented in two columns of each table. There are two observations per row.
```

**2. Compute the rate for table2, and table4a + table4b. You will need to perform four operations:**  

```r
t2.1 <- table2 %>% 
  filter(type == "cases")
t2.2 <- table2 %>%
  filter(type == "population")
mutate(t2.1, rate = count / t2.2$count * 10000)
```

```
## # A tibble: 6 × 5
##       country  year  type  count     rate
##         <chr> <int> <chr>  <int>    <dbl>
## 1 Afghanistan  1999 cases    745 0.372741
## 2 Afghanistan  2000 cases   2666 1.294466
## 3      Brazil  1999 cases  37737 2.193930
## 4      Brazil  2000 cases  80488 4.612363
## 5       China  1999 cases 212258 1.667495
## 6       China  2000 cases 213766 1.669488
```

```r
#not sure how to approach the table 4 problem.
```

**3. Recreate the plot showing change in cases over time using table2 instead of table1. What do you need to do first?**

```r
table2 %>% 
  filter(type == "cases") %>% 
  ggplot(aes(year, count)) +
  geom_line(aes(group = country), colour = "grey50") + 
  geom_point(aes(colour = country))
```

![](tidy.data1_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

```r
#first you must fliter out the cases variable
```

### 12.3 Spreading and gathering

**1. Why are gather() and spread() not perfectly symmetrical? Carefully consider the following example:**  

```r
stocks <- tibble(
  year   = c(2015, 2015, 2016, 2016),
  half  = c(   1,    2,     1,    2),
  return = c(1.88, 0.59, 0.92, 0.17)
)
stocks %>% 
  spread(year, return) %>% 
  gather("year", "return", `2015`:`2016`)
```

```
## # A tibble: 4 × 3
##    half  year return
##   <dbl> <chr>  <dbl>
## 1     1  2015   1.88
## 2     2  2015   0.59
## 3     1  2016   0.92
## 4     2  2016   0.17
```

Gather will only return column names as characters, even if they are numeric 
passing the argument convert  = TRUE requires spread to "guess" what the appropriate value type is.

**2. Why does this code fail?**

```r
#table4a %>% 
#  gather(1999, 2000, key = "year", value = "cases")
table4a %>% 
  gather(`1999`, `2000`, key = "year", value = "cases")
```

```
## # A tibble: 6 × 3
##       country  year  cases
##         <chr> <chr>  <int>
## 1 Afghanistan  1999    745
## 2      Brazil  1999  37737
## 3       China  1999 212258
## 4 Afghanistan  2000   2666
## 5      Brazil  2000  80488
## 6       China  2000 213766
```
The numeric column names require backticks. Here, the columns selected are the 1999 and 2000 coulumns in the data frame.

**3. Why does spreading this tibble fail? How could you add a new column to fix the problem?**  

```r
people <- tribble(
  ~name,             ~key,    ~value,
  #-----------------|--------|------
  "Phillip Woods",   "age",       45,
  "Phillip Woods",   "height",   186,
  "Phillip Woods",   "age",       50,
  "Jessica Cordero", "age",       37,
  "Jessica Cordero", "height",   156
)

#people %>% 
#  spread(key = key, value = value)
#Error: Duplicate identifiers for rows (1, 3)

#There is a replication of values for one variable in the same observation
people %>% 
  mutate(ID = c(1:5)) %>% 
  spread(key = key, value = value)
```

```
## # A tibble: 5 × 4
##              name    ID   age height
## *           <chr> <int> <dbl>  <dbl>
## 1 Jessica Cordero     4    37     NA
## 2 Jessica Cordero     5    NA    156
## 3   Phillip Woods     1    45     NA
## 4   Phillip Woods     2    NA    186
## 5   Phillip Woods     3    50     NA
```

**Tidy the simple tibble below. Do you need to spread or gather it? What are the variables?**  

```r
preg <- tribble(
  ~pregnant, ~male, ~female,
  "yes",     NA,    10,
  "no",      20,    12
)

preg %>% 
  gather(male, female, key = "gender", value = "count")
```

```
## # A tibble: 4 × 3
##   pregnant gender count
##      <chr>  <chr> <dbl>
## 1      yes   male    NA
## 2       no   male    20
## 3      yes female    10
## 4       no female    12
```

### 12.4 Separating and Uniting

**1. What do the extra and fill arguments do in separate()? Experiment with the various options for the following two toy datasets.**  

```r
tibble(x = c("a,b,c", "d,e,f,g", "h,i,j")) %>% 
  separate(x, c("one", "two", "three"))
```

```
## Warning: Too many values at 1 locations: 2
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2     d     e     f
## 3     h     i     j
```

```r
tibble(x = c("a,b,c", "d,e,f,g", "h,i,j")) %>% 
  separate(x, c("one", "two", "three"), extra = "drop")
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2     d     e     f
## 3     h     i     j
```

```r
tibble(x = c("a,b,c", "d,e", "f,g,i")) %>% 
  separate(x, c("one", "two", "three"))
```

```
## Warning: Too few values at 1 locations: 2
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2     d     e  <NA>
## 3     f     g     i
```

```r
tibble(x = c("a,b,c", "d,e", "f,g,i")) %>% 
  separate(x, c("one", "two", "three"), fill = "left", remove = FALSE)
```

```
## # A tibble: 3 × 4
##       x   one   two three
## * <chr> <chr> <chr> <chr>
## 1 a,b,c     a     b     c
## 2   d,e  <NA>     d     e
## 3 f,g,i     f     g     i
```

**2. Both unite() and separate() have a remove argument. What does it do? Why would you set it to FALSE?**  

```r
tibble(x = c("a,b,c", "d,e", "f,g,i")) %>% 
  separate(x, c("one", "two", "three"), fill = "left", remove = FALSE)
```

```
## # A tibble: 3 × 4
##       x   one   two three
## * <chr> <chr> <chr> <chr>
## 1 a,b,c     a     b     c
## 2   d,e  <NA>     d     e
## 3 f,g,i     f     g     i
```
The remove arguement, when TRUE, eliminates the original separated column. If there is some abiguity between withing that column, such as missing values, it might be useful to keep the original column by setting remove to FALSE.

**3. Compare and contrast separate() and extract(). Why are there three variations of separation (by position, by separator, and with groups), but only one unite?**  
Extract will isolate an entire column while separate removes two sets of values from a column. 




