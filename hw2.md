p8105 hw2
================
Yue Chen
9/24/2020

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.2.1     ✓ purrr   0.3.4
    ## ✓ tibble  3.0.3     ✓ dplyr   0.8.3
    ## ✓ tidyr   1.0.0     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.4.0

    ## Warning: package 'tibble' was built under R version 3.6.2

    ## Warning: package 'purrr' was built under R version 3.6.2

    ## ── Conflicts ────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(readxl)
```

Problem 1

Read the Mr. Trashwheel
dataset.

``` r
trash_wheel = read_excel("./data/Trash-Wheel-Collection-Totals-8-6-19.xlsx") %>%
   janitor::clean_names() %>%
   select(-x15, -x16, -x17) %>%
   drop_na(dumpster) %>%
  mutate(
    sports_balls = round(sports_balls),
    sports_balls = as.integer(sports_balls)
  )
```

    ## New names:
    ## * `` -> ...15
    ## * `` -> ...16
    ## * `` -> ...17

Read precipitation data for
2017.

``` r
prcip_2017 = read_excel("./data/Trash-Wheel-Collection-Totals-8-6-19.xlsx", sheet = 6, skip = 1) %>%
  janitor::clean_names() %>%
  drop_na(month) %>%
  mutate(year = 2017)
```

Read precipitation data for
2018.

``` r
prcip_2018 = read_excel("./data/Trash-Wheel-Collection-Totals-8-6-19.xlsx", sheet = 5, skip = 1) %>%
  janitor::clean_names() %>%
  drop_na(month) %>%
  mutate(year = 2018)
```

Combine precipitation datasets and convert month to a character
variable.

``` r
month_df = 
  tibble(
    month = 1:12,
    month_name = month.name
  )

prcip_1718 = 
  bind_rows(prcip_2017, prcip_2018)

left_join(prcip_1718, month_df, by = "month")
```

    ## # A tibble: 24 x 4
    ##    month total  year month_name
    ##    <dbl> <dbl> <dbl> <chr>     
    ##  1     1  2.34  2017 January   
    ##  2     2  1.46  2017 February  
    ##  3     3  3.57  2017 March     
    ##  4     4  3.99  2017 April     
    ##  5     5  5.64  2017 May       
    ##  6     6  1.4   2017 June      
    ##  7     7  7.09  2017 July      
    ##  8     8  4.44  2017 August    
    ##  9     9  1.95  2017 September 
    ## 10    10  0     2017 October   
    ## # … with 14 more rows

The dataset contains information of a water-wheel vessel that removes
trash from the Inner Harbor in Baltimore, recording month, year, type of
transh each dumpster collects, and weight and volume of trash collected.
There are 344 obeservations in final dataset. The total precipitation in
2018 was 2.44346610^{4}. The median number of sports balls in a dumpster
in 2017 was 8.
