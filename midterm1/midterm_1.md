---
title: "Midterm 1"
author: "Please Add Your Name Here"
date: "2022-02-01"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 15 total questions, each is worth 2 points.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by 12:00p on Thursday, January 27.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.7
## v tidyr   1.1.4     v stringr 1.4.0
## v readr   2.1.2     v forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

## Questions  
Wikipedia's definition of [data science](https://en.wikipedia.org/wiki/Data_science): "Data science is an interdisciplinary field that uses scientific methods, processes, algorithms and systems to extract knowledge and insights from noisy, structured and unstructured data, and apply knowledge and actionable insights from data across a broad range of application domains."  

1. (2 points) Consider the definition of data science above. Although we are only part-way through the quarter, what specific elements of data science do you feel we have practiced? Provide at least one specific example.  
#These days, as there is a concept called big data, the amount of data is huge. I think that programming like r is suitable to handle such a lot of data effectively and quickly, for example using data frame, I can quickly and easily organize a lot of data.

2. (2 points) What is the most helpful or interesting thing you have learned so far in BIS 15L? What is something that you think needs more work or practice?  
#data frame/ It would be good to practice to handle the data frame more freely because majoring in bio informatics, I should be able to classify big data like about numerous dna or patients and extract and use only the necessary information for the purpose.
In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.


```r
elephants<-readr::read_csv("data/ElephantsMF.csv")
```

```
## Rows: 288 Columns: 3
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (1): Sex
## dbl (2): Age, Height
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
spec(elephants)
```

```
## cols(
##   Age = col_double(),
##   Height = col_double(),
##   Sex = col_character()
## )
```
4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.

```r
elephants<-rename(elephants,age="Age",height="Height",sex="Sex")
```


```r
#another way
elephants<-select_all(elephants,tolower)
```



```r
elephants$sex<-as.factor(elephants$sex)
elephants
```

```
## # A tibble: 288 x 3
##      age height sex  
##    <dbl>  <dbl> <fct>
##  1   1.4   120  M    
##  2  17.5   227  M    
##  3  12.8   235  M    
##  4  11.2   210  M    
##  5  12.7   220  M    
##  6  12.7   189  M    
##  7  12.2   225  M    
##  8  12.2   204  M    
##  9  28.2   266. M    
## 10  11.7   233  M    
## # ... with 278 more rows
```


```r
class(elephants$sex)
```

```
## [1] "factor"
```


5. (2 points) How many male and female elephants are represented in the data?

```r
table(elephants$sex)
```

```
## 
##   F   M 
## 150 138
```

6. (2 points) What is the average age all elephants in the data?

```r
mean(elephants$age)
```

```
## [1] 10.97132
```

7. (2 points) How does the average age and height of elephants compare by sex?

```r
ele_f<-subset.data.frame(elephants,sex=="F")
mean(ele_f$age)
```

```
## [1] 12.8354
```

```r
mean(ele_f$height)
```

```
## [1] 190.0307
```


```r
ele_m<-subset.data.frame(elephants,sex=="M")
mean(ele_m$age)
```

```
## [1] 8.945145
```

```r
mean(ele_m$height)
```

```
## [1] 185.1312
```

```r
#another way
ele_mm<-filter(elephants,sex=="M")
mean(ele_mm$age)
```

```
## [1] 8.945145
```

```r
mean(ele_mm$height)
```

```
## [1] 185.1312
```

8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.  

```r
ele_m20<-filter(ele_m,age>20)
```


```r
summary(ele_m20)
```

```
##       age            height      sex   
##  Min.   :21.25   Min.   :228.7   F: 0  
##  1st Qu.:24.17   1st Qu.:253.1   M:13  
##  Median :25.42   Median :268.6         
##  Mean   :25.19   Mean   :269.6         
##  3rd Qu.:26.50   3rd Qu.:287.1         
##  Max.   :28.75   Max.   :304.1
```

```r
dim(ele_m20)
```

```
## [1] 13  3
```


```r
ele_f20<-filter(ele_f,age>20)
```


```r
summary(ele_f20)
```

```
##       age            height      sex   
##  Min.   :20.17   Min.   :192.5   F:37  
##  1st Qu.:23.33   1st Qu.:221.6   M: 0  
##  Median :26.08   Median :227.3         
##  Mean   :25.63   Mean   :232.2         
##  3rd Qu.:27.42   3rd Qu.:241.9         
##  Max.   :32.17   Max.   :277.8
```

```r
dim(ele_f20)
```

```
## [1] 37  3
```

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.


```r
midterm_dt<-readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## Rows: 24 Columns: 26
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr  (2): HuntCat, LandUse
## dbl (24): TransectID, Distance, NumHouseholds, Veg_Rich, Veg_Stems, Veg_lian...
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
spec(midterm_dt )
```

```
## cols(
##   TransectID = col_double(),
##   Distance = col_double(),
##   HuntCat = col_character(),
##   NumHouseholds = col_double(),
##   LandUse = col_character(),
##   Veg_Rich = col_double(),
##   Veg_Stems = col_double(),
##   Veg_liana = col_double(),
##   Veg_DBH = col_double(),
##   Veg_Canopy = col_double(),
##   Veg_Understory = col_double(),
##   RA_Apes = col_double(),
##   RA_Birds = col_double(),
##   RA_Elephant = col_double(),
##   RA_Monkeys = col_double(),
##   RA_Rodent = col_double(),
##   RA_Ungulate = col_double(),
##   Rich_AllSpecies = col_double(),
##   Evenness_AllSpecies = col_double(),
##   Diversity_AllSpecies = col_double(),
##   Rich_BirdSpecies = col_double(),
##   Evenness_BirdSpecies = col_double(),
##   Diversity_BirdSpecies = col_double(),
##   Rich_MammalSpecies = col_double(),
##   Evenness_MammalSpecies = col_double(),
##   Diversity_MammalSpecies = col_double()
## )
```

```r
summary(midterm_dt)
```

```
##    TransectID       Distance        HuntCat          NumHouseholds  
##  Min.   : 1.00   Min.   : 2.700   Length:24          Min.   :13.00  
##  1st Qu.: 5.75   1st Qu.: 5.668   Class :character   1st Qu.:24.75  
##  Median :14.50   Median : 9.720   Mode  :character   Median :29.00  
##  Mean   :13.50   Mean   :11.879                      Mean   :37.88  
##  3rd Qu.:20.25   3rd Qu.:17.683                      3rd Qu.:54.00  
##  Max.   :27.00   Max.   :26.760                      Max.   :73.00  
##    LandUse             Veg_Rich       Veg_Stems       Veg_liana     
##  Length:24          Min.   :10.88   Min.   :23.44   Min.   : 4.750  
##  Class :character   1st Qu.:13.10   1st Qu.:28.69   1st Qu.: 9.033  
##  Mode  :character   Median :14.94   Median :32.45   Median :11.940  
##                     Mean   :14.83   Mean   :32.80   Mean   :11.040  
##                     3rd Qu.:16.54   3rd Qu.:37.08   3rd Qu.:13.250  
##                     Max.   :18.75   Max.   :47.56   Max.   :16.380  
##     Veg_DBH        Veg_Canopy    Veg_Understory     RA_Apes      
##  Min.   :28.45   Min.   :2.500   Min.   :2.380   Min.   : 0.000  
##  1st Qu.:40.65   1st Qu.:3.250   1st Qu.:2.875   1st Qu.: 0.000  
##  Median :43.90   Median :3.430   Median :3.000   Median : 0.485  
##  Mean   :46.09   Mean   :3.469   Mean   :3.020   Mean   : 2.045  
##  3rd Qu.:50.58   3rd Qu.:3.750   3rd Qu.:3.167   3rd Qu.: 3.815  
##  Max.   :76.48   Max.   :4.000   Max.   :3.880   Max.   :12.930  
##     RA_Birds      RA_Elephant       RA_Monkeys      RA_Rodent    
##  Min.   :31.56   Min.   :0.0000   Min.   : 5.84   Min.   :1.060  
##  1st Qu.:52.51   1st Qu.:0.0000   1st Qu.:22.70   1st Qu.:2.047  
##  Median :57.90   Median :0.3600   Median :31.74   Median :3.230  
##  Mean   :58.64   Mean   :0.5450   Mean   :31.30   Mean   :3.278  
##  3rd Qu.:68.17   3rd Qu.:0.8925   3rd Qu.:39.88   3rd Qu.:4.093  
##  Max.   :85.03   Max.   :2.3000   Max.   :54.12   Max.   :6.310  
##   RA_Ungulate     Rich_AllSpecies Evenness_AllSpecies Diversity_AllSpecies
##  Min.   : 0.000   Min.   :15.00   Min.   :0.6680      Min.   :1.966       
##  1st Qu.: 1.232   1st Qu.:19.00   1st Qu.:0.7542      1st Qu.:2.248       
##  Median : 2.545   Median :20.00   Median :0.7760      Median :2.316       
##  Mean   : 4.166   Mean   :20.21   Mean   :0.7699      Mean   :2.310       
##  3rd Qu.: 5.157   3rd Qu.:22.00   3rd Qu.:0.8083      3rd Qu.:2.429       
##  Max.   :13.860   Max.   :24.00   Max.   :0.8330      Max.   :2.566       
##  Rich_BirdSpecies Evenness_BirdSpecies Diversity_BirdSpecies Rich_MammalSpecies
##  Min.   : 8.00    Min.   :0.5590       Min.   :1.162         Min.   : 6.000    
##  1st Qu.:10.00    1st Qu.:0.6825       1st Qu.:1.603         1st Qu.: 9.000    
##  Median :11.00    Median :0.7220       Median :1.680         Median :10.000    
##  Mean   :10.33    Mean   :0.7137       Mean   :1.661         Mean   : 9.875    
##  3rd Qu.:11.00    3rd Qu.:0.7722       3rd Qu.:1.784         3rd Qu.:11.000    
##  Max.   :13.00    Max.   :0.8240       Max.   :2.008         Max.   :12.000    
##  Evenness_MammalSpecies Diversity_MammalSpecies
##  Min.   :0.6190         Min.   :1.378          
##  1st Qu.:0.7073         1st Qu.:1.567          
##  Median :0.7390         Median :1.699          
##  Mean   :0.7477         Mean   :1.698          
##  3rd Qu.:0.7847         3rd Qu.:1.815          
##  Max.   :0.8610         Max.   :2.065
```


```r
glimpse(midterm_dt)
```

```
## Rows: 24
## Columns: 26
## $ TransectID              <dbl> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 16, ~
## $ Distance                <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24.06~
## $ HuntCat                 <chr> "Moderate", "None", "None", "None", "None", "N~
## $ NumHouseholds           <dbl> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46, 56~
## $ LandUse                 <chr> "Park", "Park", "Park", "Logging", "Park", "Pa~
## $ Veg_Rich                <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 14.7~
## $ Veg_Stems               <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 31.2~
## $ Veg_liana               <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38, 8~
## $ Veg_DBH                 <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 51.2~
## $ Veg_Canopy              <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4.00~
## $ Veg_Understory          <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2.71~
## $ RA_Apes                 <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, 6.1~
## $ RA_Birds                <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 42.6~
## $ RA_Elephant             <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0.43~
## $ RA_Monkeys              <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 46.2~
## $ RA_Rodent               <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1.26~
## $ RA_Ungulate             <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10, 8~
## $ Rich_AllSpecies         <dbl> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21, 22~
## $ Evenness_AllSpecies     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0.81~
## $ Diversity_AllSpecies    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2.56~
## $ Rich_BirdSpecies        <dbl> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11, 1~
## $ Evenness_BirdSpecies    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0.80~
## $ Diversity_BirdSpecies   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1.92~
## $ Rich_MammalSpecies      <dbl> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 11, ~
## $ Evenness_MammalSpecies  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0.77~
## $ Diversity_MammalSpecies <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1.92~
```

```r
midterm_dt<-select_all(midterm_dt,tolower)
```


```r
names(midterm_dt)
```

```
##  [1] "transectid"              "distance"               
##  [3] "huntcat"                 "numhouseholds"          
##  [5] "landuse"                 "veg_rich"               
##  [7] "veg_stems"               "veg_liana"              
##  [9] "veg_dbh"                 "veg_canopy"             
## [11] "veg_understory"          "ra_apes"                
## [13] "ra_birds"                "ra_elephant"            
## [15] "ra_monkeys"              "ra_rodent"              
## [17] "ra_ungulate"             "rich_allspecies"        
## [19] "evenness_allspecies"     "diversity_allspecies"   
## [21] "rich_birdspecies"        "evenness_birdspecies"   
## [23] "diversity_birdspecies"   "rich_mammalspecies"     
## [25] "evenness_mammalspecies"  "diversity_mammalspecies"
```


```r
midterm_dt$huntcat<-as.factor(midterm_dt$huntcat)
midterm_dt$landuse<-as.factor(midterm_dt$landuse)
```


```r
class(midterm_dt$huntcat)
```

```
## [1] "factor"
```

```r
class(midterm_dt$landuse)
```

```
## [1] "factor"
```


10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?


```r
midterm_dt
```

```
## # A tibble: 24 x 26
##    transectid distance huntcat  numhouseholds landuse veg_rich veg_stems
##         <dbl>    <dbl> <fct>            <dbl> <fct>      <dbl>     <dbl>
##  1          1     7.14 Moderate            54 Park        16.7      31.2
##  2          2    17.3  None                54 Park        15.8      37.4
##  3          2    18.3  None                29 Park        16.9      32.3
##  4          3    20.8  None                29 Logging     12.4      29.4
##  5          4    16.0  None                29 Park        17.1      36  
##  6          5    17.5  None                29 Park        16.5      29.2
##  7          6    24.1  None                29 Park        14.8      31.2
##  8          7    19.8  None                54 Logging     13.2      32.6
##  9          8     5.78 High                25 Neither     12.6      23.7
## 10          9     5.13 High                73 Logging     16        27.1
## # ... with 14 more rows, and 19 more variables: veg_liana <dbl>, veg_dbh <dbl>,
## #   veg_canopy <dbl>, veg_understory <dbl>, ra_apes <dbl>, ra_birds <dbl>,
## #   ra_elephant <dbl>, ra_monkeys <dbl>, ra_rodent <dbl>, ra_ungulate <dbl>,
## #   rich_allspecies <dbl>, evenness_allspecies <dbl>,
## #   diversity_allspecies <dbl>, rich_birdspecies <dbl>,
## #   evenness_birdspecies <dbl>, diversity_birdspecies <dbl>,
## #   rich_mammalspecies <dbl>, evenness_mammalspecies <dbl>, ...
```



```r
midterm_dt%>%
  filter(huntcat=="Moderate" | huntcat=="High")%>%
  summarise(mean_d_birds=mean(diversity_birdspecies))
```

```
## # A tibble: 1 x 1
##   mean_d_birds
##          <dbl>
## 1         1.64
```



```r
midterm_dt%>%
  filter(huntcat=="Moderate" | huntcat=="High")%>%
  summarise(mean_d_mammals=mean(diversity_mammalspecies))
```

```
## # A tibble: 1 x 1
##   mean_d_mammals
##            <dbl>
## 1           1.71
```


11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.  


12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`


