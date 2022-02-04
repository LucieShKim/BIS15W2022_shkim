---
title: "Lab 6 Homework"
author: "Songhee Kim"
date: "2022-02-03"
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
fisheries <- readr::read_csv("data/FAO_1950to2012_111914.csv")
```

```
## Rows: 17692 Columns: 71
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (69): Country, Common name, ISSCAAP taxonomic group, ASFIS species#, ASF...
## dbl  (2): ISSCAAP group#, FAO major fishing area
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?  

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

```r
fisheries
```

```
## # A tibble: 17,692 x 71
##    Country `Common name`      `ISSCAAP group#` `ISSCAAP taxon~` `ASFIS species#`
##    <chr>   <chr>                         <dbl> <chr>            <chr>           
##  1 Albania Angelsharks, sand~               38 Sharks, rays, c~ 10903XXXXX      
##  2 Albania Atlantic bonito                  36 Tunas, bonitos,~ 1750100101      
##  3 Albania Barracudas nei                   37 Miscellaneous p~ 17710001XX      
##  4 Albania Blue and red shri~               45 Shrimps, prawns  2280203101      
##  5 Albania Blue whiting(=Pou~               32 Cods, hakes, ha~ 1480403301      
##  6 Albania Bluefish                         37 Miscellaneous p~ 1702021301      
##  7 Albania Bogue                            33 Miscellaneous c~ 1703926101      
##  8 Albania Caramote prawn                   45 Shrimps, prawns  2280100117      
##  9 Albania Catsharks, nurseh~               38 Sharks, rays, c~ 10801003XX      
## 10 Albania Common cuttlefish                57 Squids, cuttlef~ 3210200202      
## # ... with 17,682 more rows, and 66 more variables: `ASFIS species name` <chr>,
## #   `FAO major fishing area` <dbl>, Measure <chr>, `1950` <chr>, `1951` <chr>,
## #   `1952` <chr>, `1953` <chr>, `1954` <chr>, `1955` <chr>, `1956` <chr>,
## #   `1957` <chr>, `1958` <chr>, `1959` <chr>, `1960` <chr>, `1961` <chr>,
## #   `1962` <chr>, `1963` <chr>, `1964` <chr>, `1965` <chr>, `1966` <chr>,
## #   `1967` <chr>, `1968` <chr>, `1969` <chr>, `1970` <chr>, `1971` <chr>,
## #   `1972` <chr>, `1973` <chr>, `1974` <chr>, `1975` <chr>, `1976` <chr>, ...
```


```r
head(fisheries)
```

```
## # A tibble: 6 x 71
##   Country `Common name`       `ISSCAAP group#` `ISSCAAP taxon~` `ASFIS species#`
##   <chr>   <chr>                          <dbl> <chr>            <chr>           
## 1 Albania Angelsharks, sand ~               38 Sharks, rays, c~ 10903XXXXX      
## 2 Albania Atlantic bonito                   36 Tunas, bonitos,~ 1750100101      
## 3 Albania Barracudas nei                    37 Miscellaneous p~ 17710001XX      
## 4 Albania Blue and red shrimp               45 Shrimps, prawns  2280203101      
## 5 Albania Blue whiting(=Pout~               32 Cods, hakes, ha~ 1480403301      
## 6 Albania Bluefish                          37 Miscellaneous p~ 1702021301      
## # ... with 66 more variables: `ASFIS species name` <chr>,
## #   `FAO major fishing area` <dbl>, Measure <chr>, `1950` <chr>, `1951` <chr>,
## #   `1952` <chr>, `1953` <chr>, `1954` <chr>, `1955` <chr>, `1956` <chr>,
## #   `1957` <chr>, `1958` <chr>, `1959` <chr>, `1960` <chr>, `1961` <chr>,
## #   `1962` <chr>, `1963` <chr>, `1964` <chr>, `1965` <chr>, `1966` <chr>,
## #   `1967` <chr>, `1968` <chr>, `1969` <chr>, `1970` <chr>, `1971` <chr>,
## #   `1972` <chr>, `1973` <chr>, `1974` <chr>, `1975` <chr>, `1976` <chr>, ...
```


```r
head(fisheries)
```

```
## # A tibble: 6 x 71
##   Country `Common name`       `ISSCAAP group#` `ISSCAAP taxon~` `ASFIS species#`
##   <chr>   <chr>                          <dbl> <chr>            <chr>           
## 1 Albania Angelsharks, sand ~               38 Sharks, rays, c~ 10903XXXXX      
## 2 Albania Atlantic bonito                   36 Tunas, bonitos,~ 1750100101      
## 3 Albania Barracudas nei                    37 Miscellaneous p~ 17710001XX      
## 4 Albania Blue and red shrimp               45 Shrimps, prawns  2280203101      
## 5 Albania Blue whiting(=Pout~               32 Cods, hakes, ha~ 1480403301      
## 6 Albania Bluefish                          37 Miscellaneous p~ 1702021301      
## # ... with 66 more variables: `ASFIS species name` <chr>,
## #   `FAO major fishing area` <dbl>, Measure <chr>, `1950` <chr>, `1951` <chr>,
## #   `1952` <chr>, `1953` <chr>, `1954` <chr>, `1955` <chr>, `1956` <chr>,
## #   `1957` <chr>, `1958` <chr>, `1959` <chr>, `1960` <chr>, `1961` <chr>,
## #   `1962` <chr>, `1963` <chr>, `1964` <chr>, `1965` <chr>, `1966` <chr>,
## #   `1967` <chr>, `1968` <chr>, `1969` <chr>, `1970` <chr>, `1971` <chr>,
## #   `1972` <chr>, `1973` <chr>, `1974` <chr>, `1975` <chr>, `1976` <chr>, ...
```


2. Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor. 

```r
library("janitor")
fisheries<-clean_names(fisheries)
```


```r
fisheries$country<-as.factor(fisheries$country)
fisheries$isscaap_group_number<-as.factor(fisheries$isscaap_group_number)
fisheries$asfis_species_number<-as.factor(fisheries$asfis_species_number)
fisheries$fao_major_fishing_area<-as.factor(fisheries$fao_major_fishing_area)
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


```r
fisheries_tidy
```

```
## # A tibble: 376,771 x 10
##    country common_name        isscaap_group_n~ isscaap_taxonom~ asfis_species_n~
##    <fct>   <chr>              <fct>            <chr>            <fct>           
##  1 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  2 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  3 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  4 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  5 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  6 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  7 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  8 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  9 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
## 10 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
## # ... with 376,761 more rows, and 5 more variables: asfis_species_name <chr>,
## #   fao_major_fishing_area <fct>, measure <chr>, year <dbl>, catch <dbl>
```



3. How many countries are represented in the data? Provide a count and list their names.

```r
count(fisheries_tidy,country)
```

```
## # A tibble: 203 x 2
##    country                 n
##    <fct>               <int>
##  1 Albania               934
##  2 Algeria              1561
##  3 American Samoa        556
##  4 Angola               2119
##  5 Anguilla              129
##  6 Antigua and Barbuda   356
##  7 Argentina            3403
##  8 Aruba                 172
##  9 Australia            8183
## 10 Bahamas               423
## # ... with 193 more rows
```


#answer

```r
fisheries_tidy%>%
  summarise(count_country=n_distinct(country))
```

```
## # A tibble: 1 x 1
##   count_country
##           <int>
## 1           203
```


```r
levels(fisheries_tidy$country)
```

```
##   [1] "Albania"                   "Algeria"                  
##   [3] "American Samoa"            "Angola"                   
##   [5] "Anguilla"                  "Antigua and Barbuda"      
##   [7] "Argentina"                 "Aruba"                    
##   [9] "Australia"                 "Bahamas"                  
##  [11] "Bahrain"                   "Bangladesh"               
##  [13] "Barbados"                  "Belgium"                  
##  [15] "Belize"                    "Benin"                    
##  [17] "Bermuda"                   "Bonaire/S.Eustatius/Saba" 
##  [19] "Bosnia and Herzegovina"    "Brazil"                   
##  [21] "British Indian Ocean Ter"  "British Virgin Islands"   
##  [23] "Brunei Darussalam"         "Bulgaria"                 
##  [25] "C<f4>te d'Ivoire"          "Cabo Verde"               
##  [27] "Cambodia"                  "Cameroon"                 
##  [29] "Canada"                    "Cayman Islands"           
##  [31] "Channel Islands"           "Chile"                    
##  [33] "China"                     "China, Hong Kong SAR"     
##  [35] "China, Macao SAR"          "Colombia"                 
##  [37] "Comoros"                   "Congo, Dem. Rep. of the"  
##  [39] "Congo, Republic of"        "Cook Islands"             
##  [41] "Costa Rica"                "Croatia"                  
##  [43] "Cuba"                      "Cura<e7>ao"               
##  [45] "Cyprus"                    "Denmark"                  
##  [47] "Djibouti"                  "Dominica"                 
##  [49] "Dominican Republic"        "Ecuador"                  
##  [51] "Egypt"                     "El Salvador"              
##  [53] "Equatorial Guinea"         "Eritrea"                  
##  [55] "Estonia"                   "Ethiopia"                 
##  [57] "Falkland Is.(Malvinas)"    "Faroe Islands"            
##  [59] "Fiji, Republic of"         "Finland"                  
##  [61] "France"                    "French Guiana"            
##  [63] "French Polynesia"          "French Southern Terr"     
##  [65] "Gabon"                     "Gambia"                   
##  [67] "Georgia"                   "Germany"                  
##  [69] "Ghana"                     "Gibraltar"                
##  [71] "Greece"                    "Greenland"                
##  [73] "Grenada"                   "Guadeloupe"               
##  [75] "Guam"                      "Guatemala"                
##  [77] "Guinea"                    "GuineaBissau"             
##  [79] "Guyana"                    "Haiti"                    
##  [81] "Honduras"                  "Iceland"                  
##  [83] "India"                     "Indonesia"                
##  [85] "Iran (Islamic Rep. of)"    "Iraq"                     
##  [87] "Ireland"                   "Isle of Man"              
##  [89] "Israel"                    "Italy"                    
##  [91] "Jamaica"                   "Japan"                    
##  [93] "Jordan"                    "Kenya"                    
##  [95] "Kiribati"                  "Korea, Dem. People's Rep" 
##  [97] "Korea, Republic of"        "Kuwait"                   
##  [99] "Latvia"                    "Lebanon"                  
## [101] "Liberia"                   "Libya"                    
## [103] "Lithuania"                 "Madagascar"               
## [105] "Malaysia"                  "Maldives"                 
## [107] "Malta"                     "Marshall Islands"         
## [109] "Martinique"                "Mauritania"               
## [111] "Mauritius"                 "Mayotte"                  
## [113] "Mexico"                    "Micronesia, Fed.States of"
## [115] "Monaco"                    "Montenegro"               
## [117] "Montserrat"                "Morocco"                  
## [119] "Mozambique"                "Myanmar"                  
## [121] "Namibia"                   "Nauru"                    
## [123] "Netherlands"               "Netherlands Antilles"     
## [125] "New Caledonia"             "New Zealand"              
## [127] "Nicaragua"                 "Nigeria"                  
## [129] "Niue"                      "Norfolk Island"           
## [131] "Northern Mariana Is."      "Norway"                   
## [133] "Oman"                      "Other nei"                
## [135] "Pakistan"                  "Palau"                    
## [137] "Palestine, Occupied Tr."   "Panama"                   
## [139] "Papua New Guinea"          "Peru"                     
## [141] "Philippines"               "Pitcairn Islands"         
## [143] "Poland"                    "Portugal"                 
## [145] "Puerto Rico"               "Qatar"                    
## [147] "R<e9>union"                "Romania"                  
## [149] "Russian Federation"        "Saint Barth<e9>lemy"      
## [151] "Saint Helena"              "Saint Kitts and Nevis"    
## [153] "Saint Lucia"               "Saint Vincent/Grenadines" 
## [155] "SaintMartin"               "Samoa"                    
## [157] "Sao Tome and Principe"     "Saudi Arabia"             
## [159] "Senegal"                   "Serbia and Montenegro"    
## [161] "Seychelles"                "Sierra Leone"             
## [163] "Singapore"                 "Sint Maarten"             
## [165] "Slovenia"                  "Solomon Islands"          
## [167] "Somalia"                   "South Africa"             
## [169] "Spain"                     "Sri Lanka"                
## [171] "St. Pierre and Miquelon"   "Sudan"                    
## [173] "Sudan (former)"            "Suriname"                 
## [175] "Svalbard and Jan Mayen"    "Sweden"                   
## [177] "Syrian Arab Republic"      "Taiwan Province of China" 
## [179] "Tanzania, United Rep. of"  "Thailand"                 
## [181] "TimorLeste"                "Togo"                     
## [183] "Tokelau"                   "Tonga"                    
## [185] "Trinidad and Tobago"       "Tunisia"                  
## [187] "Turkey"                    "Turks and Caicos Is."     
## [189] "Tuvalu"                    "Ukraine"                  
## [191] "Un. Sov. Soc. Rep."        "United Arab Emirates"     
## [193] "United Kingdom"            "United States of America" 
## [195] "Uruguay"                   "US Virgin Islands"        
## [197] "Vanuatu"                   "Venezuela, Boliv Rep of"  
## [199] "Viet Nam"                  "Wallis and Futuna Is."    
## [201] "Western Sahara"            "Yemen"                    
## [203] "Yugoslavia SFR"            "Zanzibar"
```


4. Refocus the data only to include country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.

```r
fisheries_tidy%>%
  select(country,isscaap_taxonomic_group,asfis_species_name,asfis_species_number,year,catch)
```

```
## # A tibble: 376,771 x 6
##    country isscaap_taxonomic_group asfis_species_n~ asfis_species_n~  year catch
##    <fct>   <chr>                   <chr>            <fct>            <dbl> <dbl>
##  1 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1995    NA
##  2 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1996    53
##  3 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1997    20
##  4 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1998    31
##  5 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1999    30
##  6 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2000    30
##  7 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2001    16
##  8 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2002    79
##  9 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2003     1
## 10 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2004     4
## # ... with 376,761 more rows
```

#answer

```r
fisheries_tidy_focused <- fisheries_tidy %>% 
  select(country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch)
fisheries_tidy_focused
```

```
## # A tibble: 376,771 x 6
##    country isscaap_taxonomic_group asfis_species_n~ asfis_species_n~  year catch
##    <fct>   <chr>                   <chr>            <fct>            <dbl> <dbl>
##  1 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1995    NA
##  2 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1996    53
##  3 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1997    20
##  4 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1998    31
##  5 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1999    30
##  6 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2000    30
##  7 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2001    16
##  8 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2002    79
##  9 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2003     1
## 10 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2004     4
## # ... with 376,761 more rows
```


5. Based on the asfis_species_number, how many distinct fish species were caught as part of these data?

```r
fisheries_tidy%>%
  summarise(n_asfis_species=n_distinct(asfis_species_number))
```

```
## # A tibble: 1 x 1
##   n_asfis_species
##             <int>
## 1            1551
```

#answer

```r
fisheries_tidy_focused %>% 
  summarize(n_taxa=n_distinct(asfis_species_number))
```

```
## # A tibble: 1 x 1
##   n_taxa
##    <int>
## 1   1551
```


6. Which country had the largest overall catch in the year 2000?

```r
fisheries_tidy%>%
  filter(year=="2000")%>%
  group_by(country)%>%
  arrange(desc(catch))
```

```
## # A tibble: 8,793 x 10
## # Groups:   country [193]
##    country        common_name isscaap_group_n~ isscaap_taxonom~ asfis_species_n~
##    <fct>          <chr>       <fct>            <chr>            <fct>           
##  1 China          Marine fis~ 39               Marine fishes n~ 199XXXXXXX010   
##  2 Peru           Anchoveta(~ 35               Herrings, sardi~ 1210600208      
##  3 Russian Feder~ Alaska pol~ 32               Cods, hakes, ha~ 1480401601      
##  4 Viet Nam       Marine fis~ 39               Marine fishes n~ 199XXXXXXX010   
##  5 Chile          Chilean ja~ 37               Miscellaneous p~ 1702300405      
##  6 China          Marine mol~ 58               Miscellaneous m~ 399XXXXXXX016   
##  7 China          Largehead ~ 34               Miscellaneous d~ 1750600302      
##  8 United States~ Alaska pol~ 32               Cods, hakes, ha~ 1480401601      
##  9 China          Marine cru~ 47               Miscellaneous m~ 299XXXXXXX013   
## 10 Philippines    Scads nei   37               Miscellaneous p~ 17023043XX      
## # ... with 8,783 more rows, and 5 more variables: asfis_species_name <chr>,
## #   fao_major_fishing_area <fct>, measure <chr>, year <dbl>, catch <dbl>
```

#answer

```r
fisheries_tidy_focused %>% 
  filter(year==2000) %>%
  group_by(country) %>% 
  summarize(catch_total=sum(catch, na.rm=T)) %>% 
  arrange(desc(catch_total))
```

```
## # A tibble: 193 x 2
##    country                  catch_total
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
## # ... with 183 more rows
```


```r
fisheries_tidy
```

```
## # A tibble: 376,771 x 10
##    country common_name        isscaap_group_n~ isscaap_taxonom~ asfis_species_n~
##    <fct>   <chr>              <fct>            <chr>            <fct>           
##  1 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  2 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  3 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  4 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  5 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  6 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  7 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  8 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
##  9 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
## 10 Albania Angelsharks, sand~ 38               Sharks, rays, c~ 10903XXXXX      
## # ... with 376,761 more rows, and 5 more variables: asfis_species_name <chr>,
## #   fao_major_fishing_area <fct>, measure <chr>, year <dbl>, catch <dbl>
```


7. Which country caught the most sardines (_Sardina pilchardus_) between the years 1990-2000?



```r
fisheries_tidy%>%
  filter(asfis_species_name=="Sardina pilchardus")%>%
  filter(between(year,1990,2000))%>%
  group_by(country)%>%
  summarise(catch_n=sum(catch,rm.na=T))%>%
  arrange(desc(catch_n))
```

```
## # A tibble: 37 x 2
##    country               catch_n
##    <fct>                   <dbl>
##  1 Morocco                  7471
##  2 Spain                    3508
##  3 Russian Federation       1640
##  4 Ukraine                  1031
##  5 Portugal                  819
##  6 Greece                    529
##  7 Italy                     508
##  8 Serbia and Montenegro     479
##  9 Denmark                   478
## 10 Tunisia                   428
## # ... with 27 more rows
```


8. Which five countries caught the most cephalopods between 2008-2012?



```r
ceph<-fisheries_tidy%>%
  filter(str_detect(isscaap_taxonomic_group,"Squids"))%>%
  filter(between(year,2008,2012))%>%
  group_by(country)%>%
  summarise(catch_ceph_n=sum(catch,rm.na=T))%>%
  arrange(desc(catch_ceph_n))
```



```r
head(ceph,n=5)
```

```
## # A tibble: 5 x 2
##   country                 catch_ceph_n
##   <fct>                          <dbl>
## 1 Peru                            3423
## 2 Thailand                         950
## 3 India                            571
## 4 Palestine, Occupied Tr.          413
## 5 Malta                            399
```

9. Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)

```r
fisheries_tidy%>%
  filter(between(year,2008,2012))%>%
  group_by(asfis_species_name)%>%
  summarise(catch_t=sum(catch,rm.na=T))%>%
  arrange(desc(catch_t))
```

```
## # A tibble: 1,472 x 2
##    asfis_species_name    catch_t
##    <chr>                   <dbl>
##  1 Theragra chalcogramma   41076
##  2 Engraulis ringens       35524
##  3 Cololabis saira          5734
##  4 Sardinella longiceps     3850
##  5 Sardinops caeruleus      3205
##  6 Brevoortia patronus      3180
##  7 Acetes japonicus         2916
##  8 Squillidae               2885
##  9 Trachurus japonicus      2711
## 10 Ammodytes personatus     2612
## # ... with 1,462 more rows
```

10. Use the data to do at least one analysis of your choice.

```r
fisheries_tidy%>%
  filter(between(year,2001,2013))%>%
  group_by(country)%>%
  summarise(catch_tt=sum(catch,rm.na=T))%>%
  arrange(desc(catch_tt))
```

```
## # A tibble: 200 x 2
##    country                  catch_tt
##    <fct>                       <dbl>
##  1 "Myanmar"                   59371
##  2 "Jordan"                     1890
##  3 "Aruba"                      1886
##  4 "Sudan (former)"             1315
##  5 "TimorLeste"                 1033
##  6 "Montserrat"                  430
##  7 "Saint Barth\xe9lemy"         301
##  8 "Sudan"                       121
##  9 "Bosnia and Herzegovina"       61
## 10 "Pitcairn Islands"             43
## # ... with 190 more rows
```



## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
