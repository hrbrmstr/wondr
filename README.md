
`wondr` : Tools to Work with the [CDC WONDER API](https://wonder.cdc.gov/wonder/help/WONDER-API.html)

The following functions are implemented:

-   `add_param`: Add a parameter to a WONDER API query call
-   `make_query`: Issue a WONDER API query
-   `wondr`: Start a query for the CDC WONDER API

### Installation

``` r
devtools::install_github("hrbrmstr/wondr")
```

``` r
options(width=120)
```

### Usage

``` r
library(wondr)

# current verison
packageVersion("wondr")
```

    ## [1] '0.1.0'

``` r
wondr() %>%
  add_param("B_1", "D76.V22") %>% 
  add_param("B_2", "D76.V23") %>% 
  add_param("B_3", "*None*") %>% 
  add_param("B_4", "*None*") %>% 
  add_param("B_5", "*None*") %>% 
  add_param("F_D76.V1", "*All*") %>% 
  add_param("F_D76.V10", "*All*") %>% 
  add_param("F_D76.V2", "*All*") %>% 
  add_param("F_D76.V27", "*All*") %>% 
  add_param("F_D76.V9", "*All*") %>% 
  add_param("I_D76.V1", "*All* (All Dates)") %>% 
  add_param("I_D76.V10", "*All* (The United States)") %>% 
  add_param("I_D76.V2", "*All* (All Causes of Death)") %>% 
  add_param("I_D76.V27", "*All* (The United States)") %>% 
  add_param("I_D76.V9", "*All* (The United States)") %>% 
  add_param("M_1", "D76.M1") %>% 
  add_param("M_2", "D76.M2") %>% 
  add_param("M_3", "D76.M3") %>% 
  add_param("O_V10_fmode", "freg") %>% 
  add_param("O_V1_fmode", "freg") %>% 
  add_param("O_V27_fmode", "freg") %>% 
  add_param("O_V2_fmode", "freg") %>% 
  add_param("O_V9_fmode", "freg") %>% 
  add_param("O_aar", "aar_none") %>% 
  add_param("O_aar_pop", "0000") %>% 
  add_param("O_age", "D76.V52") %>% 
  add_param("O_javascript", "on") %>% 
  add_param("O_location", "D76.V9") %>% 
  add_param("O_precision", "1") %>% 
  add_param("O_rate_per", "100000") %>% 
  add_param("O_show_totals", "true") %>% 
  add_param("O_timeout", "300") %>% 
  add_param("O_title", "Example2") %>% 
  add_param("O_ucd", "D76.V22") %>% 
  add_param("O_urban", "D76.V19") %>% 
  add_param("VM_D76.M6_D76.V10", "") %>% 
  add_param("VM_D76.M6_D76.V17", "*All*") %>% 
  add_param("VM_D76.M6_D76.V1_S", "*All*") %>% 
  add_param("VM_D76.M6_D76.V7", "*All*") %>% 
  add_param("VM_D76.M6_D76.V8", "*All*") %>% 
  add_param("V_D76.V1", "") %>% 
  add_param("V_D76.V10", "") %>% 
  add_param("V_D76.V11", "*All*") %>% 
  add_param("V_D76.V12", "*All*") %>% 
  add_param("V_D76.V17", "*All*") %>% 
  add_param("V_D76.V19", "*All*") %>% 
  add_param("V_D76.V2", "") %>% 
  add_param("V_D76.V20", "*All*") %>% 
  add_param("V_D76.V21", "*All*") %>% 
  add_param("V_D76.V22", "1", "2", "3", "4", "5") %>% 
  add_param("V_D76.V23", "*All*") %>% 
  add_param("V_D76.V24", "*All*") %>% 
  add_param("V_D76.V25", "*All*") %>% 
  add_param("V_D76.V27", "") %>% 
  add_param("V_D76.V4", "*All*") %>% 
  add_param("V_D76.V5", "*All*") %>% 
  add_param("V_D76.V51", "*All*") %>% 
  add_param("V_D76.V52", "0", "1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", "18") %>% 
  add_param("V_D76.V6", "00") %>% 
  add_param("V_D76.V7", "*All*") %>% 
  add_param("V_D76.V8", "*All*") %>% 
  add_param("V_D76.V9", "") %>% 
  add_param("action-Send", "Send") %>% 
  add_param("finder-stage-D76.V1", "codeset") %>% 
  add_param("finder-stage-D76.V10", "codeset") %>% 
  add_param("finder-stage-D76.V2", "codeset") %>% 
  add_param("finder-stage-D76.V27", "codeset") %>% 
  add_param("finder-stage-D76.V9", "codeset") %>% 
  add_param("stage", "request") %>%  
  make_query("D76") -> query_result

library(xml2)
library(purrr)

xml_find_all(query_result, ".//response/data-table/r") %>% 
  map_df(function(row) {
    xml_find_all(row, ".//c") %>% 
      xml_attrs() %>% 
      as.list() %>% 
      setNames(sprintf("V%d", 1:length(.))) %>% 
      as.data.frame(stringsAsFactors=FALSE)
  }) %>% 
  tibble::as.tibble()
```

    ## # A tibble: 68 x 5
    ##               V1                    V2     V3            V4    V5
    ##            <chr>                 <chr>  <chr>         <chr> <chr>
    ##  1 Unintentional            Cut/Pierce     98 1,321,112,279   0.0
    ##  2 Unintentional              Drowning 16,761 1,321,112,279   1.3
    ##  3 Unintentional                  Fall  2,314 1,321,112,279   0.2
    ##  4 Unintentional            Fire/Flame  7,548 1,321,112,279   0.6
    ##  5 Unintentional  Hot object/Substance    100 1,321,112,279   0.0
    ##  6 Unintentional               Firearm  2,123 1,321,112,279   0.2
    ##  7 Unintentional             Machinery    506 1,321,112,279   0.0
    ##  8 Unintentional Motor Vehicle Traffic 77,887 1,321,112,279   5.9
    ##  9 Unintentional   Other Pedal cyclist    483 1,321,112,279   0.0
    ## 10 Unintentional      Other Pedestrian  3,373 1,321,112,279   0.3
    ## # ... with 58 more rows
