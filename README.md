Project 1
================
Jiyue Zhang
2022-06-21

``` r
knitr::opts_chunk$set(echo = FALSE, warning = FALSE, message = FALSE)
```

# Required Packages to run the vignette

We will need `httr`, `jsonlit`, `dplyr`, `tidyverse`, `stringr`
packages.

# create the function to get data from food api

# Basic exploratory data analysis

## Get data

## Numeric summaries

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

![](C:/Users/LAILA/Desktop/558/git/project1/Project1/README_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->![](C:/Users/LAILA/Desktop/558/git/project1/Project1/README_files/figure-gfm/unnamed-chunk-5-2.png)<!-- -->![](C:/Users/LAILA/Desktop/558/git/project1/Project1/README_files/figure-gfm/unnamed-chunk-5-3.png)<!-- -->![](C:/Users/LAILA/Desktop/558/git/project1/Project1/README_files/figure-gfm/unnamed-chunk-5-4.png)<!-- -->![](C:/Users/LAILA/Desktop/558/git/project1/Project1/README_files/figure-gfm/unnamed-chunk-5-5.png)<!-- -->![](C:/Users/LAILA/Desktop/558/git/project1/Project1/README_files/figure-gfm/unnamed-chunk-5-6.png)<!-- -->![](C:/Users/LAILA/Desktop/558/git/project1/Project1/README_files/figure-gfm/unnamed-chunk-5-7.png)<!-- -->![](C:/Users/LAILA/Desktop/558/git/project1/Project1/README_files/figure-gfm/unnamed-chunk-5-8.png)<!-- -->![](C:/Users/LAILA/Desktop/558/git/project1/Project1/README_files/figure-gfm/unnamed-chunk-5-9.png)<!-- -->![](C:/Users/LAILA/Desktop/558/git/project1/Project1/README_files/figure-gfm/unnamed-chunk-5-10.png)<!-- -->
