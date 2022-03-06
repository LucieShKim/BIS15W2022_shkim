---
title: "Lab 13 Homework"
author: "Songhee Kim"
date: "2022-03-05"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Libraries

```r
library(tidyverse)
library(shiny)
library(shinydashboard)
require(janitor)
```

## Choose Your Adventure!
For this homework assignment, you have two choices of data. You only need to build an app for one of them. The first dataset is focused on UC Admissions and the second build on the Gabon data that we used for midterm 1.  

## Option 1
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

**1. Load the `UC_admit.csv` data and use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  

```r
uc_admit<-readr::read_csv("data/uc_data/UC_admit.csv")
```

```
## Rows: 2160 Columns: 6
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (4): Campus, Category, Ethnicity, Perc FR
## dbl (2): Academic_Yr, FilteredCountFR
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
uc_admit
```

```
## # A tibble: 2,160 x 6
##    Campus Academic_Yr Category   Ethnicity        `Perc FR` FilteredCountFR
##    <chr>        <dbl> <chr>      <chr>            <chr>               <dbl>
##  1 Davis         2019 Applicants International    21.16%              16522
##  2 Davis         2019 Applicants Unknown          2.51%                1959
##  3 Davis         2019 Applicants White            18.39%              14360
##  4 Davis         2019 Applicants Asian            30.76%              24024
##  5 Davis         2019 Applicants Chicano/Latino   22.44%              17526
##  6 Davis         2019 Applicants American Indian  0.35%                 277
##  7 Davis         2019 Applicants African American 4.39%                3425
##  8 Davis         2019 Applicants All              100.00%             78093
##  9 Davis         2018 Applicants International    19.87%              15507
## 10 Davis         2018 Applicants Unknown          2.83%                2208
## # ... with 2,150 more rows
```


**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**




```r
uc_admission<-clean_names(uc_admit)
```


```r
table(uc_admission$academic_yr)
```

```
## 
## 2010 2011 2012 2013 2014 2015 2016 2017 2018 2019 
##  216  216  216  216  216  216  216  216  216  216
```


```r
ui <- dashboardPage(
  dashboardHeader(title = "Admissions by ethnicity across all UC campuses"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 3,
  selectInput("x", "Select Year", choices = unique(uc_admission$academic_yr), 
              selected = "Admits"),
  selectInput("y", "Select Category", choices = c("Admits", "Applicants", "Enrollees"), 
              selected = "2010"),
  selectInput("z", "Select Campus", choices = unique(uc_admission$campus), 
              selected = "Davis"),
  ), # close the first box
  
  box(title = "UC admission", width = 7,
  plotOutput("plot", width = "600px", height = "500px")
  ) # close the second box
  ) # close the row
  ) # close the dashboard body
) # close the ui

server <- function(input, output, session) { 
  session$onSessionEnded(stopApp)
  output$plot <- renderPlot({
  uc_admission%>%
      filter(academic_yr==input$x & category==input$y & campus==input$z) %>%
      ggplot(aes(x=ethnicity, y=filtered_count_fr, fill=ethnicity)) +
      geom_col() + 
      theme_light()+
      theme(axis.text.x = element_text(angle = 60, hjust = 1))
    })
}

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


```r
table(uc_admission$ethnicity)
```

```
## 
## African American              All  American Indian            Asian 
##              270              270              270              270 
##   Chicano/Latino    International          Unknown            White 
##              270              270              270              270
```

**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**  

```r
uc_admission$academic_yr <- as.factor(uc_admission$academic_yr)
```


```r
ui <- dashboardPage(
  dashboardHeader(title = "UC campuses"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 3,
  selectInput("x", "Select Ethnicity", choices = unique(uc_admission$ethnicity), 
              selected = "African American"),
  selectInput("y", "Select Category", choices = c("Admits", "Applicants", "Enrollees"), 
              selected = "2010"),
  selectInput("z", "Select Campus", choices = unique(uc_admission$campus), 
              selected = "Davis"),
  ), # close the first box
  
  box(title = "Admission", width = 7,
  plotOutput("plot", width = "600px", height = "500px")
  ) # close the second box
  ) # close the row
  ) # close the dashboard body
) # close the ui

server <- function(input, output, session) { 
  session$onSessionEnded(stopApp)
  output$plot <- renderPlot({
  uc_admission%>%
      filter(ethnicity==input$x & category==input$y & campus==input$z) %>%
      ggplot(aes(x=academic_yr, y=filtered_count_fr)) +
      geom_col() + 
      theme_light()+
      theme(axis.text.x = element_text(angle = 60, hjust = 1))
    })
}

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}



## Option 2
We will use data from a study on vertebrate community composition and impacts from defaunation in Gabon, Africa. Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016.   

**1. Load the `IvindoData_DryadVersion.csv` data and use the function(s) of your choice to get an idea of the overall structure, including its dimensions, column names, variable classes, etc. As part of this, determine if NA's are present and how they are treated.**  


```r
vertebrate<-readr::read_csv("data/gabon_data/IvindoData_DryadVersion.csv")
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
vertebrate
```

```
## # A tibble: 24 x 26
##    TransectID Distance HuntCat  NumHouseholds LandUse Veg_Rich Veg_Stems
##         <dbl>    <dbl> <chr>            <dbl> <chr>      <dbl>     <dbl>
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
## # ... with 14 more rows, and 19 more variables: Veg_liana <dbl>, Veg_DBH <dbl>,
## #   Veg_Canopy <dbl>, Veg_Understory <dbl>, RA_Apes <dbl>, RA_Birds <dbl>,
## #   RA_Elephant <dbl>, RA_Monkeys <dbl>, RA_Rodent <dbl>, RA_Ungulate <dbl>,
## #   Rich_AllSpecies <dbl>, Evenness_AllSpecies <dbl>,
## #   Diversity_AllSpecies <dbl>, Rich_BirdSpecies <dbl>,
## #   Evenness_BirdSpecies <dbl>, Diversity_BirdSpecies <dbl>,
## #   Rich_MammalSpecies <dbl>, Evenness_MammalSpecies <dbl>, ...
```


```r
vertebrate<-clean_names(vertebrate)
```



```r
summary(vertebrate)
```

```
##   transect_id       distance        hunt_cat         num_households 
##  Min.   : 1.00   Min.   : 2.700   Length:24          Min.   :13.00  
##  1st Qu.: 5.75   1st Qu.: 5.668   Class :character   1st Qu.:24.75  
##  Median :14.50   Median : 9.720   Mode  :character   Median :29.00  
##  Mean   :13.50   Mean   :11.879                      Mean   :37.88  
##  3rd Qu.:20.25   3rd Qu.:17.683                      3rd Qu.:54.00  
##  Max.   :27.00   Max.   :26.760                      Max.   :73.00  
##    land_use            veg_rich       veg_stems       veg_liana     
##  Length:24          Min.   :10.88   Min.   :23.44   Min.   : 4.750  
##  Class :character   1st Qu.:13.10   1st Qu.:28.69   1st Qu.: 9.033  
##  Mode  :character   Median :14.94   Median :32.45   Median :11.940  
##                     Mean   :14.83   Mean   :32.80   Mean   :11.040  
##                     3rd Qu.:16.54   3rd Qu.:37.08   3rd Qu.:13.250  
##                     Max.   :18.75   Max.   :47.56   Max.   :16.380  
##     veg_dbh        veg_canopy    veg_understory     ra_apes      
##  Min.   :28.45   Min.   :2.500   Min.   :2.380   Min.   : 0.000  
##  1st Qu.:40.65   1st Qu.:3.250   1st Qu.:2.875   1st Qu.: 0.000  
##  Median :43.90   Median :3.430   Median :3.000   Median : 0.485  
##  Mean   :46.09   Mean   :3.469   Mean   :3.020   Mean   : 2.045  
##  3rd Qu.:50.58   3rd Qu.:3.750   3rd Qu.:3.167   3rd Qu.: 3.815  
##  Max.   :76.48   Max.   :4.000   Max.   :3.880   Max.   :12.930  
##     ra_birds      ra_elephant       ra_monkeys      ra_rodent    
##  Min.   :31.56   Min.   :0.0000   Min.   : 5.84   Min.   :1.060  
##  1st Qu.:52.51   1st Qu.:0.0000   1st Qu.:22.70   1st Qu.:2.047  
##  Median :57.90   Median :0.3600   Median :31.74   Median :3.230  
##  Mean   :58.64   Mean   :0.5450   Mean   :31.30   Mean   :3.278  
##  3rd Qu.:68.17   3rd Qu.:0.8925   3rd Qu.:39.88   3rd Qu.:4.093  
##  Max.   :85.03   Max.   :2.3000   Max.   :54.12   Max.   :6.310  
##   ra_ungulate     rich_all_species evenness_all_species diversity_all_species
##  Min.   : 0.000   Min.   :15.00    Min.   :0.6680       Min.   :1.966        
##  1st Qu.: 1.232   1st Qu.:19.00    1st Qu.:0.7542       1st Qu.:2.248        
##  Median : 2.545   Median :20.00    Median :0.7760       Median :2.316        
##  Mean   : 4.166   Mean   :20.21    Mean   :0.7699       Mean   :2.310        
##  3rd Qu.: 5.157   3rd Qu.:22.00    3rd Qu.:0.8083       3rd Qu.:2.429        
##  Max.   :13.860   Max.   :24.00    Max.   :0.8330       Max.   :2.566        
##  rich_bird_species evenness_bird_species diversity_bird_species
##  Min.   : 8.00     Min.   :0.5590        Min.   :1.162         
##  1st Qu.:10.00     1st Qu.:0.6825        1st Qu.:1.603         
##  Median :11.00     Median :0.7220        Median :1.680         
##  Mean   :10.33     Mean   :0.7137        Mean   :1.661         
##  3rd Qu.:11.00     3rd Qu.:0.7722        3rd Qu.:1.784         
##  Max.   :13.00     Max.   :0.8240        Max.   :2.008         
##  rich_mammal_species evenness_mammal_species diversity_mammal_species
##  Min.   : 6.000      Min.   :0.6190          Min.   :1.378           
##  1st Qu.: 9.000      1st Qu.:0.7073          1st Qu.:1.567           
##  Median :10.000      Median :0.7390          Median :1.699           
##  Mean   : 9.875      Mean   :0.7477          Mean   :1.698           
##  3rd Qu.:11.000      3rd Qu.:0.7847          3rd Qu.:1.815           
##  Max.   :12.000      Max.   :0.8610          Max.   :2.065
```


```r
names(vertebrate)
```

```
##  [1] "transect_id"              "distance"                
##  [3] "hunt_cat"                 "num_households"          
##  [5] "land_use"                 "veg_rich"                
##  [7] "veg_stems"                "veg_liana"               
##  [9] "veg_dbh"                  "veg_canopy"              
## [11] "veg_understory"           "ra_apes"                 
## [13] "ra_birds"                 "ra_elephant"             
## [15] "ra_monkeys"               "ra_rodent"               
## [17] "ra_ungulate"              "rich_all_species"        
## [19] "evenness_all_species"     "diversity_all_species"   
## [21] "rich_bird_species"        "evenness_bird_species"   
## [23] "diversity_bird_species"   "rich_mammal_species"     
## [25] "evenness_mammal_species"  "diversity_mammal_species"
```


**2. Build an app that re-creates the plots shown on page 810 of this paper. The paper is included in the folder. It compares the relative abundance % to the distance from villages in rural Gabon. Use shiny dashboard and add aesthetics to the plot.  **  


```r
ui <- dashboardPage(
  dashboardHeader(title = "Vertebrate"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 3,
  selectInput("x", "Select RA of Species", choices = c("ra_apes", "ra_birds", "ra_elephant", "ra_monkeys"), 
              selected = "ra_apes"),
  sliderInput("pointsize", "Select the Point Size", min = 1, max = 5, value = 2, step = 0.5)

  ), # close the first box
  
  box(title = "RA to the distance", width = 7,
  plotOutput("plot", width = "600px", height = "500px")
  ) # close the second box
  ) # close the row
  ) # close the dashboard body
) # close the ui

server <- function(input, output, session) { 
  session$onSessionEnded(stopApp)
  output$plot <- renderPlot({
  vertebrate%>%
      ggplot(aes_string(x=input$x, y="distance")) +
      geom_point(size=input$pointsize, alpha=0.8) + 
      theme_light()
    })
}

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}




## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
