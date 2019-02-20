---
title: 'Chapter 5: Data Transformation'
author: "Kylie"
date: "2019-02-20"
output:
  html_document:
    keep_md: yes
---



## Summary notes

This chapter introduces the dplyr package (part of the tidyverse). Five key dplyr packages are:

* `filter()`: Pick observations by their values (i.e. select rows containing the require values).  
Example: `new_tibble <- (tibble, variable_1 == "string")`  
Comparison operators that can be used: `>`, `>=`, `<`, `<=`, `!=` (not equal), and `==` (equal).
For numbers it is better to use `near()` than `==`.  
Combine multiple arguments with `&` (and), `|` (or), `!` (not), and %in%. Another useful function is `between(x, left, right)`, which is a shortcut for `x >= left & x <= right`.
Example: `new_tibble <- (tibble, variable_1 %in% c("string1", "string2"))`

`is.na()`

* `arrange()`

* `select()`

* `mutate()`

* `summarise()`


```r
#install.packages("nycflights13")
#install.packages("tidyverse")

library(nycflights13)
library(tidyverse)
```

```
## -- Attaching packages ------------------------------------------------------------------------------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.1.0       v purrr   0.3.0  
## v tibble  2.0.1       v dplyr   0.8.0.1
## v tidyr   0.8.2       v stringr 1.4.0  
## v readr   1.3.1       v forcats 0.4.0
```

```
## -- Conflicts ---------------------------------------------------------------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
nycflights13::flights
```

```
## # A tibble: 336,776 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
##  1  2013     1     1      517            515         2      830
##  2  2013     1     1      533            529         4      850
##  3  2013     1     1      542            540         2      923
##  4  2013     1     1      544            545        -1     1004
##  5  2013     1     1      554            600        -6      812
##  6  2013     1     1      554            558        -4      740
##  7  2013     1     1      555            600        -5      913
##  8  2013     1     1      557            600        -3      709
##  9  2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

## 5.2.4 Exercises

1. Find all flights that
  i) Had an arrival delay of two or more hours  
  
  ```r
  filter(flights, arr_delay >= 2*60)
  ```
  
  ```
  ## # A tibble: 10,200 x 19
  ##     year month   day dep_time sched_dep_time dep_delay arr_time
  ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
  ##  1  2013     1     1      811            630       101     1047
  ##  2  2013     1     1      848           1835       853     1001
  ##  3  2013     1     1      957            733       144     1056
  ##  4  2013     1     1     1114            900       134     1447
  ##  5  2013     1     1     1505           1310       115     1638
  ##  6  2013     1     1     1525           1340       105     1831
  ##  7  2013     1     1     1549           1445        64     1912
  ##  8  2013     1     1     1558           1359       119     1718
  ##  9  2013     1     1     1732           1630        62     2028
  ## 10  2013     1     1     1803           1620       103     2008
  ## # ... with 10,190 more rows, and 12 more variables: sched_arr_time <int>,
  ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
  ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
  ## #   minute <dbl>, time_hour <dttm>
  ```
  
  ii) Flew to Houston (`IAH` or `HOU`)  
  
  ```r
  filter(flights, dest %in% c("IAH", "HOU"))
  ```
  
  ```
  ## # A tibble: 9,313 x 19
  ##     year month   day dep_time sched_dep_time dep_delay arr_time
  ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
  ##  1  2013     1     1      517            515         2      830
  ##  2  2013     1     1      533            529         4      850
  ##  3  2013     1     1      623            627        -4      933
  ##  4  2013     1     1      728            732        -4     1041
  ##  5  2013     1     1      739            739         0     1104
  ##  6  2013     1     1      908            908         0     1228
  ##  7  2013     1     1     1028           1026         2     1350
  ##  8  2013     1     1     1044           1045        -1     1352
  ##  9  2013     1     1     1114            900       134     1447
  ## 10  2013     1     1     1205           1200         5     1503
  ## # ... with 9,303 more rows, and 12 more variables: sched_arr_time <int>,
  ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
  ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
  ## #   minute <dbl>, time_hour <dttm>
  ```
  
  iii) Were operated by United, American, or Delta  
  
  ```r
  filter(flights, carrier %in% c("UA", "AA", "DL"))
  ```
  
  ```
  ## # A tibble: 139,504 x 19
  ##     year month   day dep_time sched_dep_time dep_delay arr_time
  ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
  ##  1  2013     1     1      517            515         2      830
  ##  2  2013     1     1      533            529         4      850
  ##  3  2013     1     1      542            540         2      923
  ##  4  2013     1     1      554            600        -6      812
  ##  5  2013     1     1      554            558        -4      740
  ##  6  2013     1     1      558            600        -2      753
  ##  7  2013     1     1      558            600        -2      924
  ##  8  2013     1     1      558            600        -2      923
  ##  9  2013     1     1      559            600        -1      941
  ## 10  2013     1     1      559            600        -1      854
  ## # ... with 139,494 more rows, and 12 more variables: sched_arr_time <int>,
  ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
  ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
  ## #   minute <dbl>, time_hour <dttm>
  ```
  
  iv) Departed in summer (July, August, and September)  
  
  ```r
  filter(flights, month %in% c(7,8,9))
  ```
  
  ```
  ## # A tibble: 86,326 x 19
  ##     year month   day dep_time sched_dep_time dep_delay arr_time
  ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
  ##  1  2013     7     1        1           2029       212      236
  ##  2  2013     7     1        2           2359         3      344
  ##  3  2013     7     1       29           2245       104      151
  ##  4  2013     7     1       43           2130       193      322
  ##  5  2013     7     1       44           2150       174      300
  ##  6  2013     7     1       46           2051       235      304
  ##  7  2013     7     1       48           2001       287      308
  ##  8  2013     7     1       58           2155       183      335
  ##  9  2013     7     1      100           2146       194      327
  ## 10  2013     7     1      100           2245       135      337
  ## # ... with 86,316 more rows, and 12 more variables: sched_arr_time <int>,
  ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
  ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
  ## #   minute <dbl>, time_hour <dttm>
  ```
  
  v) Arrived more than two hours late, but didn’t leave late  
  
  ```r
  filter(flights, arr_delay > 2*60 & dep_delay <= 0)
  ```
  
  ```
  ## # A tibble: 29 x 19
  ##     year month   day dep_time sched_dep_time dep_delay arr_time
  ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
  ##  1  2013     1    27     1419           1420        -1     1754
  ##  2  2013    10     7     1350           1350         0     1736
  ##  3  2013    10     7     1357           1359        -2     1858
  ##  4  2013    10    16      657            700        -3     1258
  ##  5  2013    11     1      658            700        -2     1329
  ##  6  2013     3    18     1844           1847        -3       39
  ##  7  2013     4    17     1635           1640        -5     2049
  ##  8  2013     4    18      558            600        -2     1149
  ##  9  2013     4    18      655            700        -5     1213
  ## 10  2013     5    22     1827           1830        -3     2217
  ## # ... with 19 more rows, and 12 more variables: sched_arr_time <int>,
  ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
  ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
  ## #   minute <dbl>, time_hour <dttm>
  ```
  
  vi) Were delayed by at least an hour, but made up over 30 minutes in flight  
  
  ```r
  filter(flights, dep_delay >= 60 & arr_delay < 30)
  ```
  
  ```
  ## # A tibble: 206 x 19
  ##     year month   day dep_time sched_dep_time dep_delay arr_time
  ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
  ##  1  2013     1     3     1850           1745        65     2148
  ##  2  2013     1     3     1950           1845        65     2228
  ##  3  2013     1     3     2015           1915        60     2135
  ##  4  2013     1     6     1019            900        79     1558
  ##  5  2013     1     7     1543           1430        73     1758
  ##  6  2013     1    11     1020            920        60     1311
  ##  7  2013     1    12     1706           1600        66     1949
  ##  8  2013     1    12     1953           1845        68     2154
  ##  9  2013     1    19     1456           1355        61     1636
  ## 10  2013     1    21     1531           1430        61     1843
  ## # ... with 196 more rows, and 12 more variables: sched_arr_time <int>,
  ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
  ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
  ## #   minute <dbl>, time_hour <dttm>
  ```
  
  vii) Departed between midnight and 6am (inclusive)  
  
  ```r
  filter(flights, dep_time >= 0 & dep_time <= 6)
  ```
  
  ```
  ## # A tibble: 155 x 19
  ##     year month   day dep_time sched_dep_time dep_delay arr_time
  ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
  ##  1  2013     1     9        2           2359         3      432
  ##  2  2013     1    10        3           2359         4      426
  ##  3  2013     1    13        1           2249        72      108
  ##  4  2013     1    13        2           2359         3      502
  ##  5  2013     1    13        3           2030       213      340
  ##  6  2013     1    16        2           2125       157      119
  ##  7  2013     1    16        3           1946       257      212
  ##  8  2013     1    22        5           2359         6      452
  ##  9  2013     1    30        3           2159       124      100
  ## 10  2013     1    31        1           2100       181      124
  ## # ... with 145 more rows, and 12 more variables: sched_arr_time <int>,
  ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
  ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
  ## #   minute <dbl>, time_hour <dttm>
  ```
  or
  
  ```r
  filter(flights, between(dep_time, 0, 6))
  ```
  
  ```
  ## # A tibble: 155 x 19
  ##     year month   day dep_time sched_dep_time dep_delay arr_time
  ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
  ##  1  2013     1     9        2           2359         3      432
  ##  2  2013     1    10        3           2359         4      426
  ##  3  2013     1    13        1           2249        72      108
  ##  4  2013     1    13        2           2359         3      502
  ##  5  2013     1    13        3           2030       213      340
  ##  6  2013     1    16        2           2125       157      119
  ##  7  2013     1    16        3           1946       257      212
  ##  8  2013     1    22        5           2359         6      452
  ##  9  2013     1    30        3           2159       124      100
  ## 10  2013     1    31        1           2100       181      124
  ## # ... with 145 more rows, and 12 more variables: sched_arr_time <int>,
  ## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
  ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
  ## #   minute <dbl>, time_hour <dttm>
  ```
