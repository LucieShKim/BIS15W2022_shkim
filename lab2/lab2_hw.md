---
title: "Lab 2 Homework"
author: "Songhee Kim"
date: "2022-03-14"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

1. What is a vector in R? 
One-dimensional arrangement,It has the same data format.Can be created using c.

2. What is a data matrix in R?  
Two-dimensional arrangement, It has the same data format. 

3. Below are data collected by three scientists (Jill, Steve, Susan in order) measuring temperatures of eight hot springs. Run this code chunk to create the vectors.  

```r
spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)
```

4. Build a data matrix that has the springs as rows and the columns as scientists.  


```r
temp_spring<-c(spring_1,spring_2,spring_3,spring_4,spring_5,spring_6,spring_7,spring_8)
temp_spring
```

```
##  [1] 36.25 35.40 35.30 35.15 35.35 33.35 30.70 29.65 29.20 39.70 40.05 38.65
## [13] 31.85 31.40 29.30 30.20 30.65 29.75 32.90 32.50 32.80 36.80 36.45 33.15
```

```r
spring_mat<-matrix(temp_spring,nrow=8,byrow=T)
spring_mat
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```

5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.


```r
scientist_col<-c("Jill","Steve","Susan")
```


```r
scientist_col
```

```
## [1] "Jill"  "Steve" "Susan"
```



```r
colnames(spring_mat)<-scientist_col
```


```r
spring_name<-c("Bluebell_Spring", "Opal_Spring", "Riverside_Spring", "Too_Hot_Spring", "Mystery_Spring", "Emerald_Spring", "Black_Spring", "Pearl_Spring")
```


```r
rownames(spring_mat)<-spring_name
```


```r
spring_mat
```

```
##                   Jill Steve Susan
## Bluebell_Spring  36.25 35.40 35.30
## Opal_Spring      35.15 35.35 33.35
## Riverside_Spring 30.70 29.65 29.20
## Too_Hot_Spring   39.70 40.05 38.65
## Mystery_Spring   31.85 31.40 29.30
## Emerald_Spring   30.20 30.65 29.75
## Black_Spring     32.90 32.50 32.80
## Pearl_Spring     36.80 36.45 33.15
```

6. Calculate the mean temperature of all eight springs.

```r
rsum<-rowSums(spring_mat)
rsum
```

```
##  Bluebell_Spring      Opal_Spring Riverside_Spring   Too_Hot_Spring 
##           106.95           103.85            89.55           118.40 
##   Mystery_Spring   Emerald_Spring     Black_Spring     Pearl_Spring 
##            92.55            90.60            98.20           106.40
```


```r
r_mean<-rowMeans(spring_mat)
r_mean
```

```
##  Bluebell_Spring      Opal_Spring Riverside_Spring   Too_Hot_Spring 
##         35.65000         34.61667         29.85000         39.46667 
##   Mystery_Spring   Emerald_Spring     Black_Spring     Pearl_Spring 
##         30.85000         30.20000         32.73333         35.46667
```

7. Add this as a new column in the data matrix.  


```r
new_spmat<-cbind(spring_mat,r_mean)
new_spmat
```

```
##                   Jill Steve Susan   r_mean
## Bluebell_Spring  36.25 35.40 35.30 35.65000
## Opal_Spring      35.15 35.35 33.35 34.61667
## Riverside_Spring 30.70 29.65 29.20 29.85000
## Too_Hot_Spring   39.70 40.05 38.65 39.46667
## Mystery_Spring   31.85 31.40 29.30 30.85000
## Emerald_Spring   30.20 30.65 29.75 30.20000
## Black_Spring     32.90 32.50 32.80 32.73333
## Pearl_Spring     36.80 36.45 33.15 35.46667
```

8. Show Susan's value for Opal Spring only.

```r
new_spmat[2,3]
```

```
## [1] 33.35
```

9. Calculate the mean for Jill's column only.  

```r
J_col<-new_spmat[,1]
mean(J_col)
```

```
## [1] 34.19375
```

10. Use the data matrix to perform one calculation or operation of your interest.

```r
number_sh<-c(1,2,3,4,5,6,7,8,9)
```


```r
mat_sh<-matrix(number_sh,nrow=3,byrow=FALSE)
mat_sh
```

```
##      [,1] [,2] [,3]
## [1,]    1    4    7
## [2,]    2    5    8
## [3,]    3    6    9
```

```r
col_sh<-c("first","second","Third")
row_sh<-c("one","two","three")
```


```r
colnames(mat_sh)<-col_sh
rownames(mat_sh)<-row_sh
mat_sh
```

```
##       first second Third
## one       1      4     7
## two       2      5     8
## three     3      6     9
```

```r
sum_sh<-rowSums(mat_sh)
```


```r
cbind(mat_sh,sum_sh)
```

```
##       first second Third sum_sh
## one       1      4     7     12
## two       2      5     8     15
## three     3      6     9     18
```

```r
four<-c(10,11,12)
```


```r
rbind(mat_sh,four)
```

```
##       first second Third
## one       1      4     7
## two       2      5     8
## three     3      6     9
## four     10     11    12
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
