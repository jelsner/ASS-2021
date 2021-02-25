---
title: "Assignment 6"
author: "Your Name"
date: February 26, 2021
output: 
  html_document:
    keep_md: true
editor_options: 
  chunk_output_type: console
---

Due: February 26, 2021, 3pm

Total Points: 50

The file `http://myweb.fsu.edu/jelsner/temp/data/police.zip` contains demographic information at the county level across the state of Mississippi. Import the data as a simple feature data frame and assign it to an object called `PE.sf` setting the CRS to 4326 using the following code chunk.

```r
library(sf)
```

```
## Linking to GEOS 3.8.1, GDAL 3.1.4, PROJ 6.3.1
```

```r
library(spdep)
```

```
## Loading required package: sp
```

```
## Loading required package: spData
```

```r
if(!"police" %in% list.files()){
  download.file("http://myweb.fsu.edu/jelsner/temp/data/police.zip",
                "police.zip")
unzip("police.zip")
}

PE.sf <- read_sf(dsn = "police", 
                 layer = "police") %>%
  st_set_crs(4326)
```

(a) Create a new variable in `PE.sf` called `pPOLICE` (police expenditure per person) by dividing `POLICE` by `POP` and create a new variable `NONWHITE` by subtracting 100 from `WHITE`. Hint: use the `mutate()` function from the {tidyverse} set of packages. (5)

(b) Create a weights matrix using queen contiguity to define the neighborhoods and `style = "W"` to define the weights. (5)

(c) Fit an ordinary least squares regression model by regressing `pPOLICE` onto crime (`CRIME`) and onto the percentage of non-white people (`NONWHITE`). Save the model object as `model.ols`. Hint: assign the model formula using  `f <- pPOLICE ~ CRIME + NONWHITE` and use the `lm()` function to fit the model. Is the amount of policing associated with more or less crime? Is the amount of policing associated with higher or lower percentage of non-white people? Are these marginal effects significant? Hint: Use the `summary()` method on the model object and interpret the $p$-values. (10)

(d) Test the model residuals for spatial autocorrelation. What do you conclude? (5)

(e) Fit a spatial Durbin error model (SDEM) and save the model object as `model.sdem`.  Determine the $p$-values on the direct and indirect effects. Hint: Use the `errorsarlm()` function from the {spatialreg} package with the argument `etype = "emixed"` then use the `summary()` method on result from obtained with the `impacts()` function. (10)

(f) Perform a likelihood ratio test comparing the SDEM with the OLS model. What model is preferred? (5)

(g) Fit a spatial lag X model (SLXM) and save the model object as `model.slxm` using the `lmSLX()` function from the {spatialreg} package. Then perform a likelihood ratio test comparing the spatial lag X model with the spatial Durbin error model from part (e). What model is preferred? (10)
