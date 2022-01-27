---
title: "Midterm 1"
author: "Please Add Your Name Here"
date: "2022-01-27"
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
## v readr   2.1.1     v forcats 0.5.1
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
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (1): Sex
## dbl (2): Age, Height
```

```
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
elephants<-data.frame(age=elephants$Age,height=elephants$Height,sex=elephants$Sex)
elephants$sex<-as.factor(elephants$sex)
elephants
```

```
##       age height sex
## 1    1.40 120.00   M
## 2   17.50 227.00   M
## 3   12.75 235.00   M
## 4   11.17 210.00   M
## 5   12.67 220.00   M
## 6   12.67 189.00   M
## 7   12.25 225.00   M
## 8   12.17 204.00   M
## 9   28.17 265.85   M
## 10  11.67 233.00   M
## 11  16.33 241.00   M
## 12  12.58 212.00   M
## 13  25.42 251.51   M
## 14  12.42 201.00   M
## 15  26.50 257.70   M
## 16  12.58 249.00   M
## 17  11.58 206.00   M
## 18  12.25 212.00   M
## 19  11.58 238.00   M
## 20  11.58 217.00   M
## 21  26.67 268.56   M
## 22  25.67 237.19   M
## 23  15.25 257.00   M
## 24  28.75 302.35   M
## 25  11.33 201.00   M
## 26  12.08 238.00   M
## 27  12.50 200.00   M
## 28  24.17 253.05   M
## 29  11.75 202.00   M
## 30   9.75 224.00   M
## 31   9.25 206.00   M
## 32   9.00 180.00   M
## 33   9.50 210.00   M
## 34   9.50 208.00   M
## 35  23.92 281.56   M
## 36   9.08 195.00   M
## 37  26.08 304.06   M
## 38   8.83 212.00   M
## 39   8.17 193.00   M
## 40  18.83 251.39   M
## 41  25.42 294.15   M
## 42  18.75 219.29   M
## 43   8.25 181.00   M
## 44   8.33 188.00   M
## 45  24.17 287.13   M
## 46   8.17 193.00   M
## 47  21.25 272.91   M
## 48  18.67 271.31   M
## 49   8.33 199.00   M
## 50  21.33 228.69   M
## 51   8.50 194.00   M
## 52  18.75 258.11   M
## 53   8.42 162.00   M
## 54   6.83 161.00   M
## 55  17.25 296.84   M
## 56   7.17 199.00   M
## 57  18.75 221.37   M
## 58   7.42 181.00   M
## 59   7.00 183.00   M
## 60   0.75  84.00   M
## 61   4.67 170.00   M
## 62   4.67 167.00   M
## 63   0.67  99.00   M
## 64   4.67 155.00   M
## 65   0.67 115.00   M
## 66  11.08 206.64   M
## 67   0.01  91.00   M
## 68  10.25 200.03   M
## 69   0.17  86.00   M
## 70   0.25  85.00   M
## 71  17.25 266.75   M
## 72  10.75 227.57   M
## 73   0.17  96.00   M
## 74   3.67 163.00   M
## 75  10.58 233.09   M
## 76  10.00 209.92   M
## 77   9.50 212.96   M
## 78  12.17 217.57   M
## 79  11.50 234.92   M
## 80   9.83 174.97   M
## 81   7.33 151.31   M
## 82   5.75 171.58   M
## 83  11.33 225.93   M
## 84   7.67 205.00   M
## 85  10.08 232.77   M
## 86   4.83 167.97   M
## 87   7.25 188.25   M
## 88   4.75 176.93   M
## 89   6.75 205.48   M
## 90   9.42 218.38   M
## 91   4.58 136.39   M
## 92   9.33 199.73   M
## 93   7.42 199.87   M
## 94   9.50 178.00   M
## 95   9.50 227.49   M
## 96   8.75 204.35   M
## 97   7.42 205.53   M
## 98   8.50 210.27   M
## 99   6.33 165.85   M
## 100  8.58 223.16   M
## 101  7.33 171.07   M
## 102  1.58 119.15   M
## 103  7.08 162.57   M
## 104  6.58 165.88   M
## 105  3.83 152.26   M
## 106  5.92 171.59   M
## 107  6.00 164.57   M
## 108  5.67 197.51   M
## 109  0.08 108.08   M
## 110  3.00 136.40   M
## 111  0.25  85.18   M
## 112 11.08 217.87   F
## 113  3.08 176.84   M
## 114  5.25 163.99   M
## 115  2.33 132.89   F
## 116  2.42 144.83   F
## 117 26.50 206.07   F
## 118 11.75 207.00   F
## 119  0.33  85.59   F
## 120  2.42 126.96   M
## 121 26.75 227.34   F
## 122  1.25 114.05   M
## 123 13.42 203.00   F
## 124 28.42 228.32   F
## 125  2.08 131.28   F
## 126  7.50 182.52   F
## 127  5.50 150.57   F
## 128 27.25 226.22   F
## 129  6.58 178.61   F
## 130  0.67 103.95   M
## 131  0.01  83.00   F
## 132  0.33  84.07   M
## 133  3.33 156.12   F
## 134 11.17 217.05   F
## 135  0.17  96.22   M
## 136 14.58 214.30   F
## 137  0.58  99.00   M
## 138 12.42 214.00   F
## 139  8.42 178.00   F
## 140 16.50 217.00   F
## 141 25.42 240.41   F
## 142  1.50 132.75   F
## 143 23.33 238.10   F
## 144  1.83 115.21   M
## 145 26.75 235.43   F
## 146  0.08  84.21   F
## 147  6.75 166.17   F
## 148 11.42 213.35   F
## 149 16.50 222.19   F
## 150 17.67 215.02   F
## 151  1.30 125.00   F
## 152  8.33 200.00   F
## 153 27.80 265.00   F
## 154  1.33 137.00   F
## 155 28.25 215.88   F
## 156 26.08 215.97   F
## 157  0.50  97.31   F
## 158  8.75 207.71   F
## 159 26.67 220.51   F
## 160 20.17 232.00   F
## 161 21.50 227.09   F
## 162 19.58 223.83   F
## 163 27.33 225.58   F
## 164  8.67 193.40   F
## 165  2.42 160.79   F
## 166  0.10  82.00   M
## 167  8.25 175.00   F
## 168 17.50 200.08   F
## 169  0.33 122.39   M
## 170 25.25 218.02   F
## 171 15.50 217.58   F
## 172 21.25 227.21   F
## 173  5.17 181.50   F
## 174 16.50 228.47   F
## 175  0.67 105.28   F
## 176 11.25 205.04   F
## 177 12.25 225.00   F
## 178  2.33 144.13   M
## 179  7.33 173.72   F
## 180 15.25 220.31   F
## 181  2.25 106.30   F
## 182  7.33 162.36   F
## 183 30.25 277.80   F
## 184 27.42 207.02   F
## 185  3.25 155.58   F
## 186  3.83 157.95   F
## 187 29.25 226.10   F
## 188  3.33 160.63   M
## 189  7.67 205.20   F
## 190  0.25 102.00   F
## 191  9.67 197.00   F
## 192  8.25 187.00   F
## 193 26.42 210.30   F
## 194  6.75 167.78   F
## 195 12.33 217.00   F
## 196 16.42 221.00   F
## 197 11.33 196.00   F
## 198  0.67 117.73   F
## 199 16.75 232.03   F
## 200  2.08 143.76   F
## 201  8.42 168.00   F
## 202 15.75 232.00   F
## 203 21.42 249.42   F
## 204  1.58 146.79   F
## 205 24.67 192.54   F
## 206  4.58 131.34   F
## 207 18.75 216.34   F
## 208 10.92 194.76   F
## 209  8.67 174.97   F
## 210  1.08 140.24   F
## 211  0.75  99.00   F
## 212  8.25 204.00   F
## 213 15.42 214.00   F
## 214 17.58 190.33   F
## 215 21.92 226.21   F
## 216 22.17 210.30   F
## 217  9.75 200.21   F
## 218 10.08 215.28   F
## 219  0.50  75.46   M
## 220 21.67 232.25   F
## 221  0.25  90.00   F
## 222  9.42 205.00   F
## 223 11.42 203.00   F
## 224  1.42 119.13   F
## 225  1.50 117.78   F
## 226  0.50 116.93   M
## 227  9.33 229.47   F
## 228 17.50 234.74   F
## 229 25.25 241.94   F
## 230 28.58 234.84   F
## 231  0.33  84.27   M
## 232 16.67 220.43   F
## 233  1.50 114.91   F
## 234 11.50 190.03   F
## 235 25.83 243.74   F
## 236  9.33 193.90   F
## 237  1.50 104.83   F
## 238 23.92 253.36   F
## 239  0.25 110.37   M
## 240  0.20  94.00   M
## 241  0.10  91.00   F
## 242 21.83 258.94   F
## 243 18.58 215.21   F
## 244  1.83 150.22   M
## 245 12.83 199.00   F
## 246 18.75 216.91   F
## 247  7.42 187.58   F
## 248  1.58 145.35   M
## 249 12.50 178.00   F
## 250 10.08 208.45   F
## 251  3.33 169.94   M
## 252  8.75 207.52   F
## 253  8.42 176.00   F
## 254 12.25 214.00   F
## 255  0.50  94.00   F
## 256 16.75 198.45   F
## 257 10.67 198.45   F
## 258  3.92 163.00   F
## 259 30.50 227.03   F
## 260  2.08 132.36   M
## 261  0.01  79.00   F
## 262  0.33 105.00   M
## 263 11.33 215.00   F
## 264  8.17 186.00   F
## 265  2.20 140.00   M
## 266 15.33 213.00   F
## 267 11.42 208.00   F
## 268  8.08 169.00   F
## 269  9.58 192.00   F
## 270  8.25 175.00   F
## 271 26.58 250.79   F
## 272  0.42 117.00   M
## 273  0.04  91.50   M
## 274 15.58 206.08   F
## 275 32.17 261.02   F
## 276 28.00 232.31   F
## 277  0.08  75.57   F
## 278  5.42 173.32   M
## 279 25.58 221.61   F
## 280  5.92 162.08   F
## 281  7.58 193.27   F
## 282 10.00 204.77   F
## 283 25.08 259.25   F
## 284  4.75 122.83   F
## 285 10.50 209.23   F
## 286 19.92 190.25   F
## 287 21.25 225.53   F
## 288 13.17 221.00   F
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

8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.  

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.

10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?

11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.  

12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`
