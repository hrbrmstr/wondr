
`wondr` : Tools to Work with the [CDC WONDER API](https://wonder.cdc.gov/wonder/help/WONDER-API.html)

The following functions are implemented:

-   `add_param`: Add a parameter to a WONDER API query call
-   `make_query`: Issue a WONDER API query
-   `wondr`: Start a query for the CDC WONDER API

### Installation

``` r
devtools::install_git("https://gitlab.com/hrbrmstr/wondr.git")
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
  })
```

    ##                                        V1                                               V2            V3            V4
    ## 1                           Unintentional                                       Cut/Pierce            97 1,243,249,173
    ## 2                           Unintentional                                         Drowning        15,945 1,243,249,173
    ## 3                           Unintentional                                             Fall         2,213 1,243,249,173
    ## 4                           Unintentional                                       Fire/Flame         7,301 1,243,249,173
    ## 5                           Unintentional                             Hot object/Substance            96 1,243,249,173
    ## 6                           Unintentional                                          Firearm         2,042 1,243,249,173
    ## 7                           Unintentional                                        Machinery           484 1,243,249,173
    ## 8                           Unintentional                            Motor Vehicle Traffic        74,997 1,243,249,173
    ## 9                           Unintentional                              Other Pedal cyclist           458 1,243,249,173
    ## 10                          Unintentional                                 Other Pedestrian         3,221 1,243,249,173
    ## 11                          Unintentional                             Other land transport         3,449 1,243,249,173
    ## 12                          Unintentional                                  Other transport         1,344 1,243,249,173
    ## 13                          Unintentional                            Natural/Environmental         1,681 1,243,249,173
    ## 14                          Unintentional                                     Overexertion             2 1,243,249,173
    ## 15                          Unintentional                                        Poisoning         7,326 1,243,249,173
    ## 16                          Unintentional                             Struck by or against         1,378 1,243,249,173
    ## 17                          Unintentional                                      Suffocation        17,356 1,243,249,173
    ## 18                          Unintentional             Other specified, classifiable Injury         1,199 1,243,249,173
    ## 19                          Unintentional Other specified, not elsewhere classified Injury           518 1,243,249,173
    ## 20                          Unintentional                               Unspecified Injury         1,540 1,243,249,173
    ## 21                          Unintentional                                                1       142,647 1,243,249,173
    ## 22                                Suicide                                       Cut/Pierce            64 1,243,249,173
    ## 23                                Suicide                                         Drowning           111 1,243,249,173
    ## 24                                Suicide                                             Fall           412 1,243,249,173
    ## 25                                Suicide                                       Fire/Flame            50 1,243,249,173
    ## 26                                Suicide                                          Firearm         9,956 1,243,249,173
    ## 27                                Suicide                             Other land transport           139 1,243,249,173
    ## 28                                Suicide                                        Poisoning         1,336 1,243,249,173
    ## 29                                Suicide                                      Suffocation        10,559 1,243,249,173
    ## 30                                Suicide             Other specified, classifiable Injury           379 1,243,249,173
    ## 31                                Suicide Other specified, not elsewhere classified Injury           103 1,243,249,173
    ## 32                                Suicide                               Unspecified Injury            76 1,243,249,173
    ## 33                                Suicide                                                1        23,185 1,243,249,173
    ## 34                               Homicide                                       Cut/Pierce         2,375 1,243,249,173
    ## 35                               Homicide                                         Drowning           459 1,243,249,173
    ## 36                               Homicide                                             Fall            33 1,243,249,173
    ## 37                               Homicide                                       Fire/Flame           561 1,243,249,173
    ## 38                               Homicide                             Hot object/Substance            47 1,243,249,173
    ## 39                               Homicide                                          Firearm        20,897 1,243,249,173
    ## 40                               Homicide                             Other land transport           131 1,243,249,173
    ## 41                               Homicide                                  Other transport             8 1,243,249,173
    ## 42                               Homicide                                        Poisoning           494 1,243,249,173
    ## 43                               Homicide                             Struck by or against           338 1,243,249,173
    ## 44                               Homicide                                      Suffocation         1,582 1,243,249,173
    ## 45                               Homicide             Other specified, classifiable Injury         2,912 1,243,249,173
    ## 46                               Homicide Other specified, not elsewhere classified Injury         1,137 1,243,249,173
    ## 47                               Homicide                               Unspecified Injury         5,821 1,243,249,173
    ## 48                               Homicide                                                1        36,795 1,243,249,173
    ## 49                          Undetermined                                        Cut/Pierce            10 1,243,249,173
    ## 50                          Undetermined                                          Drowning           377 1,243,249,173
    ## 51                          Undetermined                                              Fall            96 1,243,249,173
    ## 52                          Undetermined                                        Fire/Flame           256 1,243,249,173
    ## 53                          Undetermined                              Hot object/Substance             4 1,243,249,173
    ## 54                          Undetermined                                           Firearm           501 1,243,249,173
    ## 55                          Undetermined                              Other land transport            33 1,243,249,173
    ## 56                          Undetermined                                         Poisoning         1,217 1,243,249,173
    ## 57                          Undetermined                              Struck by or against             2 1,243,249,173
    ## 58                          Undetermined                                       Suffocation         1,234 1,243,249,173
    ## 59                          Undetermined              Other specified, classifiable Injury            23 1,243,249,173
    ## 60                          Undetermined  Other specified, not elsewhere classified Injury           267 1,243,249,173
    ## 61                          Undetermined                                Unspecified Injury           743 1,243,249,173
    ## 62                          Undetermined                                                 1         4,763 1,243,249,173
    ## 63 Legal Intervention / Operations of War                                          Firearm           251 1,243,249,173
    ## 64 Legal Intervention / Operations of War             Other specified, classifiable Injury             1 1,243,249,173
    ## 65 Legal Intervention / Operations of War Other specified, not elsewhere classified Injury            10 1,243,249,173
    ## 66 Legal Intervention / Operations of War                               Unspecified Injury             2 1,243,249,173
    ## 67 Legal Intervention / Operations of War                                                1           264 1,243,249,173
    ## 68                                      2                                          207,654 1,243,249,173          16.7
    ##            V5
    ## 1         0.0
    ## 2         1.3
    ## 3         0.2
    ## 4         0.6
    ## 5         0.0
    ## 6         0.2
    ## 7         0.0
    ## 8         6.0
    ## 9         0.0
    ## 10        0.3
    ## 11        0.3
    ## 12        0.1
    ## 13        0.1
    ## 14 Unreliable
    ## 15        0.6
    ## 16        0.1
    ## 17        1.4
    ## 18        0.1
    ## 19        0.0
    ## 20        0.1
    ## 21       11.5
    ## 22        0.0
    ## 23        0.0
    ## 24        0.0
    ## 25        0.0
    ## 26        0.8
    ## 27        0.0
    ## 28        0.1
    ## 29        0.8
    ## 30        0.0
    ## 31        0.0
    ## 32        0.0
    ## 33        1.9
    ## 34        0.2
    ## 35        0.0
    ## 36        0.0
    ## 37        0.0
    ## 38        0.0
    ## 39        1.7
    ## 40        0.0
    ## 41 Unreliable
    ## 42        0.0
    ## 43        0.0
    ## 44        0.1
    ## 45        0.2
    ## 46        0.1
    ## 47        0.5
    ## 48        3.0
    ## 49 Unreliable
    ## 50        0.0
    ## 51        0.0
    ## 52        0.0
    ## 53 Unreliable
    ## 54        0.0
    ## 55        0.0
    ## 56        0.1
    ## 57 Unreliable
    ## 58        0.1
    ## 59        0.0
    ## 60        0.0
    ## 61        0.1
    ## 62        0.4
    ## 63        0.0
    ## 64 Unreliable
    ## 65 Unreliable
    ## 66 Unreliable
    ## 67        0.0
    ## 68       <NA>
