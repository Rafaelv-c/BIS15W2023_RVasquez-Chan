---
title: "Lab 6 Homework"
author: "Rafael Vasquez-Chan"
date: "2023-01-31"
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
library(skimr)
```

For this assignment we are going to work with a large data set from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. These data are pretty wild, so we need to do some cleaning. First, load the data.  

Load the data `FAO_1950to2012_111914.csv` as a new object titled `fisheries`.

```r
fisheries <- readr::read_csv(file = "data/FAO_1950to2012_111914.csv")
```

```
## Rows: 17692 Columns: 71
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (69): Country, Common name, ISSCAAP taxonomic group, ASFIS species#, ASF...
## dbl  (2): ISSCAAP group#, FAO major fishing area
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?  

```r
str(fisheries)
```

```
## spc_tbl_ [17,692 × 71] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ Country                : chr [1:17692] "Albania" "Albania" "Albania" "Albania" ...
##  $ Common name            : chr [1:17692] "Angelsharks, sand devils nei" "Atlantic bonito" "Barracudas nei" "Blue and red shrimp" ...
##  $ ISSCAAP group#         : num [1:17692] 38 36 37 45 32 37 33 45 38 57 ...
##  $ ISSCAAP taxonomic group: chr [1:17692] "Sharks, rays, chimaeras" "Tunas, bonitos, billfishes" "Miscellaneous pelagic fishes" "Shrimps, prawns" ...
##  $ ASFIS species#         : chr [1:17692] "10903XXXXX" "1750100101" "17710001XX" "2280203101" ...
##  $ ASFIS species name     : chr [1:17692] "Squatinidae" "Sarda sarda" "Sphyraena spp" "Aristeus antennatus" ...
##  $ FAO major fishing area : num [1:17692] 37 37 37 37 37 37 37 37 37 37 ...
##  $ Measure                : chr [1:17692] "Quantity (tonnes)" "Quantity (tonnes)" "Quantity (tonnes)" "Quantity (tonnes)" ...
##  $ 1950                   : chr [1:17692] NA NA NA NA ...
##  $ 1951                   : chr [1:17692] NA NA NA NA ...
##  $ 1952                   : chr [1:17692] NA NA NA NA ...
##  $ 1953                   : chr [1:17692] NA NA NA NA ...
##  $ 1954                   : chr [1:17692] NA NA NA NA ...
##  $ 1955                   : chr [1:17692] NA NA NA NA ...
##  $ 1956                   : chr [1:17692] NA NA NA NA ...
##  $ 1957                   : chr [1:17692] NA NA NA NA ...
##  $ 1958                   : chr [1:17692] NA NA NA NA ...
##  $ 1959                   : chr [1:17692] NA NA NA NA ...
##  $ 1960                   : chr [1:17692] NA NA NA NA ...
##  $ 1961                   : chr [1:17692] NA NA NA NA ...
##  $ 1962                   : chr [1:17692] NA NA NA NA ...
##  $ 1963                   : chr [1:17692] NA NA NA NA ...
##  $ 1964                   : chr [1:17692] NA NA NA NA ...
##  $ 1965                   : chr [1:17692] NA NA NA NA ...
##  $ 1966                   : chr [1:17692] NA NA NA NA ...
##  $ 1967                   : chr [1:17692] NA NA NA NA ...
##  $ 1968                   : chr [1:17692] NA NA NA NA ...
##  $ 1969                   : chr [1:17692] NA NA NA NA ...
##  $ 1970                   : chr [1:17692] NA NA NA NA ...
##  $ 1971                   : chr [1:17692] NA NA NA NA ...
##  $ 1972                   : chr [1:17692] NA NA NA NA ...
##  $ 1973                   : chr [1:17692] NA NA NA NA ...
##  $ 1974                   : chr [1:17692] NA NA NA NA ...
##  $ 1975                   : chr [1:17692] NA NA NA NA ...
##  $ 1976                   : chr [1:17692] NA NA NA NA ...
##  $ 1977                   : chr [1:17692] NA NA NA NA ...
##  $ 1978                   : chr [1:17692] NA NA NA NA ...
##  $ 1979                   : chr [1:17692] NA NA NA NA ...
##  $ 1980                   : chr [1:17692] NA NA NA NA ...
##  $ 1981                   : chr [1:17692] NA NA NA NA ...
##  $ 1982                   : chr [1:17692] NA NA NA NA ...
##  $ 1983                   : chr [1:17692] NA NA NA NA ...
##  $ 1984                   : chr [1:17692] NA NA NA NA ...
##  $ 1985                   : chr [1:17692] NA NA NA NA ...
##  $ 1986                   : chr [1:17692] NA NA NA NA ...
##  $ 1987                   : chr [1:17692] NA NA NA NA ...
##  $ 1988                   : chr [1:17692] NA NA NA NA ...
##  $ 1989                   : chr [1:17692] NA NA NA NA ...
##  $ 1990                   : chr [1:17692] NA NA NA NA ...
##  $ 1991                   : chr [1:17692] NA NA NA NA ...
##  $ 1992                   : chr [1:17692] NA NA NA NA ...
##  $ 1993                   : chr [1:17692] NA NA NA NA ...
##  $ 1994                   : chr [1:17692] NA NA NA NA ...
##  $ 1995                   : chr [1:17692] "0 0" "1" NA "0 0" ...
##  $ 1996                   : chr [1:17692] "53" "2" NA "3" ...
##  $ 1997                   : chr [1:17692] "20" "0 0" NA "0 0" ...
##  $ 1998                   : chr [1:17692] "31" "12" NA NA ...
##  $ 1999                   : chr [1:17692] "30" "30" NA NA ...
##  $ 2000                   : chr [1:17692] "30" "25" "2" NA ...
##  $ 2001                   : chr [1:17692] "16" "30" NA NA ...
##  $ 2002                   : chr [1:17692] "79" "24" NA "34" ...
##  $ 2003                   : chr [1:17692] "1" "4" NA "22" ...
##  $ 2004                   : chr [1:17692] "4" "2" "2" "15" ...
##  $ 2005                   : chr [1:17692] "68" "23" "4" "12" ...
##  $ 2006                   : chr [1:17692] "55" "30" "7" "18" ...
##  $ 2007                   : chr [1:17692] "12" "19" NA NA ...
##  $ 2008                   : chr [1:17692] "23" "27" NA NA ...
##  $ 2009                   : chr [1:17692] "14" "21" NA NA ...
##  $ 2010                   : chr [1:17692] "78" "23" "7" NA ...
##  $ 2011                   : chr [1:17692] "12" "12" NA NA ...
##  $ 2012                   : chr [1:17692] "5" "5" NA NA ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   Country = col_character(),
##   ..   `Common name` = col_character(),
##   ..   `ISSCAAP group#` = col_double(),
##   ..   `ISSCAAP taxonomic group` = col_character(),
##   ..   `ASFIS species#` = col_character(),
##   ..   `ASFIS species name` = col_character(),
##   ..   `FAO major fishing area` = col_double(),
##   ..   Measure = col_character(),
##   ..   `1950` = col_character(),
##   ..   `1951` = col_character(),
##   ..   `1952` = col_character(),
##   ..   `1953` = col_character(),
##   ..   `1954` = col_character(),
##   ..   `1955` = col_character(),
##   ..   `1956` = col_character(),
##   ..   `1957` = col_character(),
##   ..   `1958` = col_character(),
##   ..   `1959` = col_character(),
##   ..   `1960` = col_character(),
##   ..   `1961` = col_character(),
##   ..   `1962` = col_character(),
##   ..   `1963` = col_character(),
##   ..   `1964` = col_character(),
##   ..   `1965` = col_character(),
##   ..   `1966` = col_character(),
##   ..   `1967` = col_character(),
##   ..   `1968` = col_character(),
##   ..   `1969` = col_character(),
##   ..   `1970` = col_character(),
##   ..   `1971` = col_character(),
##   ..   `1972` = col_character(),
##   ..   `1973` = col_character(),
##   ..   `1974` = col_character(),
##   ..   `1975` = col_character(),
##   ..   `1976` = col_character(),
##   ..   `1977` = col_character(),
##   ..   `1978` = col_character(),
##   ..   `1979` = col_character(),
##   ..   `1980` = col_character(),
##   ..   `1981` = col_character(),
##   ..   `1982` = col_character(),
##   ..   `1983` = col_character(),
##   ..   `1984` = col_character(),
##   ..   `1985` = col_character(),
##   ..   `1986` = col_character(),
##   ..   `1987` = col_character(),
##   ..   `1988` = col_character(),
##   ..   `1989` = col_character(),
##   ..   `1990` = col_character(),
##   ..   `1991` = col_character(),
##   ..   `1992` = col_character(),
##   ..   `1993` = col_character(),
##   ..   `1994` = col_character(),
##   ..   `1995` = col_character(),
##   ..   `1996` = col_character(),
##   ..   `1997` = col_character(),
##   ..   `1998` = col_character(),
##   ..   `1999` = col_character(),
##   ..   `2000` = col_character(),
##   ..   `2001` = col_character(),
##   ..   `2002` = col_character(),
##   ..   `2003` = col_character(),
##   ..   `2004` = col_character(),
##   ..   `2005` = col_character(),
##   ..   `2006` = col_character(),
##   ..   `2007` = col_character(),
##   ..   `2008` = col_character(),
##   ..   `2009` = col_character(),
##   ..   `2010` = col_character(),
##   ..   `2011` = col_character(),
##   ..   `2012` = col_character()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

```r
anyNA(fisheries)
```

```
## [1] TRUE
```

```r
names(fisheries)
```

```
##  [1] "Country"                 "Common name"            
##  [3] "ISSCAAP group#"          "ISSCAAP taxonomic group"
##  [5] "ASFIS species#"          "ASFIS species name"     
##  [7] "FAO major fishing area"  "Measure"                
##  [9] "1950"                    "1951"                   
## [11] "1952"                    "1953"                   
## [13] "1954"                    "1955"                   
## [15] "1956"                    "1957"                   
## [17] "1958"                    "1959"                   
## [19] "1960"                    "1961"                   
## [21] "1962"                    "1963"                   
## [23] "1964"                    "1965"                   
## [25] "1966"                    "1967"                   
## [27] "1968"                    "1969"                   
## [29] "1970"                    "1971"                   
## [31] "1972"                    "1973"                   
## [33] "1974"                    "1975"                   
## [35] "1976"                    "1977"                   
## [37] "1978"                    "1979"                   
## [39] "1980"                    "1981"                   
## [41] "1982"                    "1983"                   
## [43] "1984"                    "1985"                   
## [45] "1986"                    "1987"                   
## [47] "1988"                    "1989"                   
## [49] "1990"                    "1991"                   
## [51] "1992"                    "1993"                   
## [53] "1994"                    "1995"                   
## [55] "1996"                    "1997"                   
## [57] "1998"                    "1999"                   
## [59] "2000"                    "2001"                   
## [61] "2002"                    "2003"                   
## [63] "2004"                    "2005"                   
## [65] "2006"                    "2007"                   
## [67] "2008"                    "2009"                   
## [69] "2010"                    "2011"                   
## [71] "2012"
```

2. Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor. 

```r
fisheries <- janitor::clean_names(fisheries)
names(fisheries)
```

```
##  [1] "country"                 "common_name"            
##  [3] "isscaap_group_number"    "isscaap_taxonomic_group"
##  [5] "asfis_species_number"    "asfis_species_name"     
##  [7] "fao_major_fishing_area"  "measure"                
##  [9] "x1950"                   "x1951"                  
## [11] "x1952"                   "x1953"                  
## [13] "x1954"                   "x1955"                  
## [15] "x1956"                   "x1957"                  
## [17] "x1958"                   "x1959"                  
## [19] "x1960"                   "x1961"                  
## [21] "x1962"                   "x1963"                  
## [23] "x1964"                   "x1965"                  
## [25] "x1966"                   "x1967"                  
## [27] "x1968"                   "x1969"                  
## [29] "x1970"                   "x1971"                  
## [31] "x1972"                   "x1973"                  
## [33] "x1974"                   "x1975"                  
## [35] "x1976"                   "x1977"                  
## [37] "x1978"                   "x1979"                  
## [39] "x1980"                   "x1981"                  
## [41] "x1982"                   "x1983"                  
## [43] "x1984"                   "x1985"                  
## [45] "x1986"                   "x1987"                  
## [47] "x1988"                   "x1989"                  
## [49] "x1990"                   "x1991"                  
## [51] "x1992"                   "x1993"                  
## [53] "x1994"                   "x1995"                  
## [55] "x1996"                   "x1997"                  
## [57] "x1998"                   "x1999"                  
## [59] "x2000"                   "x2001"                  
## [61] "x2002"                   "x2003"                  
## [63] "x2004"                   "x2005"                  
## [65] "x2006"                   "x2007"                  
## [67] "x2008"                   "x2009"                  
## [69] "x2010"                   "x2011"                  
## [71] "x2012"
```


```r
fisheries$country <- as.factor(fisheries$country)
fisheries$isscaap_group_number <- as.factor(fisheries$isscaap_group_number)
fisheries$asfis_species_number <- as.factor(fisheries$asfis_species_number)
fisheries$fao_major_fishing_area <- as.factor(fisheries$fao_major_fishing_area)
```

We need to deal with the years because they are being treated as characters and start with an X. We also have the problem that the column names that are years actually represent data. We haven't discussed tidy data yet, so here is some help. You should run this ugly chunk to transform the data for the rest of the homework. It will only work if you have used janitor to rename the variables in question 2!

```r
fisheries_tidy <- fisheries %>% 
  pivot_longer(-c(country,common_name,isscaap_group_number,isscaap_taxonomic_group,asfis_species_number,asfis_species_name,fao_major_fishing_area,measure),
               names_to = "year",
               values_to = "catch",
               values_drop_na = TRUE) %>% 
  mutate(year= as.numeric(str_replace(year, 'x', ''))) %>% 
  mutate(catch= str_replace(catch, c(' F'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('...'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('-'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('0 0'), replacement = ''))
fisheries_tidy$catch <- as.numeric(fisheries_tidy$catch)
```

3. How many countries are represented in the data? Provide a count and list their names.

```r
fisheries_tidy %>% 
    count(country, sort=T)
```

```
## # A tibble: 203 × 2
##    country                      n
##    <fct>                    <int>
##  1 United States of America 18080
##  2 Spain                    17482
##  3 Japan                    15429
##  4 Portugal                 11570
##  5 Korea, Republic of       10824
##  6 France                   10639
##  7 Taiwan Province of China  9927
##  8 Indonesia                 9274
##  9 Australia                 8183
## 10 Un. Sov. Soc. Rep.        7084
## # … with 193 more rows
```

4. Refocus the data only to include country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.

```r
select(fisheries_tidy, country, isscaap_group_number, asfis_species_number, asfis_species_name, year, catch)
```

```
## # A tibble: 376,771 × 6
##    country isscaap_group_number asfis_species_number asfis_species…¹  year catch
##    <fct>   <fct>                <fct>                <chr>           <dbl> <dbl>
##  1 Albania 38                   10903XXXXX           Squatinidae      1995    NA
##  2 Albania 38                   10903XXXXX           Squatinidae      1996    53
##  3 Albania 38                   10903XXXXX           Squatinidae      1997    20
##  4 Albania 38                   10903XXXXX           Squatinidae      1998    31
##  5 Albania 38                   10903XXXXX           Squatinidae      1999    30
##  6 Albania 38                   10903XXXXX           Squatinidae      2000    30
##  7 Albania 38                   10903XXXXX           Squatinidae      2001    16
##  8 Albania 38                   10903XXXXX           Squatinidae      2002    79
##  9 Albania 38                   10903XXXXX           Squatinidae      2003     1
## 10 Albania 38                   10903XXXXX           Squatinidae      2004     4
## # … with 376,761 more rows, and abbreviated variable name ¹​asfis_species_name
```

5. Based on the asfis_species_number, how many distinct fish species were caught as part of these data?

```r
fisheries_tidy %>% 
  filter(!is.na(catch)) %>% 
  summarise(asfis_species_number_distinct=n_distinct(asfis_species_number))
```

```
## # A tibble: 1 × 1
##   asfis_species_number_distinct
##                           <int>
## 1                          1491
```

6. Which country had the largest overall catch in the year 2000?

```r
fisheries_tidy %>% 
  filter(year == "2000") %>%
  group_by(country) %>% 
   summarize(total_catch = sum(catch, na.rm = T)) %>% 
  arrange(desc(total_catch))
```

```
## # A tibble: 193 × 2
##    country                  total_catch
##    <fct>                          <dbl>
##  1 China                          25899
##  2 Russian Federation             12181
##  3 United States of America       11762
##  4 Japan                           8510
##  5 Indonesia                       8341
##  6 Peru                            7443
##  7 Chile                           6906
##  8 India                           6351
##  9 Thailand                        6243
## 10 Korea, Republic of              6124
## # … with 183 more rows
```

7. Which country caught the most sardines (_Sardina pilchardus_) between the years 1990-2000?

```r
fisheries_tidy %>% 
  filter(asfis_species_name == "Sardina pilchardus") %>% 
  filter(between(year, "1990", "2000")) %>% 
  group_by(country) %>% 
  summarise(total_sardines = sum(catch, rm.na = T)) %>% 
  arrange(desc(total_sardines))
```

```
## # A tibble: 37 × 2
##    country               total_sardines
##    <fct>                          <dbl>
##  1 Morocco                         7471
##  2 Spain                           3508
##  3 Russian Federation              1640
##  4 Ukraine                         1031
##  5 Portugal                         819
##  6 Greece                           529
##  7 Italy                            508
##  8 Serbia and Montenegro            479
##  9 Denmark                          478
## 10 Tunisia                          428
## # … with 27 more rows
```

8. Which five countries caught the most cephalopods between 2008-2012?

```r
fisheries_tidy %>% 
  filter(between(year, "2008", "2012")) %>% 
  filter(isscaap_taxonomic_group == "Squids, cuttlefishes, octopuses") %>% 
  group_by(country) %>% 
  summarize(total_cephalopods = sum(catch, na.rm = T)) %>% 
  arrange(desc(total_cephalopods))
```

```
## # A tibble: 122 × 2
##    country                  total_cephalopods
##    <fct>                                <dbl>
##  1 China                                 8349
##  2 Korea, Republic of                    3480
##  3 Peru                                  3422
##  4 Japan                                 3248
##  5 Chile                                 2775
##  6 United States of America              2417
##  7 Indonesia                             1622
##  8 Taiwan Province of China              1394
##  9 Spain                                 1147
## 10 France                                1138
## # … with 112 more rows
```



9. Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)

```r
fisheries_tidy %>% 
  filter(between(year, 2008, 2012)) %>% 
  group_by(asfis_species_name) %>% 
  summarize(total_catch = sum(catch, na.rm = T)) %>% 
  arrange(desc(total_catch))
```

```
## # A tibble: 1,472 × 2
##    asfis_species_name    total_catch
##    <chr>                       <dbl>
##  1 Osteichthyes               107808
##  2 Theragra chalcogramma       41075
##  3 Engraulis ringens           35523
##  4 Katsuwonus pelamis          32153
##  5 Trichiurus lepturus         30400
##  6 Clupea harengus             28527
##  7 Thunnus albacares           20119
##  8 Scomber japonicus           14723
##  9 Gadus morhua                13253
## 10 Thunnus alalunga            12019
## # … with 1,462 more rows
```

10. Use the data to do at least one analysis of your choice.

```r
#What year were the most tunas, bonitos, and billfishes caught
fisheries_tidy %>% 
  filter(isscaap_taxonomic_group == "Tunas, bonitos, billfishes") %>% 
  group_by(year) %>% 
  summarize(total_fish = sum(catch, na.rm = T)) %>% 
  arrange(desc(total_fish))
```

```
## # A tibble: 63 × 2
##     year total_fish
##    <dbl>      <dbl>
##  1  2007      34186
##  2  2006      33880
##  3  2012      33554
##  4  2011      33487
##  5  2005      31917
##  6  2010      31252
##  7  2008      30919
##  8  2009      30332
##  9  2004      29842
## 10  2002      28659
## # … with 53 more rows
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
