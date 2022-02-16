---
title: "Lab 10 Homework"
author: "SOnghee Kim"
date: "2022-02-16"
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
library(naniar)
```

## Desert Ecology
For this assignment, we are going to use a modified data set on [desert ecology](http://esapubs.org/archive/ecol/E090/118/). The data are from: S. K. Morgan Ernest, Thomas J. Valone, and James H. Brown. 2009. Long-term monitoring and experimental manipulation of a Chihuahuan Desert ecosystem near Portal, Arizona, USA. Ecology 90:1708.

```r
deserts <- read_csv(here("lab10", "data", "surveys_complete.csv"))
```

```
## Rows: 34786 Columns: 13
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (6): species_id, sex, genus, species, taxa, plot_type
## dbl (7): record_id, month, day, year, plot_id, hindfoot_length, weight
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Use the function(s) of your choice to get an idea of its structure, including how NA's are treated. Are the data tidy?  

```r
summary(deserts)
```

```
##    record_id         month             day            year         plot_id     
##  Min.   :    1   Min.   : 1.000   Min.   : 1.0   Min.   :1977   Min.   : 1.00  
##  1st Qu.: 8964   1st Qu.: 4.000   1st Qu.: 9.0   1st Qu.:1984   1st Qu.: 5.00  
##  Median :17762   Median : 6.000   Median :16.0   Median :1990   Median :11.00  
##  Mean   :17804   Mean   : 6.474   Mean   :16.1   Mean   :1990   Mean   :11.34  
##  3rd Qu.:26655   3rd Qu.:10.000   3rd Qu.:23.0   3rd Qu.:1997   3rd Qu.:17.00  
##  Max.   :35548   Max.   :12.000   Max.   :31.0   Max.   :2002   Max.   :24.00  
##                                                                                
##   species_id            sex            hindfoot_length     weight      
##  Length:34786       Length:34786       Min.   : 2.00   Min.   :  4.00  
##  Class :character   Class :character   1st Qu.:21.00   1st Qu.: 20.00  
##  Mode  :character   Mode  :character   Median :32.00   Median : 37.00  
##                                        Mean   :29.29   Mean   : 42.67  
##                                        3rd Qu.:36.00   3rd Qu.: 48.00  
##                                        Max.   :70.00   Max.   :280.00  
##                                        NA's   :3348    NA's   :2503    
##     genus             species              taxa            plot_type        
##  Length:34786       Length:34786       Length:34786       Length:34786      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
## 
```

2. How many genera and species are represented in the data? What are the total number of observations? Which species is most/ least frequently sampled in the study?

```r
deserts%>%
  group_by(genus,species)%>%
  summarize(n=n(),.groups="keep")%>%
  arrange(desc(n))
```

```
## # A tibble: 48 x 3
## # Groups:   genus, species [48]
##    genus           species          n
##    <chr>           <chr>        <int>
##  1 Dipodomys       merriami     10596
##  2 Chaetodipus     penicillatus  3123
##  3 Dipodomys       ordii         3027
##  4 Chaetodipus     baileyi       2891
##  5 Reithrodontomys megalotis     2609
##  6 Dipodomys       spectabilis   2504
##  7 Onychomys       torridus      2249
##  8 Perognathus     flavus        1597
##  9 Peromyscus      eremicus      1299
## 10 Neotoma         albigula      1252
## # ... with 38 more rows
```


```r
deserts%>%
  count(genus,sort=T)
```

```
## # A tibble: 26 x 2
##    genus                n
##    <chr>            <int>
##  1 Dipodomys        16167
##  2 Chaetodipus       6029
##  3 Onychomys         3267
##  4 Reithrodontomys   2694
##  5 Peromyscus        2234
##  6 Perognathus       1629
##  7 Neotoma           1252
##  8 Ammospermophilus   437
##  9 Amphispiza         303
## 10 Spermophilus       249
## # ... with 16 more rows
```



```r
deserts%>%
  count(species,sort=T)
```

```
## # A tibble: 40 x 2
##    species          n
##    <chr>        <int>
##  1 merriami     10596
##  2 penicillatus  3123
##  3 ordii         3027
##  4 baileyi       2891
##  5 megalotis     2609
##  6 spectabilis   2504
##  7 torridus      2249
##  8 flavus        1597
##  9 eremicus      1299
## 10 albigula      1252
## # ... with 30 more rows
```


3. What is the proportion of taxa included in this study? Show a table and plot that reflects this count.

```r
deserts%>%
  tabyl(taxa)
```

```
##     taxa     n      percent
##     Bird   450 0.0129362387
##   Rabbit    75 0.0021560398
##  Reptile    14 0.0004024608
##   Rodent 34247 0.9845052607
```

```r
deserts%>%
  ggplot(aes(x=taxa))+
  scale_y_log10()+
  geom_bar()
```

![](lab10_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->


4. For the taxa included in the study, use the fill option to show the proportion of individuals sampled by `plot_type.`


```r
deserts%>%
  ggplot(aes(x=taxa,fill=plot_type))+
  scale_y_log10()+
  geom_bar(position = "dodge")+
  labs(title = "Taxa_polt type",
       x="Taxon",
       y="Count(log10)")+
  theme(plot.title=element_text(size = rel(1.7),hjust = 0.5 ))+
  coord_flip()
```

![](lab10_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


```r
deserts%>%
  ggplot(aes(x=taxa,fill=plot_type))+
  geom_bar(position = position_fill())+
  scale_y_continuous(labels = scales::percent)+
  scale_y_log10()+
  labs(title = "Taxon_Plot type",
       x="Taxon",
       y="Proportion")+
  theme(plot.title = element_text(size = rel(1.5), hjust = 0.5))+
  coord_flip()
```

```
## Scale for 'y' is already present. Adding another scale for 'y', which will
## replace the existing scale.
```

![](lab10_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


5. What is the range of weight for each species included in the study? Remove any observations of weight that are NA so they do not show up in the plot.

```r
deserts%>%
  filter(!is.na(weight))%>%
  ggplot(aes(x=species, y=weight))+
  geom_boxplot()+
  scale_y_log10()+
  labs(title = "Species_weight",
       x="Species",
       y="Weight")+
  theme(axis.text.x = element_text(angle=60,hjust=1),
        plot.title = element_text(size = rel(1.6), hjust = 0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


```r
deserts%>%
  filter(!is.na(weight))%>%
  ggplot(aes(x=species, y=weight))+
  geom_boxplot()+
  scale_y_log10()+
  labs(title = "Species_weight",
       x="Species",
       y="Weight")+
  theme(plot.title = element_text(size = rel(1.6), hjust = 0.5))+
  coord_flip()
```

![](lab10_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


6. Add another layer to your answer from #4 using `geom_point` to get an idea of how many measurements were taken for each species.


```r
deserts%>%
  filter(!is.na(weight))%>%
  ggplot(aes(x=species, y=weight))+
  geom_boxplot()+
  geom_point(alpha=0.1,color="red")+
  scale_y_log10()+
  labs(title = "Species_weight",
       x="Species",
       y="Weight")+
  theme(axis.text.x = element_text(angle=60,hjust=1),
        plot.title = element_text(size = rel(1.6), hjust = 0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


7. [Dipodomys merriami](https://en.wikipedia.org/wiki/Merriam's_kangaroo_rat) is the most frequently sampled animal in the study. How have the number of observations of this species changed over the years included in the study?

```r
id_dt<-deserts%>%
  filter(species_id=="DM")%>%
  group_by(species_id,year)%>%
  summarise(n=n())
```

```
## `summarise()` has grouped output by 'species_id'. You can override using the
## `.groups` argument.
```

```r
id_dt
```

```
## # A tibble: 26 x 3
## # Groups:   species_id [1]
##    species_id  year     n
##    <chr>      <dbl> <int>
##  1 DM          1977   264
##  2 DM          1978   389
##  3 DM          1979   209
##  4 DM          1980   493
##  5 DM          1981   559
##  6 DM          1982   609
##  7 DM          1983   528
##  8 DM          1984   396
##  9 DM          1985   667
## 10 DM          1986   406
## # ... with 16 more rows
```


```r
id_dt%>%
  ggplot(aes(x=year,y=n))+
  geom_col()+
  labs(title = "The number of species_year",
       x="Year",
       y="Count species")
```

![](lab10_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->


8. What is the relationship between `weight` and `hindfoot` length? Consider whether or not over plotting is an issue.

```r
deserts%>%
  ggplot(aes(x=weight,y=hindfoot_length,color=genus))+
  geom_jitter(alpha=0.3,na.rm=T)
```

![](lab10_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->


9. Which two species have, on average, the highest weight? Once you have identified them, make a new column that is a ratio of `weight` to `hindfoot_length`. Make a plot that shows the range of this new ratio and fill by sex.

```r
deserts%>%
  group_by(species)%>%
  filter(!is.na(weight))%>%
  summarise(weight_mean=mean(weight))%>%
  arrange(desc(weight_mean))
```

```
## # A tibble: 22 x 2
##    species      weight_mean
##    <chr>              <dbl>
##  1 albigula           159. 
##  2 spectabilis        120. 
##  3 spilosoma           93.5
##  4 hispidus            65.6
##  5 fulviventer         58.9
##  6 ochrognathus        55.4
##  7 ordii               48.9
##  8 merriami            43.2
##  9 baileyi             31.7
## 10 leucogaster         31.6
## # ... with 12 more rows
```


```r
deserts%>%
  filter(species=="albigula"|species=="spectabilis")%>%
  filter(!is.na(weight)&!is.na(hindfoot_length)&!is.na(sex))%>%
  mutate(ratio=weight/hindfoot_length)%>%
  ggplot(aes(x=species,y=ratio,fill=sex))+
  geom_boxplot(na.rm=T)+
  labs(title = "Species & Weight/Hindfoot lengh Ratio",
       x="Species",
       y="Ratio(weight/hindfood length")+
  theme(plot.title = element_text(size = rel(1.5),hjust = 0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-18-1.png)<!-- -->


10. Make one plot of your choice! Make sure to include at least two of the aesthetics options you have learned.

```r
deserts%>%
  filter(!is.na(weight))%>%
  group_by(species_id,year)%>%
  summarise(mean_w=mean(weight))%>%
  ggplot(aes(x=year,y=mean_w,color=species_id))+
  geom_jitter()
```

```
## `summarise()` has grouped output by 'species_id'. You can override using the
## `.groups` argument.
```

![](lab10_hw_files/figure-html/unnamed-chunk-19-1.png)<!-- -->


```r
deserts
```

```
## # A tibble: 34,786 x 13
##    record_id month   day  year plot_id species_id sex   hindfoot_length weight
##        <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <chr>           <dbl>  <dbl>
##  1         1     7    16  1977       2 NL         M                  32     NA
##  2         2     7    16  1977       3 NL         M                  33     NA
##  3         3     7    16  1977       2 DM         F                  37     NA
##  4         4     7    16  1977       7 DM         M                  36     NA
##  5         5     7    16  1977       3 DM         M                  35     NA
##  6         6     7    16  1977       1 PF         M                  14     NA
##  7         7     7    16  1977       2 PE         F                  NA     NA
##  8         8     7    16  1977       1 DM         M                  37     NA
##  9         9     7    16  1977       1 DM         F                  34     NA
## 10        10     7    16  1977       6 PF         F                  20     NA
## # ... with 34,776 more rows, and 4 more variables: genus <chr>, species <chr>,
## #   taxa <chr>, plot_type <chr>
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
