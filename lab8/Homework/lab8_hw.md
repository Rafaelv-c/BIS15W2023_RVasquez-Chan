---
title: "Lab 8 Homework"
author: "Rafael Vasquez-Chan"
date: "2023-02-08"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

```r
#install.packages("here")
```

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
sydneybeaches <-read_csv(here("lab8", "data", "sydneybeaches.csv")) %>% janitor::clean_names()
```

```
## Rows: 3690 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
glimpse(sydneybeaches)
```

```
## Rows: 3,690
## Columns: 8
## $ beach_id              <dbl> 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, …
## $ region                <chr> "Sydney City Ocean Beaches", "Sydney City Ocean …
## $ council               <chr> "Randwick Council", "Randwick Council", "Randwic…
## $ site                  <chr> "Clovelly Beach", "Clovelly Beach", "Clovelly Be…
## $ longitude             <dbl> 151.2675, 151.2675, 151.2675, 151.2675, 151.2675…
## $ latitude              <dbl> -33.91449, -33.91449, -33.91449, -33.91449, -33.…
## $ date                  <chr> "02/01/2013", "06/01/2013", "12/01/2013", "18/01…
## $ enterococci_cfu_100ml <dbl> 19, 3, 2, 13, 8, 7, 11, 97, 3, 0, 6, 0, 1, 8, 3,…
```

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library(here)
```

The quotes show the folder structure from the root directory.

```r
sydneybeaches <-read_csv(here("lab8", "data", "sydneybeaches.csv")) %>% janitor::clean_names()
```

```
## Rows: 3690 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?

No they are not, sites and dates can be separated into their own variables.

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`

```r
sydneybeaches_long <- sydneybeaches %>% 
  select(site, date,enterococci_cfu_100ml)
sydneybeaches_long
```

```
## # A tibble: 3,690 × 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 02/01/2013                    19
##  2 Clovelly Beach 06/01/2013                     3
##  3 Clovelly Beach 12/01/2013                     2
##  4 Clovelly Beach 18/01/2013                    13
##  5 Clovelly Beach 30/01/2013                     8
##  6 Clovelly Beach 05/02/2013                     7
##  7 Clovelly Beach 11/02/2013                    11
##  8 Clovelly Beach 23/02/2013                    97
##  9 Clovelly Beach 07/03/2013                     3
## 10 Clovelly Beach 25/03/2013                     0
## # … with 3,680 more rows
```


4. Pivot the data such that the dates are column names and each beach only appears once. Name the object `sydneybeaches_wide`

```r
sydneybeaches_wide <- sydneybeaches %>% 
  pivot_wider(names_from = date,
              values_from = enterococci_cfu_100ml)
sydneybeaches_wide
```

```
## # A tibble: 11 × 350
##    beach_id region council site  longi…¹ latit…² 02/01…³ 06/01…⁴ 12/01…⁵ 18/01…⁶
##       <dbl> <chr>  <chr>   <chr>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
##  1     25   Sydne… Randwi… Clov…    151.   -33.9      19       3       2      13
##  2     26   Sydne… Randwi… Coog…    151.   -33.9      15       4      17      18
##  3     25.9 Sydne… Randwi… Gord…    151.   -33.9      NA      NA      NA      NA
##  4     29   Sydne… Randwi… Litt…    151.   -34.0       9       3      72       1
##  5     28   Sydne… Randwi… Mala…    151.   -34.0       2       4     390      15
##  6     27   Sydne… Randwi… Maro…    151.   -33.9       1       1      20       2
##  7     27.4 Sydne… Randwi… Sout…    151.   -34.0       1       0      33       2
##  8     27.3 Sydne… Randwi… Sout…    151.   -34.0      12       2     110      13
##  9     22   Sydne… Waverl… Bond…    151.   -33.9       3       1       2       1
## 10     24   Sydne… Waverl… Bron…    151.   -33.9       4       2      38       3
## 11     23   Sydne… Waverl… Tama…    151.   -33.9       1       0       7      22
## # … with 340 more variables: `30/01/2013` <dbl>, `05/02/2013` <dbl>,
## #   `11/02/2013` <dbl>, `23/02/2013` <dbl>, `07/03/2013` <dbl>,
## #   `25/03/2013` <dbl>, `02/04/2013` <dbl>, `12/04/2013` <dbl>,
## #   `18/04/2013` <dbl>, `24/04/2013` <dbl>, `01/05/2013` <dbl>,
## #   `20/05/2013` <dbl>, `31/05/2013` <dbl>, `06/06/2013` <dbl>,
## #   `12/06/2013` <dbl>, `24/06/2013` <dbl>, `06/07/2013` <dbl>,
## #   `18/07/2013` <dbl>, `24/07/2013` <dbl>, `08/08/2013` <dbl>, …
```


5. Pivot the data back so that the dates are data and not column names.

```r
sydneybeaches_wide %>% 
  pivot_longer(-c(beach_id, region, council, site, longitude, latitude),
               names_to = "date",
               values_to = "enterococci_cfu_100ml")
```

```
## # A tibble: 3,784 × 8
##    beach_id region                   council site  longi…¹ latit…² date  enter…³
##       <dbl> <chr>                    <chr>   <chr>   <dbl>   <dbl> <chr>   <dbl>
##  1       25 Sydney City Ocean Beach… Randwi… Clov…    151.   -33.9 02/0…      19
##  2       25 Sydney City Ocean Beach… Randwi… Clov…    151.   -33.9 06/0…       3
##  3       25 Sydney City Ocean Beach… Randwi… Clov…    151.   -33.9 12/0…       2
##  4       25 Sydney City Ocean Beach… Randwi… Clov…    151.   -33.9 18/0…      13
##  5       25 Sydney City Ocean Beach… Randwi… Clov…    151.   -33.9 30/0…       8
##  6       25 Sydney City Ocean Beach… Randwi… Clov…    151.   -33.9 05/0…       7
##  7       25 Sydney City Ocean Beach… Randwi… Clov…    151.   -33.9 11/0…      11
##  8       25 Sydney City Ocean Beach… Randwi… Clov…    151.   -33.9 23/0…      97
##  9       25 Sydney City Ocean Beach… Randwi… Clov…    151.   -33.9 07/0…       3
## 10       25 Sydney City Ocean Beach… Randwi… Clov…    151.   -33.9 25/0…       0
## # … with 3,774 more rows, and abbreviated variable names ¹​longitude, ²​latitude,
## #   ³​enterococci_cfu_100ml
```


6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.

```r
sydneybeaches_long_separated <- sydneybeaches_long %>% 
  separate(date, into = c("day", "month", "year"), sep = "/")
sydneybeaches_long_separated
```

```
## # A tibble: 3,690 × 5
##    site           day   month year  enterococci_cfu_100ml
##    <chr>          <chr> <chr> <chr>                 <dbl>
##  1 Clovelly Beach 02    01    2013                     19
##  2 Clovelly Beach 06    01    2013                      3
##  3 Clovelly Beach 12    01    2013                      2
##  4 Clovelly Beach 18    01    2013                     13
##  5 Clovelly Beach 30    01    2013                      8
##  6 Clovelly Beach 05    02    2013                      7
##  7 Clovelly Beach 11    02    2013                     11
##  8 Clovelly Beach 23    02    2013                     97
##  9 Clovelly Beach 07    03    2013                      3
## 10 Clovelly Beach 25    03    2013                      0
## # … with 3,680 more rows
```


7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.

```r
avg_enterococci_cfu_100ml_per_beach_per_year <- sydneybeaches_long_separated %>% 
  group_by(year, site) %>% 
  summarise(avg_enterococci_cfu_100ml = mean(enterococci_cfu_100ml, na.rm = T))
```

```
## `summarise()` has grouped output by 'year'. You can override using the
## `.groups` argument.
```

```r
avg_enterococci_cfu_100ml_per_beach_per_year
```

```
## # A tibble: 66 × 3
## # Groups:   year [6]
##    year  site                    avg_enterococci_cfu_100ml
##    <chr> <chr>                                       <dbl>
##  1 2013  Bondi Beach                                 32.2 
##  2 2013  Bronte Beach                                26.8 
##  3 2013  Clovelly Beach                               9.28
##  4 2013  Coogee Beach                                39.7 
##  5 2013  Gordons Bay (East)                          24.8 
##  6 2013  Little Bay Beach                           122.  
##  7 2013  Malabar Beach                              101.  
##  8 2013  Maroubra Beach                              47.1 
##  9 2013  South Maroubra Beach                        39.3 
## 10 2013  South Maroubra Rockpool                     96.4 
## # … with 56 more rows
```


8. Make the output from question 7 easier to read by pivoting it to wide format.

```r
avg_enterococci_cfu_100ml_per_beach_per_year %>% 
  pivot_wider(names_from = year,
              values_from = avg_enterococci_cfu_100ml)
```

```
## # A tibble: 11 × 7
##    site                    `2013` `2014` `2015` `2016` `2017` `2018`
##    <chr>                    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 Bondi Beach              32.2   11.1   14.3    19.4  13.2   22.9 
##  2 Bronte Beach             26.8   17.5   23.6    61.3  16.8   43.4 
##  3 Clovelly Beach            9.28  13.8    8.82   11.3   7.93  10.6 
##  4 Coogee Beach             39.7   52.6   40.3    59.5  20.7   21.6 
##  5 Gordons Bay (East)       24.8   16.7   36.2    39.0  13.7   17.6 
##  6 Little Bay Beach        122.    19.5   25.5    31.2  18.2   59.1 
##  7 Malabar Beach           101.    54.5   66.9    91.0  49.8   38.0 
##  8 Maroubra Beach           47.1    9.23  14.5    26.6  11.6    9.21
##  9 South Maroubra Beach     39.3   14.9    8.25   10.7   8.26  12.5 
## 10 South Maroubra Rockpool  96.4   40.6   47.3    59.3  46.9  112.  
## 11 Tamarama Beach           29.7   39.6   57.0    50.3  20.4   15.5
```


9. What was the most polluted beach in 2018?

```r
avg_enterococci_cfu_100ml_per_beach_per_year %>% 
  filter(year == "2018") %>% 
  arrange(desc(avg_enterococci_cfu_100ml))
```

```
## # A tibble: 11 × 3
## # Groups:   year [1]
##    year  site                    avg_enterococci_cfu_100ml
##    <chr> <chr>                                       <dbl>
##  1 2018  South Maroubra Rockpool                    112.  
##  2 2018  Little Bay Beach                            59.1 
##  3 2018  Bronte Beach                                43.4 
##  4 2018  Malabar Beach                               38.0 
##  5 2018  Bondi Beach                                 22.9 
##  6 2018  Coogee Beach                                21.6 
##  7 2018  Gordons Bay (East)                          17.6 
##  8 2018  Tamarama Beach                              15.5 
##  9 2018  South Maroubra Beach                        12.5 
## 10 2018  Clovelly Beach                              10.6 
## 11 2018  Maroubra Beach                               9.21
```

10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
