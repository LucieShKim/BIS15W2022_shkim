---
title: "Lab 12 Homework"
author: "Songhee Kim"
date: "2022-02-27"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(ggmap)
```


```r
library(albersusa)
```

## Load the Data
We will use two separate data sets for this homework.  

1. The first [data set](https://rcweb.dartmouth.edu/~f002d69/workshops/index_rspatial.html) represent sightings of grizzly bears (Ursos arctos) in Alaska.  

2. The second data set is from Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  


1. Load the `grizzly` data and evaluate its structure. As part of this step, produce a summary that provides the range of latitude and longitude so you can build an appropriate bounding box.

```r
grizzly<-readr::read_csv("data/bear-sightings.csv")
```

```
## Rows: 494 Columns: 3
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## dbl (3): bear.id, longitude, latitude
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
summary(grizzly)
```

```
##     bear.id       longitude         latitude    
##  Min.   :   7   Min.   :-166.2   Min.   :55.02  
##  1st Qu.:2569   1st Qu.:-154.2   1st Qu.:58.13  
##  Median :4822   Median :-151.0   Median :60.97  
##  Mean   :4935   Mean   :-149.1   Mean   :61.41  
##  3rd Qu.:7387   3rd Qu.:-145.6   3rd Qu.:64.13  
##  Max.   :9996   Max.   :-131.3   Max.   :70.37
```


2. Use the range of the latitude and longitude to build an appropriate bounding box for your map.

```r
lat<-c(55.02,70.37)
long<-c(-166.2,-131.3)
bbox<-make_bbox(long,lat,f=0.05)
```

3. Load a map from `stamen` in a terrain style projection and display the map.

```r
map_grz<-get_map(bbox,maptype = "terrain",source = "stamen")
```

```
## Map tiles by Stamen Design, under CC BY 3.0. Data by OpenStreetMap, under ODbL.
```


```r
ggmap(map_grz)
```

![](lab12_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
4. Build a final map that overlays the recorded observations of grizzly bears in Alaska.

```r
ggmap(map_grz)+
  geom_point(data=grizzly,aes(longitude,latitude),color="red", alpha=0.5, size=1)+
  labs(x="Longitude",y="Latitude", title="Grizzly Location")
```

![](lab12_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

Let's switch to the wolves data. Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  

5. Load the data and evaluate its structure.  

```r
wolves_dt<-readr::read_csv("data/wolves_data/wolves_dataset.csv")
```

```
## Rows: 1986 Columns: 23
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr  (4): pop, age.cat, sex, color
## dbl (19): year, lat, long, habitat, human, pop.density, pack.size, standard....
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
summary(wolves_dt)
```

```
##      pop                 year        age.cat              sex           
##  Length:1986        Min.   :1992   Length:1986        Length:1986       
##  Class :character   1st Qu.:2006   Class :character   Class :character  
##  Mode  :character   Median :2011   Mode  :character   Mode  :character  
##                     Mean   :2010                                        
##                     3rd Qu.:2016                                        
##                     Max.   :2019                                        
##                                                                         
##     color                lat             long            habitat       
##  Length:1986        Min.   :33.89   Min.   :-157.84   Min.   :  254.1  
##  Class :character   1st Qu.:44.60   1st Qu.:-123.73   1st Qu.:10375.2  
##  Mode  :character   Median :46.83   Median :-110.99   Median :11211.3  
##                     Mean   :50.43   Mean   :-116.86   Mean   :12797.4  
##                     3rd Qu.:57.89   3rd Qu.:-110.55   3rd Qu.:11860.8  
##                     Max.   :80.50   Max.   : -82.42   Max.   :34676.6  
##                                                                        
##      human          pop.density      pack.size    standard.habitat  
##  Min.   :   0.02   Min.   : 3.74   Min.   :3.55   Min.   :-1.63390  
##  1st Qu.:  80.60   1st Qu.: 7.40   1st Qu.:5.62   1st Qu.:-0.30620  
##  Median :2787.67   Median :11.63   Median :6.37   Median :-0.19650  
##  Mean   :2335.38   Mean   :14.91   Mean   :6.47   Mean   : 0.01158  
##  3rd Qu.:3973.47   3rd Qu.:25.32   3rd Qu.:8.25   3rd Qu.:-0.11130  
##  Max.   :6228.64   Max.   :33.96   Max.   :9.56   Max.   : 2.88180  
##                                                                     
##  standard.human     standard.pop      standard.packsize standard.latitude  
##  Min.   :-0.9834   Min.   :-1.13460   Min.   :-1.7585   Min.   :-1.805900  
##  1st Qu.:-0.9444   1st Qu.:-0.74630   1st Qu.:-0.5418   1st Qu.:-0.636900  
##  Median : 0.3648   Median :-0.29760   Median :-0.1009   Median :-0.392600  
##  Mean   : 0.1461   Mean   : 0.05084   Mean   :-0.0422   Mean   :-0.000006  
##  3rd Qu.: 0.9383   3rd Qu.: 1.15480   3rd Qu.: 1.0041   3rd Qu.: 0.814300  
##  Max.   : 2.0290   Max.   : 2.07150   Max.   : 1.7742   Max.   : 3.281900  
##                                                                            
##  standard.longitude    cav.binary       cdv.binary       cpv.binary    
##  Min.   :-2.144100   Min.   :0.0000   Min.   :0.0000   Min.   :0.0000  
##  1st Qu.:-0.359500   1st Qu.:1.0000   1st Qu.:0.0000   1st Qu.:1.0000  
##  Median : 0.306900   Median :1.0000   Median :0.0000   Median :1.0000  
##  Mean   :-0.000005   Mean   :0.8529   Mean   :0.2219   Mean   :0.7943  
##  3rd Qu.: 0.330200   3rd Qu.:1.0000   3rd Qu.:0.0000   3rd Qu.:1.0000  
##  Max.   : 1.801500   Max.   :1.0000   Max.   :1.0000   Max.   :1.0000  
##                      NA's   :321      NA's   :21       NA's   :7       
##    chv.binary       neo.binary      toxo.binary    
##  Min.   :0.0000   Min.   :0.0000   Min.   :0.0000  
##  1st Qu.:1.0000   1st Qu.:0.0000   1st Qu.:0.0000  
##  Median :1.0000   Median :0.0000   Median :0.0000  
##  Mean   :0.8018   Mean   :0.2804   Mean   :0.4832  
##  3rd Qu.:1.0000   3rd Qu.:1.0000   3rd Qu.:1.0000  
##  Max.   :1.0000   Max.   :1.0000   Max.   :1.0000  
##  NA's   :548      NA's   :538      NA's   :827
```

6. How many distinct wolf populations are included in this study? Mae a new object that restricts the data to the wolf populations in the lower 48 US states.

```r
count(wolves_dt,pop)%>%
  arrange(n)
```

```
## # A tibble: 17 x 2
##    pop         n
##    <chr>   <int>
##  1 SE.AK      10
##  2 ELLES      11
##  3 SS.NWT     34
##  4 INT.AK     35
##  5 GTNP       60
##  6 ONT        60
##  7 N.NWT      67
##  8 SNF        92
##  9 BAN.JAS    96
## 10 AK.PEN    100
## 11 MI        102
## 12 YUCH      105
## 13 BC        145
## 14 DENALI    154
## 15 MEXICAN   181
## 16 MT        351
## 17 YNP       383
```

```r
wolves_lowpop<-wolves_dt%>%
  filter(pop=="SE,AK"|pop=="ELLES"|pop=="SS.NWT"|pop=="INT.AK"|pop=="GTNP")
```


7. Use the range of the latitude and longitude to build an appropriate bounding box for your map.

```r
summary(wolves_lowpop)
```

```
##      pop                 year        age.cat              sex           
##  Length:140         Min.   :2012   Length:140         Length:140        
##  Class :character   1st Qu.:2015   Class :character   Class :character  
##  Mode  :character   Median :2017   Mode  :character   Mode  :character  
##                     Mean   :2016                                        
##                     3rd Qu.:2018                                        
##                     Max.   :2019                                        
##                                                                         
##     color                lat             long            habitat       
##  Length:140         Min.   :43.82   Min.   :-147.75   Min.   :  261.3  
##  Class :character   1st Qu.:43.82   1st Qu.:-122.94   1st Qu.:10184.4  
##  Mode  :character   Median :60.79   Median :-110.71   Median :10375.2  
##                     Mean   :56.12   Mean   :-118.71   Mean   :11640.1  
##                     3rd Qu.:65.02   3rd Qu.:-110.71   3rd Qu.:10375.2  
##                     Max.   :80.50   Max.   : -82.42   Max.   :19052.1  
##                                                                        
##      human          pop.density      pack.size     standard.habitat 
##  Min.   :   0.02   Min.   : 3.74   Min.   :3.550   Min.   :-1.6330  
##  1st Qu.: 214.86   1st Qu.: 7.10   1st Qu.:6.240   1st Qu.:-0.3312  
##  Median : 908.61   Median :16.20   Median :8.100   Median :-0.3062  
##  Mean   :1961.09   Mean   :20.07   Mean   :6.616   Mean   :-0.1403  
##  3rd Qu.:3924.09   3rd Qu.:33.96   3rd Qu.:8.100   3rd Qu.:-0.3062  
##  Max.   :3924.09   Max.   :33.96   Max.   :9.190   Max.   : 0.8321  
##                                                                     
##  standard.human      standard.pop     standard.packsize  standard.latitude
##  Min.   :-0.98340   Min.   :-1.1346   Min.   :-1.75850   Min.   :-0.7219  
##  1st Qu.:-0.87950   1st Qu.:-0.7782   1st Qu.:-0.17740   1st Qu.:-0.7219  
##  Median :-0.54400   Median : 0.1873   Median : 0.91600   Median : 1.1306  
##  Mean   :-0.03497   Mean   : 0.5979   Mean   : 0.04347   Mean   : 0.6211  
##  3rd Qu.: 0.91440   3rd Qu.: 2.0715   3rd Qu.: 0.91600   3rd Qu.: 1.5923  
##  Max.   : 0.91440   Max.   : 2.0715   Max.   : 1.55670   Max.   : 3.2819  
##                                                                           
##  standard.longitude   cav.binary       cdv.binary       cpv.binary    
##  Min.   :-1.61610   Min.   :0.0000   Min.   :0.0000   Min.   :0.0000  
##  1st Qu.:-0.31822   1st Qu.:1.0000   1st Qu.:0.0000   1st Qu.:1.0000  
##  Median : 0.32190   Median :1.0000   Median :0.0000   Median :1.0000  
##  Mean   :-0.09674   Mean   :0.8849   Mean   :0.3406   Mean   :0.8551  
##  3rd Qu.: 0.32190   3rd Qu.:1.0000   3rd Qu.:1.0000   3rd Qu.:1.0000  
##  Max.   : 1.80150   Max.   :1.0000   Max.   :1.0000   Max.   :1.0000  
##                     NA's   :1        NA's   :2        NA's   :2       
##    chv.binary       neo.binary   toxo.binary 
##  Min.   :0.0000   Min.   :0.0   Min.   :0.0  
##  1st Qu.:1.0000   1st Qu.:0.0   1st Qu.:0.0  
##  Median :1.0000   Median :0.0   Median :1.0  
##  Mean   :0.8489   Mean   :0.3   Mean   :0.6  
##  3rd Qu.:1.0000   3rd Qu.:1.0   3rd Qu.:1.0  
##  Max.   :1.0000   Max.   :1.0   Max.   :1.0  
##  NA's   :1
```

```r
lat<-c(43.82,80.50)
long<-c(-147.75,-82.42)
bbox<-make_bbox(long,lat,f=0.05)
```

8.  Load a map from `stamen` in a `terrain-lines` projection and display the map.

```r
map_wol<-get_map(bbox,maptype = "terrain",source="stamen")
```

```
## Map tiles by Stamen Design, under CC BY 3.0. Data by OpenStreetMap, under ODbL.
```


```r
ggmap(map_wol)
```

![](lab12_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

9. Build a final map that overlays the recorded observations of wolves in the lower 48 states.

```r
ggmap(map_wol)+
  geom_point(wolves_lowpop,mapping=aes(long,lat))+
  labs(x="Longitude",y="Latitude",title="Obsevations of the wolves with lowest 5 population is the US")
```

![](lab12_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->


10. Use the map from #9 above, but add some aesthetics. Try to `fill` and `color` by population.

```r
ggmap(map_wol)+
  geom_point(data=wolves_lowpop,aes(long,lat,fill=pop,color=pop))+
  labs(x="Longitude",y="Latitude",title="Obsevations of the wolves with lowest 5 population in the US")+
  theme(plot.title = element_text(size = rel(1.2),hjust = 0.5))
```

![](lab12_hw_files/figure-html/unnamed-chunk-17-1.png)<!-- -->



## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
