Project 1
================
Jiyue Zhang
2022-06-21

# Required Packages to run the vignette

We will need `httr`, `jsonlit`, `dplyr`, `tidyverse`, `stringr`
packages.

# create the function to get data from food api

``` r
library(httr)
library(jsonlite)
get_recipes_data <- function(apikey, parameters, ...){
  if (is.null(parameters)) {
    apiurl <-
      paste0("https://api.spoonacular.com/recipes/findByNutrients?apiKey=", apikey)
  } 
  else {
    apiurl <-
      paste0("https://api.spoonacular.com/recipes/findByNutrients?apiKey=", apikey)
    for (i in 1:length(parameters)){
      apiurl <- paste(apiurl, gsub("[[:blank:]]", "",parameters[i]), sep = "&")
      }
  }
  recipes <- GET(apiurl)
  parsed <- fromJSON(rawToChar(recipes$content))
  return(parsed)
}
```

# Basic exploratory data analysis

## Get data

``` r
library(dplyr)
#Store the apiKey first
apikey <- "2dcd260a1aa14ce582831edb99088af2"

#I want to get a data that the max calories is less or equal than 666 since a person need 2000 calories a day, each meal should be around 666 calories. And I also want to set the min value of carbs, fat and protein to 75g, 14g and 16g. These values calculated base on the least proportion of each nutrient that we need a meal
nutrient_data <- get_recipes_data(apikey, c("maxCalories=666","minCrabs=75","minFat=14","minProtein=1"))
nutrient_data <- nutrient_data %>%
  select(-imageType, -image)
```

## Numeric summaries

``` r
nutrient_data$carbs <- as.numeric(gsub("g", " ", nutrient_data$carbs))
nutrient_data$fat <- as.numeric(gsub("g", " ", nutrient_data$fat))
nutrient_data$protein <- as.numeric(gsub("g", " ", nutrient_data$protein))

#create a new variable that present the proportion of crabs in the three elements
nutrient_data$carbs_prop <- nutrient_data$carbs/(nutrient_data$protein+nutrient_data$carbs+nutrient_data$fat)
 #create a new variable that present the proportion of fat in the three elements
nutrient_data$fat_prop <- nutrient_data$fat/(nutrient_data$protein+nutrient_data$carbs+nutrient_data$fat)
#create a new variable that present the proportion of protein in the three elements
nutrient_data$protein_prop <- nutrient_data$protein/(nutrient_data$protein+nutrient_data$carbs+nutrient_data$fat)

#Based on the proportion to evaluate if this recipes is low(less than 0.45), normal(0.45 to 0.65) or high carbs(larger than 0.65) recipie.
nutrient_data$carbs_type <- 
  ifelse(nutrient_data$carbs_prop < 0.45, "low carbs", 
         ifelse(nutrient_data$carbs_prop > 0.65, "high carbs", "normal carbs" ))
#Based on the proportion to evaluate if this recipes is low(less than 0.2), normal(0.2 to 0.35) or high fat(larger than 0.35) recipie.
nutrient_data$fat_type <- 
  ifelse(nutrient_data$fat_prop < 0.2, "low fat", 
         ifelse(nutrient_data$fat_prop > 0.35, "high fat", "normal fat" ))
#Based on the proportion to evaluate if this recipes is low(less than 0.1), normal(0.1 to 0.35) or high protein(larger than 0.35) recipie.
nutrient_data$protein_type <- 
  ifelse(nutrient_data$protein_prop < 0.1, "low protein", 
         ifelse(nutrient_data$protein_prop > 0.35, "high protein", "normal protein" ))

#recipes are health base on the requirements
subset(nutrient_data, carbs_type == "normal carbs" &
         fat_type == "normal fat" &
         protein_type == "normal protein")
```

    ##       id               title calories protein fat carbs
    ## 4 645354   Greek Shrimp Orzo      558      29  28    47
    ## 8 660525 Soft-Baked Pretzels      376       7  22    41
    ##   carbs_prop  fat_prop protein_prop   carbs_type   fat_type
    ## 4  0.4519231 0.2692308    0.2788462 normal carbs normal fat
    ## 8  0.5857143 0.3142857    0.1000000 normal carbs normal fat
    ##     protein_type
    ## 4 normal protein
    ## 8 normal protein

## Contingency tables

``` r
#The recipes that with crabs type and fat type
table(nutrient_data$title, nutrient_data$carbs_type, nutrient_data$fat_type)
```

    ## , ,  = high fat
    ## 
    ##                                                               
    ##                                                                high carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                   0
    ##   Easter Sugar Cookie Bars                                              0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous          0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad               0
    ##   Greek Shrimp Orzo                                                     0
    ##   Nori Seaweed Muffins                                                  0
    ##   Prawn Curry                                                           0
    ##   Simple Sage Pesto                                                     0
    ##   Soft-Baked Pretzels                                                   0
    ##   Trinidadian Chicken Potato Curry                                      0
    ##                                                               
    ##                                                                low carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                  0
    ##   Easter Sugar Cookie Bars                                             0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous         0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad              0
    ##   Greek Shrimp Orzo                                                    0
    ##   Nori Seaweed Muffins                                                 0
    ##   Prawn Curry                                                          0
    ##   Simple Sage Pesto                                                    1
    ##   Soft-Baked Pretzels                                                  0
    ##   Trinidadian Chicken Potato Curry                                     1
    ##                                                               
    ##                                                                normal carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                     0
    ##   Easter Sugar Cookie Bars                                                0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous            0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                 0
    ##   Greek Shrimp Orzo                                                       0
    ##   Nori Seaweed Muffins                                                    0
    ##   Prawn Curry                                                             0
    ##   Simple Sage Pesto                                                       0
    ##   Soft-Baked Pretzels                                                     0
    ##   Trinidadian Chicken Potato Curry                                        0
    ## 
    ## , ,  = low fat
    ## 
    ##                                                               
    ##                                                                high carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                   0
    ##   Easter Sugar Cookie Bars                                              0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous          0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad               0
    ##   Greek Shrimp Orzo                                                     0
    ##   Nori Seaweed Muffins                                                  0
    ##   Prawn Curry                                                           0
    ##   Simple Sage Pesto                                                     0
    ##   Soft-Baked Pretzels                                                   0
    ##   Trinidadian Chicken Potato Curry                                      0
    ##                                                               
    ##                                                                low carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                  0
    ##   Easter Sugar Cookie Bars                                             0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous         1
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad              0
    ##   Greek Shrimp Orzo                                                    0
    ##   Nori Seaweed Muffins                                                 0
    ##   Prawn Curry                                                          0
    ##   Simple Sage Pesto                                                    0
    ##   Soft-Baked Pretzels                                                  0
    ##   Trinidadian Chicken Potato Curry                                     0
    ##                                                               
    ##                                                                normal carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                     1
    ##   Easter Sugar Cookie Bars                                                0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous            0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                 0
    ##   Greek Shrimp Orzo                                                       0
    ##   Nori Seaweed Muffins                                                    0
    ##   Prawn Curry                                                             1
    ##   Simple Sage Pesto                                                       0
    ##   Soft-Baked Pretzels                                                     0
    ##   Trinidadian Chicken Potato Curry                                        0
    ## 
    ## , ,  = normal fat
    ## 
    ##                                                               
    ##                                                                high carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                   0
    ##   Easter Sugar Cookie Bars                                              1
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous          0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad               0
    ##   Greek Shrimp Orzo                                                     0
    ##   Nori Seaweed Muffins                                                  0
    ##   Prawn Curry                                                           0
    ##   Simple Sage Pesto                                                     0
    ##   Soft-Baked Pretzels                                                   0
    ##   Trinidadian Chicken Potato Curry                                      0
    ##                                                               
    ##                                                                low carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                  0
    ##   Easter Sugar Cookie Bars                                             0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous         0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad              1
    ##   Greek Shrimp Orzo                                                    0
    ##   Nori Seaweed Muffins                                                 0
    ##   Prawn Curry                                                          0
    ##   Simple Sage Pesto                                                    0
    ##   Soft-Baked Pretzels                                                  0
    ##   Trinidadian Chicken Potato Curry                                     0
    ##                                                               
    ##                                                                normal carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                     0
    ##   Easter Sugar Cookie Bars                                                0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous            0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                 0
    ##   Greek Shrimp Orzo                                                       1
    ##   Nori Seaweed Muffins                                                    1
    ##   Prawn Curry                                                             0
    ##   Simple Sage Pesto                                                       0
    ##   Soft-Baked Pretzels                                                     1
    ##   Trinidadian Chicken Potato Curry                                        0

``` r
#The recipes that with crabs type and protein type
table(nutrient_data$title, nutrient_data$carbs_type, nutrient_data$protein_type)
```

    ## , ,  = high protein
    ## 
    ##                                                               
    ##                                                                high carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                   0
    ##   Easter Sugar Cookie Bars                                              0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous          0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad               0
    ##   Greek Shrimp Orzo                                                     0
    ##   Nori Seaweed Muffins                                                  0
    ##   Prawn Curry                                                           0
    ##   Simple Sage Pesto                                                     0
    ##   Soft-Baked Pretzels                                                   0
    ##   Trinidadian Chicken Potato Curry                                      0
    ##                                                               
    ##                                                                low carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                  0
    ##   Easter Sugar Cookie Bars                                             0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous         1
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad              1
    ##   Greek Shrimp Orzo                                                    0
    ##   Nori Seaweed Muffins                                                 0
    ##   Prawn Curry                                                          0
    ##   Simple Sage Pesto                                                    0
    ##   Soft-Baked Pretzels                                                  0
    ##   Trinidadian Chicken Potato Curry                                     1
    ##                                                               
    ##                                                                normal carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                     0
    ##   Easter Sugar Cookie Bars                                                0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous            0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                 0
    ##   Greek Shrimp Orzo                                                       0
    ##   Nori Seaweed Muffins                                                    0
    ##   Prawn Curry                                                             0
    ##   Simple Sage Pesto                                                       0
    ##   Soft-Baked Pretzels                                                     0
    ##   Trinidadian Chicken Potato Curry                                        0
    ## 
    ## , ,  = low protein
    ## 
    ##                                                               
    ##                                                                high carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                   0
    ##   Easter Sugar Cookie Bars                                              1
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous          0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad               0
    ##   Greek Shrimp Orzo                                                     0
    ##   Nori Seaweed Muffins                                                  0
    ##   Prawn Curry                                                           0
    ##   Simple Sage Pesto                                                     0
    ##   Soft-Baked Pretzels                                                   0
    ##   Trinidadian Chicken Potato Curry                                      0
    ##                                                               
    ##                                                                low carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                  0
    ##   Easter Sugar Cookie Bars                                             0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous         0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad              0
    ##   Greek Shrimp Orzo                                                    0
    ##   Nori Seaweed Muffins                                                 0
    ##   Prawn Curry                                                          0
    ##   Simple Sage Pesto                                                    0
    ##   Soft-Baked Pretzels                                                  0
    ##   Trinidadian Chicken Potato Curry                                     0
    ##                                                               
    ##                                                                normal carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                     0
    ##   Easter Sugar Cookie Bars                                                0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous            0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                 0
    ##   Greek Shrimp Orzo                                                       0
    ##   Nori Seaweed Muffins                                                    1
    ##   Prawn Curry                                                             0
    ##   Simple Sage Pesto                                                       0
    ##   Soft-Baked Pretzels                                                     0
    ##   Trinidadian Chicken Potato Curry                                        0
    ## 
    ## , ,  = normal protein
    ## 
    ##                                                               
    ##                                                                high carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                   0
    ##   Easter Sugar Cookie Bars                                              0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous          0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad               0
    ##   Greek Shrimp Orzo                                                     0
    ##   Nori Seaweed Muffins                                                  0
    ##   Prawn Curry                                                           0
    ##   Simple Sage Pesto                                                     0
    ##   Soft-Baked Pretzels                                                   0
    ##   Trinidadian Chicken Potato Curry                                      0
    ##                                                               
    ##                                                                low carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                  0
    ##   Easter Sugar Cookie Bars                                             0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous         0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad              0
    ##   Greek Shrimp Orzo                                                    0
    ##   Nori Seaweed Muffins                                                 0
    ##   Prawn Curry                                                          0
    ##   Simple Sage Pesto                                                    1
    ##   Soft-Baked Pretzels                                                  0
    ##   Trinidadian Chicken Potato Curry                                     0
    ##                                                               
    ##                                                                normal carbs
    ##   Chicken Brats & Root Beer BBQ Sauce                                     1
    ##   Easter Sugar Cookie Bars                                                0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous            0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                 0
    ##   Greek Shrimp Orzo                                                       1
    ##   Nori Seaweed Muffins                                                    0
    ##   Prawn Curry                                                             1
    ##   Simple Sage Pesto                                                       0
    ##   Soft-Baked Pretzels                                                     1
    ##   Trinidadian Chicken Potato Curry                                        0

``` r
#The recipes that with crabs protein and fat type
table(nutrient_data$title, nutrient_data$protein_type, nutrient_data$fat_type)
```

    ## , ,  = high fat
    ## 
    ##                                                               
    ##                                                                high protein
    ##   Chicken Brats & Root Beer BBQ Sauce                                     0
    ##   Easter Sugar Cookie Bars                                                0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous            0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                 0
    ##   Greek Shrimp Orzo                                                       0
    ##   Nori Seaweed Muffins                                                    0
    ##   Prawn Curry                                                             0
    ##   Simple Sage Pesto                                                       0
    ##   Soft-Baked Pretzels                                                     0
    ##   Trinidadian Chicken Potato Curry                                        1
    ##                                                               
    ##                                                                low protein
    ##   Chicken Brats & Root Beer BBQ Sauce                                    0
    ##   Easter Sugar Cookie Bars                                               0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous           0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                0
    ##   Greek Shrimp Orzo                                                      0
    ##   Nori Seaweed Muffins                                                   0
    ##   Prawn Curry                                                            0
    ##   Simple Sage Pesto                                                      0
    ##   Soft-Baked Pretzels                                                    0
    ##   Trinidadian Chicken Potato Curry                                       0
    ##                                                               
    ##                                                                normal protein
    ##   Chicken Brats & Root Beer BBQ Sauce                                       0
    ##   Easter Sugar Cookie Bars                                                  0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous              0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                   0
    ##   Greek Shrimp Orzo                                                         0
    ##   Nori Seaweed Muffins                                                      0
    ##   Prawn Curry                                                               0
    ##   Simple Sage Pesto                                                         1
    ##   Soft-Baked Pretzels                                                       0
    ##   Trinidadian Chicken Potato Curry                                          0
    ## 
    ## , ,  = low fat
    ## 
    ##                                                               
    ##                                                                high protein
    ##   Chicken Brats & Root Beer BBQ Sauce                                     0
    ##   Easter Sugar Cookie Bars                                                0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous            1
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                 0
    ##   Greek Shrimp Orzo                                                       0
    ##   Nori Seaweed Muffins                                                    0
    ##   Prawn Curry                                                             0
    ##   Simple Sage Pesto                                                       0
    ##   Soft-Baked Pretzels                                                     0
    ##   Trinidadian Chicken Potato Curry                                        0
    ##                                                               
    ##                                                                low protein
    ##   Chicken Brats & Root Beer BBQ Sauce                                    0
    ##   Easter Sugar Cookie Bars                                               0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous           0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                0
    ##   Greek Shrimp Orzo                                                      0
    ##   Nori Seaweed Muffins                                                   0
    ##   Prawn Curry                                                            0
    ##   Simple Sage Pesto                                                      0
    ##   Soft-Baked Pretzels                                                    0
    ##   Trinidadian Chicken Potato Curry                                       0
    ##                                                               
    ##                                                                normal protein
    ##   Chicken Brats & Root Beer BBQ Sauce                                       1
    ##   Easter Sugar Cookie Bars                                                  0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous              0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                   0
    ##   Greek Shrimp Orzo                                                         0
    ##   Nori Seaweed Muffins                                                      0
    ##   Prawn Curry                                                               1
    ##   Simple Sage Pesto                                                         0
    ##   Soft-Baked Pretzels                                                       0
    ##   Trinidadian Chicken Potato Curry                                          0
    ## 
    ## , ,  = normal fat
    ## 
    ##                                                               
    ##                                                                high protein
    ##   Chicken Brats & Root Beer BBQ Sauce                                     0
    ##   Easter Sugar Cookie Bars                                                0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous            0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                 1
    ##   Greek Shrimp Orzo                                                       0
    ##   Nori Seaweed Muffins                                                    0
    ##   Prawn Curry                                                             0
    ##   Simple Sage Pesto                                                       0
    ##   Soft-Baked Pretzels                                                     0
    ##   Trinidadian Chicken Potato Curry                                        0
    ##                                                               
    ##                                                                low protein
    ##   Chicken Brats & Root Beer BBQ Sauce                                    0
    ##   Easter Sugar Cookie Bars                                               1
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous           0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                0
    ##   Greek Shrimp Orzo                                                      0
    ##   Nori Seaweed Muffins                                                   1
    ##   Prawn Curry                                                            0
    ##   Simple Sage Pesto                                                      0
    ##   Soft-Baked Pretzels                                                    0
    ##   Trinidadian Chicken Potato Curry                                       0
    ##                                                               
    ##                                                                normal protein
    ##   Chicken Brats & Root Beer BBQ Sauce                                       0
    ##   Easter Sugar Cookie Bars                                                  0
    ##   Garam Masala Pork Chops with Mint Yogurt and Spiced Couscous              0
    ##   Gluten Free Dairy Free Sugar Free Chinese Chicken Salad                   0
    ##   Greek Shrimp Orzo                                                         1
    ##   Nori Seaweed Muffins                                                      0
    ##   Prawn Curry                                                               0
    ##   Simple Sage Pesto                                                         0
    ##   Soft-Baked Pretzels                                                       1
    ##   Trinidadian Chicken Potato Curry                                          0

## plot

``` r
library(tidyverse)
#barplot
ggplot(nutrient_data, aes(x = carbs_type)) +
  geom_bar() +
  labs(x = "Carbs Type", 
       title = "Bar Plot of the Level of Carbs for the Recipes")
```

![](../README_files/unnamed-chunk-5-1.png)<!-- -->

``` r
ggplot(nutrient_data, aes(x = fat_type)) +
  geom_bar() +
  labs(x = "Fat Type", 
       title = "Bar Plot of the Level of Fat for the Recipes")
```

![](../README_files/unnamed-chunk-5-2.png)<!-- -->

``` r
ggplot(nutrient_data, aes(x = protein_type)) +
  geom_bar() +
  labs(x = "Protein Type", 
       title = "Bar Plot of the Level of Protein for the Recipes")
```

![](../README_files/unnamed-chunk-5-3.png)<!-- -->

``` r
#histogram
ggplot(nutrient_data, aes(x = carbs_prop)) + 
  geom_histogram(color = "black", fill = "red", binwidth = 0.25) + 
  labs(x = "Carbs", title = "Histogram of the Proportion of Carbs for the Recipes")
```

![](../README_files/unnamed-chunk-5-4.png)<!-- -->

``` r
ggplot(nutrient_data, aes(x = fat_prop)) + 
  geom_histogram(color = "black", fill = "blue", binwidth = 0.25) + 
  labs(x = "Fat", title = "Histogram of the Proportion of Fat for the Recipes")
```

![](../README_files/unnamed-chunk-5-5.png)<!-- -->

``` r
ggplot(nutrient_data, aes(x = protein_prop)) + 
  geom_histogram(color = "black", fill = "yellow", binwidth = 0.25) + 
  labs(x = "Protein", title = "Histogram of the Proportion of Protein for the Recipes")
```

![](../README_files/unnamed-chunk-5-6.png)<!-- -->

``` r
#box plot
ggplot(nutrient_data, aes(x = carbs_type, y = carbs_prop)) +
  geom_boxplot() +
  labs(x = "Level of Carbs", title = "Boxplot of the proprtion of carbs base on carbs type")
```

![](../README_files/unnamed-chunk-5-7.png)<!-- -->

``` r
#scatter plot
library(stringr)
nutrient_data$title<- str_wrap(nutrient_data$title, width = 20)
ggplot(nutrient_data, aes(x = carbs_type, y = fat_type)) +
  geom_point()+
  labs(x = "Level of Carbs", y = "Level of Fat", 
       title = "Scatterplot of Level of Carbs and Level of Fat") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1),
        strip.text = element_text(size = 5)) +
  facet_wrap(~ title)
```

![](../README_files/unnamed-chunk-5-8.png)<!-- -->

``` r
ggplot(nutrient_data, aes(x = carbs_type, y = protein_type)) +
  geom_point()+
  labs(x = "Level of Carbs", y = "Level of Protein", 
       title = "Scatterplot of Level of Carbs and Level of Protein") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1),
        strip.text = element_text(size = 5)) +
  facet_wrap(~ title)
```

![](../README_files/unnamed-chunk-5-9.png)<!-- -->

``` r
ggplot(nutrient_data, aes(x = fat_type, y = protein_type)) +
  geom_point()+
  labs(x = "Level of Fat", y = "Level of Protein", 
       title = "Scatterplot of Level of Fat and Level of Protein") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1),
        strip.text = element_text(size = 5)) +
  facet_wrap(~ title)
```

![](../README_files/unnamed-chunk-5-10.png)<!-- -->
