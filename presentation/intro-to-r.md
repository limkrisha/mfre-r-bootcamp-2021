---
title: "Intro to R (Summer 2021)"
output:
  html_document:
    toc: TRUE
    toc_float: TRUE
    theme: lumen
    keep_md: TRUE
---



The main objective of this R bootcamp is to introduce R programming to incoming MFRE students. The content of this bootcamp was adapted from [Software Carpentry](https://datacarpentry.org/r-socialsci/), Dr. Nick Huntington-Klein's Teaching Econometrics with R [slides](https://rpubs.com/NickCHK/RTeach2020), and the [Open Case Studies](https://www.opencasestudies.org/ocs-bp-co2-emissions/#Data_Analysis). If you spot any errors or issues, please email krisha.lim[at]ubc.ca. 

# Before we get started

  * Familiarize yourself with the RStudio IDE
  * Understand the benefits of a project-oriented workflow. Read Jenny Bryan's post on why you should not start your scripts with `setwd()` and `rm(list = ls())` [here](https://www.tidyverse.org/blog/2017/12/workflow-vs-script/). 

#  Packages

There are thousands of packages (bundles of codes) available on [CRAN](https://cran.r-project.org/). If you are struggling to accomplish a certain task, it is very likely that someone has already created a function or a package to do it for you. 

To install a package, use the command `install.package("package_name")` - do not forget the quotation marks! To install multiple packages at once, you can use the code `install.packages(c("package_1", "package_2", ...))`. 

A big difference between R and Stata is that you have to load the package you will (or plan to use) use every time you start a new R session. To load a package, use the command `library(package_name)` - the quotation marks are now optional. You can write a function to load multiple libraries at once.

The `pacman` package allows you to install and load packages in R more efficiently. The function `p_load()` checks whether a package is already installed, and if not, installs the package and loads it. The function can also be applied to several package at once, so you save a few (or many) lines of codes. 


```r
# install.packages("pacman")
pacman::p_load(tidyverse, readxl, googlesheets4, readstata13, magrittr, cansim, Quandl, stats, broom, modelsummary)
```
 


# R Basics 

By the end of this section, students will be able to

* create, name, and assign values to objects
* use comments to inform script
* inspect, manipulate, and subset vectors, lists, and dataframes/tibbles
* call functions and use arguments to change default options
* deal with missing values in objects

## Interacting with R 

We can get output from R console by typing math expressions. 


```r
1+1 
2+4*1^3
100 %/% 60 
100 %% 60
```

We can also get output from R by typing logic statements in the console.


```r
1 > 2 & 1 > 0.5 # The "&" stands for "and"
1 >2 | 1 > 0.5 # The "|" stands for "or"
3 + 4 == 4 + 3 # The "==" stands for "equal to"
3 + 4 != 4 + 3 # The "!" is negation
```

To do useful things, we assign values to objects. `<-` is the assignment operator in R. Objects are one of the main differences of R with Stata. 

To create an object, the syntax is `object_name <- value`. When assigning values to an object, R does not automatically print the object; you will have to print this object to see the value or output. 

It is also smart to comment on your code. In R, the `#` symbol indicates the start of the comment. 


```r
x <- 3 
x # prints the value of x
```

```
## [1] 3
```

```r
x <- 10
x # we can also overwrite the value of an existing object
```

```
## [1] 10
```

## Data object types

There are different types of objects in R. The common ones we will use are: numeric, character, factor, and logical. There are different functions (or commands) in R to examine objects. Common examples are `class()`, `typeof()`, `str()`, `length()`. 


```r
numeric_var <- 1.5
character_var <- "one"
factor_var <- factor(1, labels = "one")
logical_var <- TRUE
```

## Vectors

Multiple observations of the same type are called vectors. You use the `c()` function to concatenate values.  

For example, we can create a vector of the emissions of three countries and assign it to a new object called `emissions`. 


```r
emissions <- c(53700, 14300, 5250000)
emissions
```

```
## [1]   53700   14300 5250000
```

We can also create a character vector called `countries`. Quotes around the text are important to indicate the data is of the type character. If not, R will think it is an object. Since these objects don't exist in R, you will get an error. 


```r
countries <- c("Canada", "Kenya", "United States")
countries
```

```
## [1] "Canada"        "Kenya"         "United States"
```

You can also add values to existing vectors.

```r
countries <- c(countries, "China")
countries
```

```
## [1] "Canada"        "Kenya"         "United States" "China"
```

Here are some functions to inspect the content of a vector. 

```r
length(countries) # inspect the number of elements in a vector
```

```
## [1] 4
```

```r
class(countries) # type/class of an object
```

```
## [1] "character"
```

```r
class(emissions)
```

```
## [1] "numeric"
```

```r
# The str() function provides the structure of an object and a preview of its elements. This function is useful when inspecting large and complex objects. 
str(emissions) 
```

```
##  num [1:3] 53700 14300 5250000
```

Vectors must always be of the same type. If we try to mix different types (character, numeric, logical, etc.) in a vector, R will force the content to be the same. 


```r
trythis <- c(1, 2, 3, "a")
class(trythis)
```

```
## [1] "character"
```

### Subsetting Vectors

We use the index position of an element in square brackets to extract one or more elements from a vector. Note that the index starts at 1. 


```r
# to extract the first element
emissions[1]
```

```
## [1] 53700
```

```r
# to extract the first and third element
emissions[c(1,3)]
```

```
## [1]   53700 5250000
```

```r
# to extract the second and third element
emissions[c(2:3)]
```

```
## [1]   14300 5250000
```

We can also use logical tests to subset vectors. 


```r
# to select emissions greater than 100000
emissions[emissions > 100000]
```

```
## [1] 5250000
```

```r
# to select emissions that are greater than 0 and less than 55000
emissions[emissions > 0 & emissions < 55000]
```

```
## [1] 53700 14300
```

We can also use `%in` to check whether an object is contained within or matches with a list of items. 


```r
emissions %in% c("Canada")
```

```
## [1] FALSE FALSE FALSE
```

```r
countries %in% c("Canada", "United States")
```

```
## [1]  TRUE FALSE  TRUE FALSE
```

```r
emissions %in% c(14300, 50000)
```

```
## [1] FALSE  TRUE FALSE
```

## Lists

A list is a flexible R object that is a collection of different objects. You can extract sub-objects using `[[]]` or `$`. A common example of a list object you will interact with often is regression objects (more on this later!). 


```r
first_list <- list(a = 1:5, b = 6:10, c = c("food", "resource", "economics"))
first_list #print the output
```

```
## $a
## [1] 1 2 3 4 5
## 
## $b
## [1]  6  7  8  9 10
## 
## $c
## [1] "food"      "resource"  "economics"
```

```r
class(first_list)
```

```
## [1] "list"
```

```r
class(first_list$a)
```

```
## [1] "integer"
```

```r
class(first_list[["c"]])
```

```
## [1] "character"
```

```r
first_list[["a"]] # extract elements in a using [[]]
```

```
## [1] 1 2 3 4 5
```

```r
first_list$c #extract elements in c using $ 
```

```
## [1] "food"      "resource"  "economics"
```

## Data Frames

In MFRE, we will work a lot with data frames. A data frame is a list composed of vectors of equal length. Data frames can story different data types in each column. For example, the first column can be a character vector, and the second column is a numeric vector. You can createa data frame object using the function `data.frame()`.


```r
first_df <- data.frame(countries = c("Canada", "Kenya", "United States"),
                       emissions = c(53700, 14300, 5250000))

first_df
```

```
##       countries emissions
## 1        Canada     53700
## 2         Kenya     14300
## 3 United States   5250000
```

```r
# class(first_df)
# extract values in country column: first_df[["countries]] or first_df$countries
```

A special type of data frame we will work with in this bootcamp is called tibbles; you can read more about it [here](https://r4ds.had.co.nz/tibbles.html). 

We can convert the `first_df` into a tibble using the `as_tibble()` function and call it `first_tibble`. We can also create a new tibble, which we will call `second_tibble` 


```r
first_tibble <- as_tibble(first_df)
first_tibble 
```

```
## # A tibble: 3 x 2
##   countries     emissions
##   <chr>             <dbl>
## 1 Canada            53700
## 2 Kenya             14300
## 3 United States   5250000
```

```r
class(first_tibble)
```

```
## [1] "tbl_df"     "tbl"        "data.frame"
```

```r
# first_tibble[["countries"]] or first_tibble$countries to extract the data in the countries column

second_tibble <- tibble(countries = c("Peru", "Mexico", "China"),
                        emissions = c(61700, 480000, 10300000))
```

## Functions

Functions are scripts that automate complicated commands. You can write your own function, or you can also use functions that are available in R packages. A function gets one or more inputs called *arguments*. Functions often, but not always, return a value. One example of a function is the `sqrt()`. If you type in `?sqrt()` in the console, you will the documentation on the lower right panel under the 'Help` tab. You learn that the input (argument) must be numeric, and running the function will return the square root of the number. 


```r
a <- sqrt(100)
a
```

```
## [1] 10
```

In the example above, the value 100 is given to the `sqrt()` function. The `sqrt()` function calculates the square root, and returns the value assigned to the object `a`. 

Arguments differ per function, and you can look up the documentation using the `?function_name` command in your console. Some functions take arguments that must be specified by the user, and if not, will use a default value - these are called options. Options are ways to change how the function works. 

Let's try a function that takes multiple arguments -- `round()`. 


```r
round(3.14159)
```

```
## [1] 3
```

Here, we used the `round()` function with just one argument, `3.14159`. The function returned a value of 3. If we look at the documentation in `?round`, we see that the default option is `digits = 0` or to round up to the nearest whole number. If we want to round up to only 2 decimal places, we can type in `digits = 2`. 


```r
round(3.14159, digits = 2)
```

```
## [1] 3.14
```

If you provide the arguments in the same order as they are defined, i.e. `round(x, digits = 0)`, then you don't need to include `digits =` anymore. 


```r
round(3.14159, 2)
```

```
## [1] 3.14
```

You can find some built in R functions [here](https://www.statmethods.net/management/functions.html). Common ones we will use are `mean()`, `median()`, `min()`, `max()`. The code below shows you can take the mean of the emissions variable in the `first_tibble` tibble. 


```r
mean(first_tibble$emissions)
```

```
## [1] 1772667
```

For these statistical functions, you will have to indicate how the function will treat missing values. In R, missing data are represented in vectors as `NA`. If you don't specify how the function will treat missing values, the function will return NA. 

Let's return to our `first_tibble` object. Let's say we want to add Peru's emissions, but we don't have the data yet. We can use the `add_row()` function to do this step. Notice that I also have a `%>%` operator there. It is called the pipe. The pipe operator allows you to express a series of operators clearly (more info [here](https://r4ds.had.co.nz/pipes.html)). It takes the output on the left of the `%>%` and pass it to the function on the right. In the code below, `first_tibble` (left of the `%>%`) is passed on to another function, which is the `add_row` function. Then we are assigning it back to `first_tibble`, overwriting our initial data. If we don't use the assignment operator `<-`, then we have not overwritten `first_tibble`


```r
# let's first add a row with missing data in first_tibble
# no assignment operator
first_tibble %>% 
  add_row(countries = "Peru", emissions = NA)
```

```
## # A tibble: 4 x 2
##   countries     emissions
##   <chr>             <dbl>
## 1 Canada            53700
## 2 Kenya             14300
## 3 United States   5250000
## 4 Peru                 NA
```

```r
first_tibble # Peru is not saved in first_tibble
```

```
## # A tibble: 3 x 2
##   countries     emissions
##   <chr>             <dbl>
## 1 Canada            53700
## 2 Kenya             14300
## 3 United States   5250000
```

```r
first_tibble <- first_tibble %>% 
  add_row(countries = "Peru", emissions = NA)

first_tibble 
```

```
## # A tibble: 4 x 2
##   countries     emissions
##   <chr>             <dbl>
## 1 Canada            53700
## 2 Kenya             14300
## 3 United States   5250000
## 4 Peru                 NA
```

Now, let's try to take the mean of emissions. 


```r
mean(first_tibble$emissions)
```

```
## [1] NA
```

Notice that you `NA` as the output. This feature makes it harder to overlook cases where you are dealing with missing data. If you take a look at the documentation (`?mean`), you can see that the default is `na.rm = FALSE`. By adding in `na.rm = TRUE`, then you are asking R to calculate the mean and that NA values should be ignored. Now the output will show 1.7726667\times 10^{6}


```r
mean(first_tibble$emissions, na.rm = T)
```

```
## [1] 1772667
```

### Writing your own functions

You may consider writing your own functions to automate certain tasks and to reuse them later on. Read more about it [here](http://environmentalcomputing.net/writing-simple-functions/). If you find yourself repeating the same steps (in your current or across multiple scripts), writing a function may help you save time and also avoid mistakes. 

The syntax of a function is 


```r
function_name <- function(arg1, arg2, ...){
  statements # do something interesting
  object # return value
}
```

  * The `function_name` is the name you will provide to the function. You can call it anything you want, as long as it is not a keyword in R. Also, provide a meaningful name, such as a short description of the function. It is best not to call it `f1` or `func1`. 

  * The `arg1, arg2,...` are the arguments of the function. A function can have multiple arguments and can take in different data types. 

  * The code between the `{}` contains the body of the function, and will contain the code that will run everytime you call `function_name`. 

  * The `object` is the value to be returned by the function. Some people may write `return(object)` or `object` or do not specify. 

Let's take a look at an example. R does not have a built in function to calculate standard error. Recall that the formula of the standard error is $ SE_\tilde{x} =  \sqrt{var/ n} $


```r
first_tibble %<>% add_column(gdp = c(50300, 1090, 52100, 6110))
```

Let's say we want to calculate the standard error for emissions.


```r
sqrt(var(first_tibble$emissions, na.rm = T) / length(first_tibble$emissions))
```

```
## [1] 1505762
```

Not too bad! But what if you want to calculate the standard error for multiple variables, then you'd have to write the code above multiple times. Even if you copy paste the code and change the variable names, this process may be prone to errors. A better approach would be to make it a function. Following the syntax, we can write the standard error function as follows. 


```r
std_error <- function(x){
  sqrt(var(x, na.rm = T) / length(x))
  # because the calculation will return an output, we no longer have to indicate the return value 
}
```

Here, `std_error` is the function name. It takes `x` as the argument, which in our case would be a vector. Then everytime we pass a vector to this function, it will calculate the formula provided in the body. 

Now we can call use this function similar to the built-in functions of R introduced earlier. 


```r
std_error(first_tibble$emissions)
```

```
## [1] 1505762
```

```r
std_error(first_tibble$gdp)
```

```
## [1] 13783.99
```

# Data Import

You will likely work with existing data in the MFRE program and beyond. In this section, you will learn how to load data from 5 different sources : 

 * csv 
 * xlsx
 * dta files (Stata)
 * Google Sheet
 * API (Statistics Canada, Quandl)

It's common for people to save data in spreadsheets as comma-separated values, or csv. To open a csv file in R, we use the `read_csv([insert_file_path_here])` function of the tidyverse package. 

The code below shows that we are reading the file "yearly_co2_emissions.csv" saved in our data folder and assigning it to the data object called `carbon`. Assigning data to objects is one of the big difference between R and Stata, as R allows you to work with multiple data sets at once. 


```r
carbon <- read_csv("../data/yearly_co2_emissions.csv")
```

  * Notice the quotation marks around the file path. The file path is relative to the location of the R project folder. My R project folder is called 2021-r-bootcamp, and in it I have multiple folders including "code" (where my current script is saved) and "data" (where data files are saved). The `../' in the file path means that R has to go two folders up from where the current script lives (i.e. go to "code" then go to "2021-r-bootcamp") to find the "data" folder and the file name itself. 


```r
temp <- read_csv("../data/temperature.csv", skip = 4, na = "-99")
```


```r
energy_hist <- read_xlsx("../data/energy_use_per_person.xlsx", sheet = 1)
energy_recent <- read_xlsx("../data/energy_use_per_person.xlsx", sheet = 2)
energy <- full_join(energy_hist, energy_recent, by = c("country"))

politics <- read.dta13("../data/political_party.dta")

gs4_deauth() # so no need to sign in
disasters <- read_sheet("https://docs.google.com/spreadsheets/d/17s15o7jdDpGSKgsIboZdnYU2UxHtU9DHKNRmYVVgwJo/edit#gid=0", skip = 2) 


gdp <- read_xlsx("../data/gdp_per_capita_yearly_growth.xlsx")

ag <- get_cansim('32-10-0359-01')

corn <- Quandl("CHRIS/LIFFE_EMA1")

politics <- read.dta13("../data/politics.dta", nonint.factors = TRUE)
```

  ** Note to self: Need to add about how to call different datasets and variables using the `$` operator
  
## View Data

The following functions can be used to explore your dataset

```r
head(politics)
tail(politics)
names(politics)
dim(politics)
str(politics)
glimpse(politics)
```

Using `dplyr`


```r
carbon %>% 
  select(country, `2010`:`2014`) %>%
  slice_head(n = 5)
```

```
## # A tibble: 5 x 6
##   country     `2010` `2011` `2012` `2013` `2014`
##   <chr>        <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
## 1 Afghanistan   8460  12200  10800  10000   9810
## 2 Albania       4600   5240   4910   5060   5720
## 3 Algeria     119000 121000 130000 134000 145000
## 4 Andorra        517    491    488    477    462
## 5 Angola       29100  30300  33400  32600  34800
```

```r
set.seed(2021)
carbon %>% 
  select(country, `2010`:`2014`) %>%
  slice_sample(n = 10)
```

```
## # A tibble: 10 x 6
##    country       `2010` `2011` `2012` `2013` `2014`
##    <chr>          <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 Peru           57600  49600  55100  57200  61700
##  2 Sweden         52000  51700  47000  44800  43400
##  3 Tonga            117    103    106    114    121
##  4 Uzbekistan    104000 114000 116000 103000 105000
##  5 Romania        79400  84900  81700  70900  70000
##  6 Guinea-Bissau    238    246    253    257    271
##  7 Zambia          2690   2940   3670   3960   4500
##  8 Malawi          1140   1180   1100   1220   1280
##  9 Mexico        464000 484000 496000 490000 480000
## 10 Guinea          2600   2780   2580   2300   2450
```

## Basic data exploration

The `modelsummary` package allows you to produce very nice summary statistic plots for tidy data. You can read more [here](https://vincentarelbundock.github.io/modelsummary/articles/datasummary.html). 


```r
politics %>% group_by(region) %>% summarize(mean(v2x_libdem, na.rm = T))
```

```
## # A tibble: 7 x 2
##   region                  `mean(v2x_libdem, na.rm = T)`
##   <chr>                                           <dbl>
## 1 ""                                             0.162 
## 2 "Africa"                                       0.141 
## 3 "Americas"                                     0.224 
## 4 "Eastern Mediterranean"                        0.0881
## 5 "Europe"                                       0.372 
## 6 "South-East Asia"                              0.152 
## 7 "Western Pacific"                              0.245
```

```r
datasummary_skim(politics)
```

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> Unique (#) </th>
   <th style="text-align:right;"> Missing (%) </th>
   <th style="text-align:right;"> Mean </th>
   <th style="text-align:right;"> SD </th>
   <th style="text-align:right;"> Min </th>
   <th style="text-align:right;"> Median </th>
   <th style="text-align:right;"> Max </th>
   <th style="text-align:right;">    </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> year </td>
   <td style="text-align:right;"> 233 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1927.2 </td>
   <td style="text-align:right;"> 64.0 </td>
   <td style="text-align:right;"> 1789.0 </td>
   <td style="text-align:right;"> 1937.0 </td>
   <td style="text-align:right;"> 2020.0 </td>
   <td style="text-align:right;">  <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" class="svglite" width="48.00pt" height="12.00pt" viewBox="0 0 48.00 12.00"><defs><style type="text/css">
    .svglite line, .svglite polyline, .svglite polygon, .svglite path, .svglite rect, .svglite circle {
      fill: none;
      stroke: #000000;
      stroke-linecap: round;
      stroke-linejoin: round;
      stroke-miterlimit: 10.00;
    }
  </style></defs><rect width="100%" height="100%" style="stroke: none; fill: none;"></rect><defs><clipPath id="cpMC4wMHw0OC4wMHwwLjAwfDEyLjAw"><rect x="0.00" y="0.00" width="48.00" height="12.00"></rect></clipPath></defs><g clip-path="url(#cpMC4wMHw0OC4wMHwwLjAwfDEyLjAw)">
</g><defs><clipPath id="cpMC4wMHw0OC4wMHwyLjg4fDEyLjAw"><rect x="0.00" y="2.88" width="48.00" height="9.12"></rect></clipPath></defs><g clip-path="url(#cpMC4wMHw0OC4wMHwyLjg4fDEyLjAw)"><rect x="0.046" y="9.81" width="3.85" height="1.86" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="3.89" y="8.39" width="3.85" height="3.27" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="7.74" y="7.94" width="3.85" height="3.72" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="11.59" y="7.61" width="3.85" height="4.05" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="15.44" y="8.18" width="3.85" height="3.49" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="19.29" y="8.26" width="3.85" height="3.40" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="23.13" y="5.29" width="3.85" height="6.37" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="26.98" y="4.45" width="3.85" height="7.21" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="30.83" y="4.38" width="3.85" height="7.28" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="34.68" y="4.23" width="3.85" height="7.43" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="38.53" y="3.75" width="3.85" height="7.91" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="42.37" y="3.22" width="3.85" height="8.44" style="stroke-width: 0.38; fill: #000000;"></rect></g></svg>
</td>
  </tr>
  <tr>
   <td style="text-align:left;"> v2x_libdem </td>
   <td style="text-align:right;"> 884 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.2 </td>
   <td style="text-align:right;"> 0.2 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 0.1 </td>
   <td style="text-align:right;"> 0.9 </td>
   <td style="text-align:right;">  <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" class="svglite" width="48.00pt" height="12.00pt" viewBox="0 0 48.00 12.00"><defs><style type="text/css">
    .svglite line, .svglite polyline, .svglite polygon, .svglite path, .svglite rect, .svglite circle {
      fill: none;
      stroke: #000000;
      stroke-linecap: round;
      stroke-linejoin: round;
      stroke-miterlimit: 10.00;
    }
  </style></defs><rect width="100%" height="100%" style="stroke: none; fill: none;"></rect><defs><clipPath id="cpMC4wMHw0OC4wMHwwLjAwfDEyLjAw"><rect x="0.00" y="0.00" width="48.00" height="12.00"></rect></clipPath></defs><g clip-path="url(#cpMC4wMHw0OC4wMHwwLjAwfDEyLjAw)">
</g><defs><clipPath id="cpMC4wMHw0OC4wMHwyLjg4fDEyLjAw"><rect x="0.00" y="2.88" width="48.00" height="9.12"></rect></clipPath></defs><g clip-path="url(#cpMC4wMHw0OC4wMHwyLjg4fDEyLjAw)"><rect x="1.53" y="3.56" width="2.51" height="8.10" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="4.03" y="3.22" width="2.51" height="8.44" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="6.54" y="6.16" width="2.51" height="5.50" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="9.04" y="7.63" width="2.51" height="4.03" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="11.55" y="9.76" width="2.51" height="1.90" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="14.05" y="10.12" width="2.51" height="1.54" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="16.56" y="10.56" width="2.51" height="1.11" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="19.06" y="10.77" width="2.51" height="0.89" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="21.57" y="10.73" width="2.51" height="0.93" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="24.08" y="10.94" width="2.51" height="0.73" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="26.58" y="10.87" width="2.51" height="0.79" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="29.09" y="11.08" width="2.51" height="0.58" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="31.59" y="10.94" width="2.51" height="0.72" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="34.10" y="10.94" width="2.51" height="0.72" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="36.60" y="10.95" width="2.51" height="0.71" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="39.11" y="10.68" width="2.51" height="0.98" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="41.61" y="10.67" width="2.51" height="1.00" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="44.12" y="11.32" width="2.51" height="0.34" style="stroke-width: 0.38; fill: #000000;"></rect></g></svg>
</td>
  </tr>
</tbody>
</table>

  ** Note to self: Need to add in base R commands to prep for Dr. Vercammen's code

We need to add an argument `type = categorical` to see summary statistics for non-numeric data.

```r
datasummary_skim(politics, type = "categorical")
```

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:left;">    </th>
   <th style="text-align:right;"> N </th>
   <th style="text-align:right;"> % </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> v2psnatpar_ord </td>
   <td style="text-align:left;"> Unified coalition control </td>
   <td style="text-align:right;"> 6448 </td>
   <td style="text-align:right;"> 23.7 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Divided party control </td>
   <td style="text-align:right;"> 4533 </td>
   <td style="text-align:right;"> 16.7 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Unified party control </td>
   <td style="text-align:right;"> 7762 </td>
   <td style="text-align:right;"> 28.5 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> v2x_regime </td>
   <td style="text-align:left;"> Closed Autocracy </td>
   <td style="text-align:right;"> 9635 </td>
   <td style="text-align:right;"> 35.4 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Electoral Autocracy </td>
   <td style="text-align:right;"> 4370 </td>
   <td style="text-align:right;"> 16.1 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Electoral Democracy </td>
   <td style="text-align:right;"> 2518 </td>
   <td style="text-align:right;"> 9.2 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Liberal Democracy </td>
   <td style="text-align:right;"> 2315 </td>
   <td style="text-align:right;"> 8.5 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> region </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 3509 </td>
   <td style="text-align:right;"> 12.9 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:right;"> 5070 </td>
   <td style="text-align:right;"> 18.6 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Americas </td>
   <td style="text-align:right;"> 4958 </td>
   <td style="text-align:right;"> 18.2 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Eastern Mediterranean </td>
   <td style="text-align:right;"> 3298 </td>
   <td style="text-align:right;"> 12.1 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Europe </td>
   <td style="text-align:right;"> 6552 </td>
   <td style="text-align:right;"> 24.1 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> South-East Asia </td>
   <td style="text-align:right;"> 1408 </td>
   <td style="text-align:right;"> 5.2 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Western Pacific </td>
   <td style="text-align:right;"> 2427 </td>
   <td style="text-align:right;"> 8.9 </td>
  </tr>
</tbody>
</table>

# Data Wrangling

  * Reshaping Data
  * Renaming columns
  * Changing data types
  * Creating new variables
  * Filtering observations
  * Selecting columns/variables
  * String manipulation
  * Joining/Merging datasets (need to modify to just use merge())

## Reshaping Data


```r
carbon_long <- carbon %>%
  pivot_longer(cols = -country,
               names_to = "Year",
               values_to = "Emissions")

head(carbon_long)
```

```
## # A tibble: 6 x 3
##   country     Year  Emissions
##   <chr>       <chr>     <dbl>
## 1 Afghanistan 1751         NA
## 2 Afghanistan 1752         NA
## 3 Afghanistan 1753         NA
## 4 Afghanistan 1754         NA
## 5 Afghanistan 1755         NA
## 6 Afghanistan 1756         NA
```

```r
set.seed(2021)
carbon_long %>% 
  slice_sample(n = 10)
```

```
## # A tibble: 10 x 3
##    country             Year  Emissions
##    <chr>               <chr>     <dbl>
##  1 Liberia             1952       55  
##  2 Trinidad and Tobago 1778       NA  
##  3 Grenada             1804       NA  
##  4 Russia              1979  2090000  
##  5 Georgia             1869       16.4
##  6 Philippines         1873       NA  
##  7 Grenada             1922       NA  
##  8 Estonia             1969       NA  
##  9 France              1907   146000  
## 10 Moldova             1763       NA
```

## Renaming columns

An alternative to using the assignment operator is `%<>%`.


```r
carbon_long %<>%
	rename(year = Year,
		      emissions = Emissions)

set.seed(2021)
carbon_long %>% 
  slice_sample(n = 10)
```

```
## # A tibble: 10 x 3
##    country             year  emissions
##    <chr>               <chr>     <dbl>
##  1 Liberia             1952       55  
##  2 Trinidad and Tobago 1778       NA  
##  3 Grenada             1804       NA  
##  4 Russia              1979  2090000  
##  5 Georgia             1869       16.4
##  6 Philippines         1873       NA  
##  7 Grenada             1922       NA  
##  8 Estonia             1969       NA  
##  9 France              1907   146000  
## 10 Moldova             1763       NA
```


## Changing Data types 


```r
#glimpse(carbon_long)
str(carbon_long)
```

```
## tibble [50,688 x 3] (S3: tbl_df/tbl/data.frame)
##  $ country  : chr [1:50688] "Afghanistan" "Afghanistan" "Afghanistan" "Afghanistan" ...
##  $ year     : chr [1:50688] "1751" "1752" "1753" "1754" ...
##  $ emissions: num [1:50688] NA NA NA NA NA NA NA NA NA NA ...
```

```r
carbon_long %<>% mutate(year = as.numeric(year))

str(carbon_long)
```

```
## tibble [50,688 x 3] (S3: tbl_df/tbl/data.frame)
##  $ country  : chr [1:50688] "Afghanistan" "Afghanistan" "Afghanistan" "Afghanistan" ...
##  $ year     : num [1:50688] 1751 1752 1753 1754 1755 ...
##  $ emissions: num [1:50688] NA NA NA NA NA NA NA NA NA NA ...
```

## Creating new variables


```r
# create a new column that contains the average of that country's emissions 
sample <- carbon_long %>%
  group_by(country) %>%
  mutate(avg_emissions = mean(emissions, na.rm = T))

set.seed(2021)
sample %>% 
  slice_sample(n = 10)
```

```
## # A tibble: 1,920 x 4
## # Groups:   country [192]
##    country      year emissions avg_emissions
##    <chr>       <dbl>     <dbl>         <dbl>
##  1 Afghanistan  1916        NA         2174.
##  2 Afghanistan  1981      1980         2174.
##  3 Afghanistan  1820        NA         2174.
##  4 Afghanistan  1942        NA         2174.
##  5 Afghanistan  2001       818         2174.
##  6 Afghanistan  1852        NA         2174.
##  7 Afghanistan  1860        NA         2174.
##  8 Afghanistan  1853        NA         2174.
##  9 Afghanistan  1855        NA         2174.
## 10 Afghanistan  1773        NA         2174.
## # ... with 1,910 more rows
```

## Filtering observations


```r
canada <- carbon_long %>% filter(country == "Canada") %>%
  drop_na(emissions)
```

## Tidying the gdp and energy data

Practice tidying the gdp data

```r
# glimpse(gdp)
head(gdp)
```

```
## # A tibble: 6 x 220
##   country           `1801`   `1802`   `1803`   `1804`   `1805`   `1806`   `1807`
##   <chr>              <dbl>    <dbl>    <dbl>    <dbl>    <dbl>    <dbl>    <dbl>
## 1 Afghanistan     NA       NA       NA       NA       NA       NA       NA      
## 2 Albania          0.104    0.104    0.104    0.104    0.104    0.104    0.104  
## 3 Algeria         -0.00247 -0.00247 -0.00247 -0.00247 -0.00247 -0.00247 -0.00247
## 4 Andorra          0.166    0.166    0.166    0.166    0.166    0.166    0.166  
## 5 Angola           0.425    0.425    0.425    0.425    0.425    0.425    0.425  
## 6 Antigua and Ba~ NA       NA       NA       NA       NA       NA       NA      
## # ... with 212 more variables: 1808 <dbl>, 1809 <dbl>, 1810 <dbl>, 1811 <dbl>,
## #   1812 <dbl>, 1813 <dbl>, 1814 <dbl>, 1815 <dbl>, 1816 <dbl>, 1817 <dbl>,
## #   1818 <dbl>, 1819 <dbl>, 1820 <dbl>, 1821 <dbl>, 1822 <dbl>, 1823 <dbl>,
## #   1824 <dbl>, 1825 <dbl>, 1826 <dbl>, 1827 <dbl>, 1828 <dbl>, 1829 <dbl>,
## #   1830 <dbl>, 1831 <dbl>, 1832 <dbl>, 1833 <dbl>, 1834 <dbl>, 1835 <dbl>,
## #   1836 <dbl>, 1837 <dbl>, 1838 <dbl>, 1839 <dbl>, 1840 <dbl>, 1841 <dbl>,
## #   1842 <dbl>, 1843 <dbl>, 1844 <dbl>, 1845 <dbl>, 1846 <dbl>, 1847 <dbl>,
## #   1848 <dbl>, 1849 <dbl>, 1850 <dbl>, 1851 <dbl>, 1852 <dbl>, 1853 <dbl>,
## #   1854 <dbl>, 1855 <dbl>, 1856 <dbl>, 1857 <dbl>, 1858 <dbl>, 1859 <dbl>,
## #   1860 <dbl>, 1861 <dbl>, 1862 <dbl>, 1863 <dbl>, 1864 <dbl>, 1865 <dbl>,
## #   1866 <dbl>, 1867 <dbl>, 1868 <dbl>, 1869 <dbl>, 1870 <dbl>, 1871 <dbl>,
## #   1872 <dbl>, 1873 <dbl>, 1874 <dbl>, 1875 <dbl>, 1876 <dbl>, 1877 <dbl>,
## #   1878 <dbl>, 1879 <dbl>, 1880 <dbl>, 1881 <dbl>, 1882 <dbl>, 1883 <dbl>,
## #   1884 <dbl>, 1885 <dbl>, 1886 <dbl>, 1887 <dbl>, 1888 <dbl>, 1889 <dbl>,
## #   1890 <dbl>, 1891 <dbl>, 1892 <dbl>, 1893 <dbl>, 1894 <dbl>, 1895 <dbl>,
## #   1896 <dbl>, 1897 <dbl>, 1898 <dbl>, 1899 <dbl>, 1900 <dbl>, 1901 <dbl>,
## #   1902 <dbl>, 1903 <dbl>, 1904 <dbl>, 1905 <dbl>, 1906 <dbl>, 1907 <dbl>, ...
```

```r
# str(gdp)

gdp_long <- gdp %>% 
  pivot_longer(cols = -country,
               names_to = "year",
               values_to = "gdp") %>%
  mutate(year = as.numeric(year))

head(gdp)
```

```
## # A tibble: 6 x 220
##   country           `1801`   `1802`   `1803`   `1804`   `1805`   `1806`   `1807`
##   <chr>              <dbl>    <dbl>    <dbl>    <dbl>    <dbl>    <dbl>    <dbl>
## 1 Afghanistan     NA       NA       NA       NA       NA       NA       NA      
## 2 Albania          0.104    0.104    0.104    0.104    0.104    0.104    0.104  
## 3 Algeria         -0.00247 -0.00247 -0.00247 -0.00247 -0.00247 -0.00247 -0.00247
## 4 Andorra          0.166    0.166    0.166    0.166    0.166    0.166    0.166  
## 5 Angola           0.425    0.425    0.425    0.425    0.425    0.425    0.425  
## 6 Antigua and Ba~ NA       NA       NA       NA       NA       NA       NA      
## # ... with 212 more variables: 1808 <dbl>, 1809 <dbl>, 1810 <dbl>, 1811 <dbl>,
## #   1812 <dbl>, 1813 <dbl>, 1814 <dbl>, 1815 <dbl>, 1816 <dbl>, 1817 <dbl>,
## #   1818 <dbl>, 1819 <dbl>, 1820 <dbl>, 1821 <dbl>, 1822 <dbl>, 1823 <dbl>,
## #   1824 <dbl>, 1825 <dbl>, 1826 <dbl>, 1827 <dbl>, 1828 <dbl>, 1829 <dbl>,
## #   1830 <dbl>, 1831 <dbl>, 1832 <dbl>, 1833 <dbl>, 1834 <dbl>, 1835 <dbl>,
## #   1836 <dbl>, 1837 <dbl>, 1838 <dbl>, 1839 <dbl>, 1840 <dbl>, 1841 <dbl>,
## #   1842 <dbl>, 1843 <dbl>, 1844 <dbl>, 1845 <dbl>, 1846 <dbl>, 1847 <dbl>,
## #   1848 <dbl>, 1849 <dbl>, 1850 <dbl>, 1851 <dbl>, 1852 <dbl>, 1853 <dbl>,
## #   1854 <dbl>, 1855 <dbl>, 1856 <dbl>, 1857 <dbl>, 1858 <dbl>, 1859 <dbl>,
## #   1860 <dbl>, 1861 <dbl>, 1862 <dbl>, 1863 <dbl>, 1864 <dbl>, 1865 <dbl>,
## #   1866 <dbl>, 1867 <dbl>, 1868 <dbl>, 1869 <dbl>, 1870 <dbl>, 1871 <dbl>,
## #   1872 <dbl>, 1873 <dbl>, 1874 <dbl>, 1875 <dbl>, 1876 <dbl>, 1877 <dbl>,
## #   1878 <dbl>, 1879 <dbl>, 1880 <dbl>, 1881 <dbl>, 1882 <dbl>, 1883 <dbl>,
## #   1884 <dbl>, 1885 <dbl>, 1886 <dbl>, 1887 <dbl>, 1888 <dbl>, 1889 <dbl>,
## #   1890 <dbl>, 1891 <dbl>, 1892 <dbl>, 1893 <dbl>, 1894 <dbl>, 1895 <dbl>,
## #   1896 <dbl>, 1897 <dbl>, 1898 <dbl>, 1899 <dbl>, 1900 <dbl>, 1901 <dbl>,
## #   1902 <dbl>, 1903 <dbl>, 1904 <dbl>, 1905 <dbl>, 1906 <dbl>, 1907 <dbl>, ...
```

```r
# str(gdp)

# glimpse(energy)
head(energy)
```

```
## # A tibble: 6 x 57
##   country  `1960` `1961` `1962` `1963` `1964` `1965` `1966` `1967` `1968` `1969`
##   <chr>     <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
## 1 Albania      NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 2 Algeria      NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 3 Angola       NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 4 Antigua~     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 5 Argenti~     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 6 Armenia      NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## # ... with 46 more variables: 1970 <dbl>, 1971 <dbl>, 1972 <dbl>, 1973 <dbl>,
## #   1974 <dbl>, 1975 <dbl>, 1976 <dbl>, 1977 <dbl>, 1978 <dbl>, 1979 <dbl>,
## #   1980 <dbl>, 1981 <dbl>, 1982 <dbl>, 1983 <dbl>, 1984 <dbl>, 1985 <dbl>,
## #   1986 <dbl>, 1987 <dbl>, 1988 <dbl>, 1989 <dbl>, 1990 <dbl>, 1991 <dbl>,
## #   1992 <dbl>, 1993 <dbl>, 1994 <dbl>, 1995 <dbl>, 1996 <dbl>, 1997 <dbl>,
## #   1998 <dbl>, 1999 <dbl>, 2000 <dbl>, 2001 <dbl>, 2002 <dbl>, 2003 <dbl>,
## #   2004 <dbl>, 2005 <dbl>, 2006 <dbl>, 2007 <dbl>, 2008 <dbl>, 2009 <dbl>,
## #   2010 <dbl>, 2011 <dbl>, 2012 <dbl>, 2013 <dbl>, 2014 <dbl>, 2015 <dbl>
```

```r
energy_long <- energy %>%
  pivot_longer(cols = -country,
               names_to = "year", 
               values_to = "energy") %>%
  mutate(year = as.numeric(year))
head(energy)
```

```
## # A tibble: 6 x 57
##   country  `1960` `1961` `1962` `1963` `1964` `1965` `1966` `1967` `1968` `1969`
##   <chr>     <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
## 1 Albania      NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 2 Algeria      NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 3 Angola       NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 4 Antigua~     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 5 Argenti~     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 6 Armenia      NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## # ... with 46 more variables: 1970 <dbl>, 1971 <dbl>, 1972 <dbl>, 1973 <dbl>,
## #   1974 <dbl>, 1975 <dbl>, 1976 <dbl>, 1977 <dbl>, 1978 <dbl>, 1979 <dbl>,
## #   1980 <dbl>, 1981 <dbl>, 1982 <dbl>, 1983 <dbl>, 1984 <dbl>, 1985 <dbl>,
## #   1986 <dbl>, 1987 <dbl>, 1988 <dbl>, 1989 <dbl>, 1990 <dbl>, 1991 <dbl>,
## #   1992 <dbl>, 1993 <dbl>, 1994 <dbl>, 1995 <dbl>, 1996 <dbl>, 1997 <dbl>,
## #   1998 <dbl>, 1999 <dbl>, 2000 <dbl>, 2001 <dbl>, 2002 <dbl>, 2003 <dbl>,
## #   2004 <dbl>, 2005 <dbl>, 2006 <dbl>, 2007 <dbl>, 2008 <dbl>, 2009 <dbl>,
## #   2010 <dbl>, 2011 <dbl>, 2012 <dbl>, 2013 <dbl>, 2014 <dbl>, 2015 <dbl>
```

## Selecting columns based on text


```r
names(disasters)
```

```
##  [1] "Year"                      "Drought Count"            
##  [3] "Drought Cost"              "Drought Lower 75"         
##  [5] "Drought Upper 75"          "Drought Lower 90"         
##  [7] "Drought Upper 90"          "Drought Lower 95"         
##  [9] "Drought Upper 95"          "Flooding Count"           
## [11] "Flooding Cost"             "Flooding Lower 75"        
## [13] "Flooding Upper 75"         "Flooding Lower 90"        
## [15] "Flooding Upper 90"         "Flooding Lower 95"        
## [17] "Flooding Upper 95"         "Freeze Count"             
## [19] "Freeze Cost"               "Freeze Lower 75"          
## [21] "Freeze Upper 75"           "Freeze Lower 90"          
## [23] "Freeze Upper 90"           "Freeze Lower 95"          
## [25] "Freeze Upper 95"           "Severe Storm Count"       
## [27] "Severe Storm Cost"         "Severe Storm Lower 75"    
## [29] "Severe Storm Upper 75"     "Severe Storm Lower 90"    
## [31] "Severe Storm Upper 90"     "Severe Storm Lower 95"    
## [33] "Severe Storm Upper 95"     "Tropical Cyclone Count"   
## [35] "Tropical Cyclone Cost"     "Tropical Cyclone Lower 75"
## [37] "Tropical Cyclone Upper 75" "Tropical Cyclone Lower 90"
## [39] "Tropical Cyclone Upper 90" "Tropical Cyclone Lower 95"
## [41] "Tropical Cyclone Upper 95" "Wildfire Count"           
## [43] "Wildfire Cost"             "Wildfire Lower 75"        
## [45] "Wildfire Upper 75"         "Wildfire Lower 90"        
## [47] "Wildfire Upper 90"         "Wildfire Lower 95"        
## [49] "Wildfire Upper 95"         "Winter Storm Count"       
## [51] "Winter Storm Cost"         "Winter Storm Lower 75"    
## [53] "Winter Storm Upper 75"     "Winter Storm Lower 90"    
## [55] "Winter Storm Upper 90"     "Winter Storm Lower 95"    
## [57] "Winter Storm Upper 95"
```

```r
# Overwrite the disasters dataset to only contain Year and columns with the text 'count' in them

disasters %<>% select(Year, contains("Count"))
names(disasters)
```

```
## [1] "Year"                   "Drought Count"          "Flooding Count"        
## [4] "Freeze Count"           "Severe Storm Count"     "Tropical Cyclone Count"
## [7] "Wildfire Count"         "Winter Storm Count"
```

```r
disasters %<>% rename(year = Year,
                      drought = `Drought Count`,
                      flooding = `Flooding Count`,
                      freeze = `Freeze Count`,
                      severe_storm = `Severe Storm Count`,
                      tropical_cyclone = `Tropical Cyclone Count`,
                      wildfire_count = `Wildfire Count`,
                      winter_storm = `Winter Storm Count`)
names(disasters)
```

```
## [1] "year"             "drought"          "flooding"         "freeze"          
## [5] "severe_storm"     "tropical_cyclone" "wildfire_count"   "winter_storm"
```

## Creating a total count of disasters 


```r
# create a new variable that contains the total number of disasters per year
disasters %<>%
 mutate(total_disasters = rowSums(select(., -year)))
```

## Select only the total_disasters column and create a new variable country to indicate this data is from USA


```r
disasters %<>%
  mutate(country = "United States") %>%
  select(year, country, total_disasters) 

disasters %>%
  slice_head(n = 5)
```

```
## # A tibble: 5 x 3
##    year country       total_disasters
##   <dbl> <chr>                   <dbl>
## 1  1980 United States               3
## 2  1981 United States               2
## 3  1982 United States               3
## 4  1983 United States               5
## 5  1984 United States               2
```

## String manipulation

We will now clean the temperature data


```r
# glimpse(temp)

# If we look at the 'Date' variable of the temperature data, we see that it is 6 digits and somehow all end with 12 
# let's first check whether the length of each data entry is indeed 6
str_length(temp$Date)
```

```
##   [1] 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6
##  [38] 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6
##  [75] 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6
## [112] 6 6 6 6 6 6 6 6 6 6 6 6 6 6
```

```r
# let's now check whether the last 2 characters of each Date entry is indeed 12
str_ends(temp$Date, pattern = "12")
```

```
##   [1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
##  [16] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
##  [31] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
##  [46] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
##  [61] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
##  [76] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
##  [91] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
## [106] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
## [121] TRUE TRUE TRUE TRUE TRUE
```

```r
# Now we will overwrite the Date variable to only get the first 4 digits
temp %<>% mutate(Date = str_sub(Date, start = 1, end = 4))
head(temp)
```

```
## # A tibble: 6 x 3
##   Date  Value Anomaly
##   <chr> <dbl>   <dbl>
## 1 1895   50.3   -1.68
## 2 1896   52.0   -0.03
## 3 1897   51.6   -0.46
## 4 1898   51.4   -0.59
## 5 1899   51.0   -1.01
## 6 1900   52.8    0.75
```

## Tidy temperature dataset

We will (a) drop the Anomaly variable, (b) rename Date to year, (c) make year numeric) (c) rename Value to temp, (d) add a country variable 


```r
temp %<>% 
  rename(temp = Value) %>%
  mutate(year = as.numeric(Date),
         country = "United States") %>%
  select(year, country, temp)

head(temp)
```

```
## # A tibble: 6 x 3
##    year country        temp
##   <dbl> <chr>         <dbl>
## 1  1895 United States  50.3
## 2  1896 United States  52.0
## 3  1897 United States  51.6
## 4  1898 United States  51.4
## 5  1899 United States  51.0
## 6  1900 United States  52.8
```

## Joining data - full join


```r
data <- carbon_long %>%
  full_join(gdp_long, by = c("country", "year")) %>%
  full_join(energy_long, by = c("country", "year")) 

tail(data)
```

```
## # A tibble: 6 x 5
##   country   year emissions   gdp energy
##   <chr>    <dbl>     <dbl> <dbl>  <dbl>
## 1 Zambia    2019        NA  2.6      NA
## 2 Zimbabwe  2015        NA  3.33     NA
## 3 Zimbabwe  2016        NA  3.67     NA
## 4 Zimbabwe  2017        NA  2.98     NA
## 5 Zimbabwe  2018        NA  2.87     NA
## 6 Zimbabwe  2019        NA  2.87     NA
```

```r
# 
# data_long <- data %>%
#   pivot_longer(cols = c(-country, -year),
#                names_to = "indicator", 
#                values_to = "value")
# 
# head(data_long)
```

### Some more cleaning


```r
data %<>% mutate(region = case_when(country == "United States" ~ "United States",
                                    country != "United States" ~ "Rest of the World")) %>%
  drop_na()
```

## Data exploration


```r
data %>% filter(country == "United States") %>%
  summarize(first(emissions), first(year))
```

## Joining data - left join


```r
usa <- carbon_long %>%
  filter(country == "United States") %>%
  left_join(gdp_long, by = c("country", "year")) %>%
  left_join(energy_long, by = c("country", "year")) %>%
  full_join(temp, by = c("country", "year")) %>%
  full_join(disasters, by = c("country", "year"))
```

# Data Visualization

## Basic R

```r
plot(data$year, data$emissions)
```

![](intro-to-r_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

## ggpplot

How has carbon emissions changed over time? 

```r
data %>% group_by(year) %>%
  summarize(total_emissions = sum(emissions)) %>%
  ggplot(aes(x = year, y = total_emissions)) +
  geom_line(size = 1.5)
```

![](intro-to-r_files/figure-html/unnamed-chunk-20-1.png)<!-- -->


```r
data %>% group_by(year) %>%
  summarize(total_emissions = sum(emissions)) %>%
  ggplot(aes(x = year, y = total_emissions)) +
  geom_line(size = 1.5) + 
  labs(title = "World" ~CO[2]~ "Emissions",
       caption = "Limited to reporting countries only",
       y = "Emissions (metric tonnes)") +
  theme_classic()
```

![](intro-to-r_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

Are certain countries contribute more emissions than others? 

```r
data %>% 
  ggplot(aes(x = year, y = emissions, group = country)) +
  geom_line(size = 1) +
  theme_classic()
```

![](intro-to-r_files/figure-html/unnamed-chunk-22-1.png)<!-- -->

Change the transparency of the lines due to overlapping lines

```r
data %>% 
  ggplot(aes(x = year, y = emissions, group = country)) +
  geom_line(size = 1, 
            alpha = 0.4) +
  theme_classic()
```

![](intro-to-r_files/figure-html/unnamed-chunk-23-1.png)<!-- -->

Compare USA to the rest of the world

```r
data %>% 
  ggplot(aes(x = year, y = emissions, group = country, color = region)) +
  geom_line(size = 1) +
  scale_colour_manual(values = c("grey", "black")) +
  theme_classic()
```

![](intro-to-r_files/figure-html/unnamed-chunk-24-1.png)<!-- -->

## Visualize carbon emissions over the years for the top 10 countries with the highest emissions in 2014


```r
# Create a new object to identify the 10 countries with highest emissions in 2014
top10countries <- data %>%
  filter(year == 2014) %>%
  mutate(rank = dense_rank(desc(emissions))) %>%
  filter(rank <= 10) %>%
  arrange(rank)

# filter original data for countries in top10
top10data <- data %>%
  filter(country %in% pull(top10countries, country)) 

ggplot(top10data, aes(x = year, y = emissions, color = country)) + 
  geom_line() + 
  theme_classic()
```

![](intro-to-r_files/figure-html/unnamed-chunk-25-1.png)<!-- -->

# Data Analysis

By the end of this section, the students will be able to:

* Calculate basic summary statistics
* Run hypothesis tests
* Run regressions
* Format regression results

## Summary Statistics

```r
usa %>% summarise(mean_emissions = mean(emissions, na.rm = T), 
                  mean_temp = mean(temp, na.rm = T),
                  sd_emissions = sd(emissions, na.rm = T),
                  sd_temp = sd(temp, na.rm = T))
```

```
## # A tibble: 1 x 4
##   mean_emissions mean_temp sd_emissions sd_temp
##            <dbl>     <dbl>        <dbl>   <dbl>
## 1       1748376.      52.2     1951916.   0.987
```

## Correlation

```r
usa <- usa %>% drop_na()

usa %>%
  summarize(r = cor(x = emissions,
                    y = temp,
                    method = "pearson")) %>%
  pull(r)
```

```
## [1] 0.4711717
```

```r
cortest <- cor.test(pull(usa, emissions), 
                    pull(usa, temp))

# see the structure of the data
# str(cortest)
cortest$statistic
```

```
##        t 
## 3.068649
```

```r
cortest$p.value
```

```
## [1] 0.004277393
```

```r
# tidy the results
tidy(cortest)
```

```
## # A tibble: 1 x 8
##   estimate statistic p.value parameter conf.low conf.high method     alternative
##      <dbl>     <dbl>   <dbl>     <int>    <dbl>     <dbl> <chr>      <chr>      
## 1    0.471      3.07 0.00428        33    0.164     0.695 Pearson's~ two.sided
```


```r
politics %<>% rename(country = country_name)
data2 <- data %>% full_join(politics, by =c("country", "year"))
data2$gdp2 <- data2$gdp * data2$gdp
```

## Regressions

```r
# run the regression

reg1 <- lm(emissions ~ gdp, data = usa)
# stargazer(reg1, type = 'html')
summary(reg1)
```

```
## 
## Call:
## lm(formula = emissions ~ gdp, data = usa)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -865769 -338037   14430  409582  651790 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  5154372      99767  51.664   <2e-16 ***
## gdp            -7588      39629  -0.191    0.849    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 457100 on 33 degrees of freedom
## Multiple R-squared:  0.00111,	Adjusted R-squared:  -0.02916 
## F-statistic: 0.03666 on 1 and 33 DF,  p-value: 0.8493
```

```r
reg2 <- lm(emissions ~ log(energy) + gdp + gdp2 +v2x_libdem, data = data2)
summary(reg2)
```

```
## 
## Call:
## lm(formula = emissions ~ log(energy) + gdp + gdp2 + v2x_libdem, 
##     data = data2)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
##  -390392  -124870   -76765   -18701 10071571 
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -354595.68   45110.82  -7.861 4.59e-15 ***
## log(energy)   73092.41    6759.05  10.814  < 2e-16 ***
## gdp            3757.54    1017.34   3.693 0.000223 ***
## gdp2            -40.82      16.70  -2.444 0.014543 *  
## v2x_libdem   -84982.83   24451.62  -3.476 0.000514 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 465300 on 5368 degrees of freedom
##   (22285 observations deleted due to missingness)
## Multiple R-squared:  0.02394,	Adjusted R-squared:  0.02321 
## F-statistic: 32.92 on 4 and 5368 DF,  p-value: < 2.2e-16
```


```r
sum(residuals(reg2)^2)
```

```
## [1] 1.162e+15
```

```r
anova(reg2)
```

```
## Analysis of Variance Table
## 
## Response: emissions
##               Df     Sum Sq    Mean Sq  F value    Pr(>F)    
## log(energy)    1 2.3141e+13 2.3141e+13 106.9034 < 2.2e-16 ***
## gdp            1 1.7958e+12 1.7958e+12   8.2958 0.0039894 ** 
## gdp2           1 9.5075e+11 9.5075e+11   4.3921 0.0361521 *  
## v2x_libdem     1 2.6148e+12 2.6148e+12  12.0794 0.0005138 ***
## Residuals   5368 1.1620e+15 2.1647e+11                       
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```
