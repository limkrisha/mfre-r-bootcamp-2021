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
pacman::p_load(tidyverse, readxl, googlesheets4, readstata13, magrittr, cansim, stats, broom, modelsummary)
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

You will likely have to import data into R for your MFRE courses, instead of typing the data by hand. In this section, you will learn how to load data from 5 different sources: 

 * csv
 * xlsx
 * Stata data format (dta)
 * Google Sheet
 * API (Statistics Canada)

## csv

It's common for people to save data in spreadsheets as comma-separated values (csv). To open a csv file in R, we use the `read_csv()` function of the `{tidyverse}` package, and the `here()` function of the `{here}` package. The `here()` function takes the folder and file names as inputs, which are enclosed by quotation marks and you wouldn't have to worry about to use back or forward slashes. 

The code below shows that we are reading the file "yearly_co2_emissions.csv" saved in our data folder and assigning it to the data object called `carbon`. Assigning data to objects is one of the big difference between R and Stata, as R allows you to work with multiple data sets at once. 


```r
carbon <- read_csv(here("data", "yearly_co2_emissions.csv"))
```

  * If you don't use the `here()` function, you have to specify the full file path relative to the location of the R project (my working directory). For example, my R project folder is called 2021-r-bootcamp, and in it I have multiple folders including "code" (where my current script is saved) and "data" (where data files are saved). To import the carbon emissions file, I will have to type in `carbon <- read_csv("data/yearly_co2_emissions.csv")`
  
  * Note: The `read_csv` function assumes fields are delimited by comma. If you want a more flexible way of reading text files, look up the `{readr}` package for other [functions](https://readr.tidyverse.org/). 
  
  * In FRE 501, Dr. Vercammen uses the base R `read.csv` function. It is doing the same thing as the `read_csv` function. The additional arguments you will see are `header = TRUE` meaning that the first row will be treated as columns, and `sep = ","` meaning that the file is delimited by comma. 
  
## Excel xlsx format

Another way that spreadsheets are stored is in the Excel xlsx format. Sometimes, there are multiple sheets in one Excel file. The `read_xlsx()` function of the `{readxl}` package allows us to read files in xlsx format. 


```r
gdp <- read_xlsx(here("data", "gdp_per_capita_yearly_growth.xlsx"))
```

Sometimes there are multiple sheets in one spreadsheet. We use the argument `sheet = insert_sheet_number` to indicate the sheet we are importing. 


```r
energy_hist <- read_xlsx(here("data", "energy_use_per_person.xlsx"), sheet = 1)
energy_recent <- read_xlsx(here("data", "energy_use_per_person.xlsx"), sheet = 2)
```

If you look at the two imported sheets (either by typing `View(energy_hist)` or clicking the `energy_hist` object in the Environment tab in the upper right panel of RStudio), they contain data for the same countries. The only difference is that in the first sheet (`energy_hist`), the data is from 1960-1999, and the data in the second sheet (`energy_recent`) is from 2000-2015. We want to join `energy_hist` and `energy_recent`. There are many types of [joins](https://dplyr.tidyverse.org/reference/join.html), but for now we will do a full join, meaning the new data frame called `energy` will include all rows and columns in both `energy_hist` and `energy_recent`. The variable `country` is the common variable in both data frames that I used to link them together. 


```r
energy <- full_join(energy_hist, energy_recent, by = c("country"))
```

There are many ways to do the same thing in R. In FRE 501, Dr. Vercammen's uses the base R `merge` function. You can look at the full documentation for more details (`?merge`). The syntax is `merge(x, y)` where `x` and `y` are both the names of your dataframes. If you want to keep all observations in `x` and only merge `y` based on a common variable (`country`), I will use the `all.x = TRUE` argument. Since we want to do a full join (although the output would be the same anyway), we will use the `all.x = TRUE, all.y = TRUE` arguments to indicate that we want all rows in `x` and all rows in `y` in the output. 
  

```r
energy_basemerge <- merge(energy_hist, energy_recent, by = c("country"), all.x = TRUE, all.y = TRUE)
```

  * If you look at `energy` and `energy_basemerge` in your Environment tab, you will notice that both of them have the same dimensions. 
  
## Stata dta format

Because you will use Stata in some of your classes, you may read in a Stata data format into R as well. We will use the `read.dta13` of the `{readstata13}` package. This function allows you to read Stata file formats from version 17 and older. 


```r
politics <- read.dta13(here("data", "politics.dta"), nonint.factors = TRUE)
```

  * The argument `nonint.factors = TRUE` is to keep factor labels instead of the value itself. You can try loading the data with and without that argument to see the difference. 
  
It is also common now to share data using Google Sheets. We will use the `read_sheet("insert_link_here")` function of the `{googlesheets4}` package. This function will prompt you to provide authorization to the tidyverse API and log in with your Gmail credentials. One way to get around this step would be to add in the function `gs4_deauth()` before you use the `read_sheet()` function. 

## Google Sheets 


```r
gs4_deauth() # so no need to sign in
disasters <- read_sheet("https://docs.google.com/spreadsheets/d/17s15o7jdDpGSKgsIboZdnYU2UxHtU9DHKNRmYVVgwJo/edit#gid=0", skip = 2) 
```
* When you open the Google sheet link in your browser, you will notice that the first two rows are the notes or the title of the table. The data actually starts in row 3. Because of this reason, we add the argument `skip = 2` to tell R to start reading the data from the third row.

## API - Statistics Canada

When you look for data online, you usually have the option to download the tables as a csv or xlsx file. You may also want to check online if someone has already written a package that connects to the data's server directly and load it into R. The advantage of doing this approach is that you can work with 'updated' data whenever you run the code, and you also save yourself time from having to save the table as a spreadsheet every time the table is updated. 

For exapmle, you want to download the Estimated areas, yield, production, average farm price, and total farm value of principal field crops table from [Statistics Canada](https://www150.statcan.gc.ca/t1/tbl1/en/tv.action?pid=3210035901). A quick Google Search of "import stats can table in R" will reveal that someone already wrote the `{cansim}` package to do just that. The function we will use is called `get_cansim('insert_table_number_here'_)`. 

```r
ag <- get_cansim('32-10-0359-01')
```

There are other many other ways to import data into R, but these are the ones you will most likely need to know for your MFRE courses. 

# Data Exploration

You can view your data with the command `View(dataframe_name)` or click the data object in the Environment tab in the upper right panel of RStudio. These two actions will open a new tab in the upper left panel of RStudio. If you want to see your data in the console, simply type in the name of the data object into your console. 

Here are a few functions to inspect your data. We will use the  `politics` data as an example. 

Size: 
  * `dim(politics)` returns a vector with the number of rows in the first element, and the number of columns in the second element
  * `nrow(politics)` returns the number of rows
  * `ncol(politics)` returns the number of columns 
  
Content:
  * `head(politics)` returns the first 5 observations 
  * `tail(politics)` returns the last 5 observations
  * `politics %>% slice_head(n = 6)` returns the first 6 observations 
  * `politics %>% slice_tail(n = 7)` returns the last 7 observations
  * `politics %>% slice_sample(n = 10)` returns 10 observations selected at random 
  
Column Names:
  * `names(politics)` returns the column names 
  
Summary:
  * `str(politics)` shows the structure of the object and information about the class, length, and content of each column
  * `summary(politics)` returns the summary statistics of each column
  * `glimpse(politics)` returns the dimension of the data, the names and class of each column, and previous as many values per column. 

## Indexing and Subsetting Data Frames

To extract certain columns and rows from our data, we use `[]` or `[[]]` or `$` symbols. 
  
  * `politics[1, 2]` To extract the first element in the second column of the table as a vector
  * `politics[[1]]` To extract the first column as a vector 
  * `politics[3,]` To extract the third row of the table as a data frame
  * `politics[,3]` To extract the third column of the table as a data frame
  * `politics[, -1]` To extract all the columns except the first column as a data frame
    * `politics[c(5:10), ]` To extract rows to 5 and all of the columns as a data frame. If you do not include a comma, you will get an error. By not specifying a value after the comma, you indicate that you want all the columns. 
  * `politics[, c(3:4)]` To extract the third and fourth columns as a data frame. Notice that there is a comma in front to indicate that we want all the rows. 
  * `politics["country_name"]` To extract the `country_name` column as a data frame 
    * `politics$country_name` To extract the `country_name` column as a vector

As you can see, there are so many different ways to extract values. The most common way we extract values in MFRE classes would be last point, `politics$country_name`.   

## Shoud I add something about factors?

# Data Wrangling

  * Selecting columns/variables and filtering rows
  * Reshaping Data
  * Renaming columns
  * Changing data types
  * Creating new variables
  * Filtering observations
  * Selecting columns/variables
  * String manipulation
  * Joining/Merging datasets (need to modify to just use merge())

We will only work with 3 data frames: `carbon`, `politics`, and `gdp`. 

There are many packages you can use to wrangle data. In this case study, I will mostly use packages in the `{tidyverse}` library, but I will also show you alternatives that you might encounter in some MFRE courses. 

You will see that I will use the pipe operator `%>%` frequently. The operator allows us to chain functions together. It takes the function specified to the left of the operator and allows you to pass the intermediate output to the function specified to the right of the operator. You can read more about it [here](https://www.datacamp.com/community/tutorials/pipe-r-tutorial).

## Selecting columns/variables and filtering rows 

In the `politics` data frame, let's say I only want to keep the `country_name`, `year`, `v2x_libdem`, `v2x_regime`, and `region` variables. We will use the `select(col1, col2, ...)` function. 


```r
politics %>% select(country_name, year, v2x_libdem, v2x_regime, region)
```

Alternatively, we can also drop the columns we do not want by adding a `-` symbol before the column name/s. 


```r
politics %>% select(-country_text_id, -v2psnatpar_ord)
```

The output from the two codes above is a dataframe of all the rows of the columns we selected But the original data `politics` remains unchanged. To "save" this new dataframe, we have to use the assignment operator `<-`. In the code below, we will just overwrite the original `politics` data.


```r
politics <- politics %>% select(country_name, year, v2x_libdem, v2x_regime, region)
```

Running the command `dim(politics)`, you will notice that we only have 5 columns left in our dataframe. 

If we want to keep observations to those from years 1991 onwards, we use the `filter()` function. 


```r
politics <- politics %>% filter(year > 1991)

# To filter observations where region = Africa
# politics %>% filter(region == "Africa") 
```

We can use the pipe operator to chain the `select()` and `filter()` functions. Running this code will not change anything anymore because we already used the assignment operator. But it may be something you may want to do in the future and save yourself a few lines of code. 


```r
politics <- politics %>% 
  select(country_name, year, v2x_libdem, v2x_regime, region) %>%
  filter(year > 1991)
```

## Renaming columns

Let's say we want to rename the `country_name` column to `country`, `v2x_libdem` to `democracy`, and `v2x_regime` to regime. The function is `rename(new_name = old_name)`.


```r
politics <- politics %>% 
  rename(country = country_name,
         democracy = v2x_libdem,
         regime = v2x_regime)
```

  * In Dr. Vercammen's code, you will see that he renames variables using the `names()` and `which()` functions -- `names(politics)[which(names(politics) == "country")] = "country_name"`. 
  
## Reshaping Data

### Pivoting wider

In the `politics` data, each row contains the values of variables associated with each country and year. This dataframe is said to be in the "long" data format. 

If you look at the `carbon` and `gdp` dataframes, you will notice that they are in the wide format. Each row is a country, and the columns are the different years we have observations for. 

Most R functions expect your data to be in the "long" data format because it is more machine readable and is closer to the formatting of databases. In the `tidyr` package, we can use the `pivot_longer()` and `pivot_wider` columns to reshape our data. 


```r
carbon %>% 
  slice_sample(n = 3)
```

```
## # A tibble: 3 x 265
##   country  `1751` `1752` `1753` `1754` `1755` `1756` `1757` `1758` `1759` `1760`
##   <chr>     <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
## 1 Maurita~     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 2 Cambodia     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 3 Singapo~     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## # ... with 254 more variables: 1761 <dbl>, 1762 <dbl>, 1763 <dbl>, 1764 <dbl>,
## #   1765 <dbl>, 1766 <dbl>, 1767 <dbl>, 1768 <dbl>, 1769 <dbl>, 1770 <dbl>,
## #   1771 <dbl>, 1772 <dbl>, 1773 <dbl>, 1774 <dbl>, 1775 <dbl>, 1776 <dbl>,
## #   1777 <dbl>, 1778 <dbl>, 1779 <dbl>, 1780 <dbl>, 1781 <dbl>, 1782 <dbl>,
## #   1783 <dbl>, 1784 <dbl>, 1785 <dbl>, 1786 <dbl>, 1787 <dbl>, 1788 <dbl>,
## #   1789 <dbl>, 1790 <dbl>, 1791 <dbl>, 1792 <dbl>, 1793 <dbl>, 1794 <dbl>,
## #   1795 <dbl>, 1796 <dbl>, 1797 <dbl>, 1798 <dbl>, 1799 <dbl>, 1800 <dbl>,
## #   1801 <dbl>, 1802 <dbl>, 1803 <dbl>, 1804 <dbl>, 1805 <dbl>, 1806 <dbl>,
## #   1807 <dbl>, 1808 <dbl>, 1809 <dbl>, 1810 <dbl>, 1811 <dbl>, 1812 <dbl>,
## #   1813 <dbl>, 1814 <dbl>, 1815 <dbl>, 1816 <dbl>, 1817 <dbl>, 1818 <dbl>,
## #   1819 <dbl>, 1820 <dbl>, 1821 <dbl>, 1822 <dbl>, 1823 <dbl>, 1824 <dbl>,
## #   1825 <dbl>, 1826 <dbl>, 1827 <dbl>, 1828 <dbl>, 1829 <dbl>, 1830 <dbl>,
## #   1831 <dbl>, 1832 <dbl>, 1833 <dbl>, 1834 <dbl>, 1835 <dbl>, 1836 <dbl>,
## #   1837 <dbl>, 1838 <dbl>, 1839 <dbl>, 1840 <dbl>, 1841 <dbl>, 1842 <dbl>,
## #   1843 <dbl>, 1844 <dbl>, 1845 <dbl>, 1846 <dbl>, 1847 <dbl>, 1848 <dbl>,
## #   1849 <dbl>, 1850 <dbl>, 1851 <dbl>, 1852 <dbl>, 1853 <dbl>, 1854 <dbl>,
## #   1855 <dbl>, 1856 <dbl>, 1857 <dbl>, 1858 <dbl>, 1859 <dbl>, 1860 <dbl>, ...
```

  * *Note*: Something else you might notice is that the column names start with a number. If you create your own dataframe in the future, I strongly recommend that you DO NOT use numbers as the first character of your variable name. To refer to the last column of the `carbon` data, for example, whose variable/column name is 2014, you will have to wrap the column name with a backtick. 



```r
head(carbon$`2014`)
```

```
## [1]   9810   5720 145000    462  34800    532
```

### Pivoting longer

The `pivot_longer()` function takes four main arguments:
  * the (wide) data
  * *cols* are the names of the columns we use to fill the new values variable (or to drop)
  * the *names_to* column variable we wish to create the *cols* provided
  * the *values_to* column variable we wish to create and fill with values associated with the *cols* provided
  
Let's now reshape the `carbon` data longer to create new columns for `year` and `emissions`. 

To create a long format of the `carbon` data, we use the following
  * the (wide) data - `carbon` 
  * *cols* include all columns except the country variable, so we can just put `-country`. Usually you can put the column names of interest as in `cols = col1:col5` or `cols = c(col1, col2, col5)`, but because our variable names start as numbers, it is not allowing us to use this format. 
  * the *names_to* column variable will be a character string of the name the column names these columns (1762:2014) will collapse into - `"year"`
  * the *values_to* column variable will be a character string of the name of the column the values of the collapsed columns will be inserted into - `"emissions"`
  

```r
carbon_long <- carbon %>%
  pivot_longer(cols = -country,
               names_to = "year",
               values_to = "emissions")
```

Now we can take a look at the long data format. 


```r
head(carbon_long)
```

```
## # A tibble: 6 x 3
##   country     year  emissions
##   <chr>       <chr>     <dbl>
## 1 Afghanistan 1751         NA
## 2 Afghanistan 1752         NA
## 3 Afghanistan 1753         NA
## 4 Afghanistan 1754         NA
## 5 Afghanistan 1755         NA
## 6 Afghanistan 1756         NA
```

```r
# To always get the same random sample, use set.seed(number)
# set.seed(2021)
carbon_long %>% 
  slice_sample(n = 10)
```

```
## # A tibble: 10 x 3
##    country             year  emissions
##    <chr>               <chr>     <dbl>
##  1 Denmark             1760      NA   
##  2 Iceland             1972    1490   
##  3 Sweden              1779      NA   
##  4 Belgium             2004  111000   
##  5 South Korea         1959   11200   
##  6 Romania             1861       7.33
##  7 Germany             1805    1040   
##  8 St. Kitts and Nevis 1836      NA   
##  9 North Korea         2002   69200   
## 10 Argentina           1866      NA
```

```r
str(carbon_long)
```

```
## tibble [50,688 x 3] (S3: tbl_df/tbl/data.frame)
##  $ country  : chr [1:50688] "Afghanistan" "Afghanistan" "Afghanistan" "Afghanistan" ...
##  $ year     : chr [1:50688] "1751" "1752" "1753" "1754" ...
##  $ emissions: num [1:50688] NA NA NA NA NA NA NA NA NA NA ...
```
Notice that the `year` variable is of the `character (chr)` type, so we want to recode that variable to numeric. To create or modify columns as a function of existing columns, we use the `mutate(new_var_name = function_of_existing_vars)` function. To change the data type to numeric, we use the `as.numeric()` function.  


```r
carbon_long <- carbon_long %>%
  mutate(year = as.numeric(year))
```

The `gdp` data is in the same format, so let's do reshape that one too.


```r
gdp_long <- gdp %>%
  pivot_longer(cols = -country, 
               names_to = "year",
               values_to = "gdp") %>%
  mutate(year = as.numeric(year))
head(gdp_long)
```

```
## # A tibble: 6 x 3
##   country      year   gdp
##   <chr>       <dbl> <dbl>
## 1 Afghanistan  1801    NA
## 2 Afghanistan  1802    NA
## 3 Afghanistan  1803    NA
## 4 Afghanistan  1804    NA
## 5 Afghanistan  1805    NA
## 6 Afghanistan  1806    NA
```


### Pivoting wider

In the case that we want to reshape the `carbon_long` back to its original "wide" data format, we can use the `pivot_wider()` function. This function takes three main arguments. 

  1. the data
  2. the *names_from* column whose values will become new column names
  3. the *values_from* column whose values will fill the new column variables. 
  

```r
carbon_wide <- carbon_long %>%
  pivot_wider(names_from = "year",
              values_from = "emissions")

head(carbon)
```

```
## # A tibble: 6 x 265
##   country  `1751` `1752` `1753` `1754` `1755` `1756` `1757` `1758` `1759` `1760`
##   <chr>     <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
## 1 Afghani~     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 2 Albania      NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 3 Algeria      NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 4 Andorra      NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 5 Angola       NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## 6 Antigua~     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## # ... with 254 more variables: 1761 <dbl>, 1762 <dbl>, 1763 <dbl>, 1764 <dbl>,
## #   1765 <dbl>, 1766 <dbl>, 1767 <dbl>, 1768 <dbl>, 1769 <dbl>, 1770 <dbl>,
## #   1771 <dbl>, 1772 <dbl>, 1773 <dbl>, 1774 <dbl>, 1775 <dbl>, 1776 <dbl>,
## #   1777 <dbl>, 1778 <dbl>, 1779 <dbl>, 1780 <dbl>, 1781 <dbl>, 1782 <dbl>,
## #   1783 <dbl>, 1784 <dbl>, 1785 <dbl>, 1786 <dbl>, 1787 <dbl>, 1788 <dbl>,
## #   1789 <dbl>, 1790 <dbl>, 1791 <dbl>, 1792 <dbl>, 1793 <dbl>, 1794 <dbl>,
## #   1795 <dbl>, 1796 <dbl>, 1797 <dbl>, 1798 <dbl>, 1799 <dbl>, 1800 <dbl>,
## #   1801 <dbl>, 1802 <dbl>, 1803 <dbl>, 1804 <dbl>, 1805 <dbl>, 1806 <dbl>,
## #   1807 <dbl>, 1808 <dbl>, 1809 <dbl>, 1810 <dbl>, 1811 <dbl>, 1812 <dbl>,
## #   1813 <dbl>, 1814 <dbl>, 1815 <dbl>, 1816 <dbl>, 1817 <dbl>, 1818 <dbl>,
## #   1819 <dbl>, 1820 <dbl>, 1821 <dbl>, 1822 <dbl>, 1823 <dbl>, 1824 <dbl>,
## #   1825 <dbl>, 1826 <dbl>, 1827 <dbl>, 1828 <dbl>, 1829 <dbl>, 1830 <dbl>,
## #   1831 <dbl>, 1832 <dbl>, 1833 <dbl>, 1834 <dbl>, 1835 <dbl>, 1836 <dbl>,
## #   1837 <dbl>, 1838 <dbl>, 1839 <dbl>, 1840 <dbl>, 1841 <dbl>, 1842 <dbl>,
## #   1843 <dbl>, 1844 <dbl>, 1845 <dbl>, 1846 <dbl>, 1847 <dbl>, 1848 <dbl>,
## #   1849 <dbl>, 1850 <dbl>, 1851 <dbl>, 1852 <dbl>, 1853 <dbl>, 1854 <dbl>,
## #   1855 <dbl>, 1856 <dbl>, 1857 <dbl>, 1858 <dbl>, 1859 <dbl>, 1860 <dbl>, ...
```

## Joining our data together

Now we will join the three dataframes together. Like before, we will use the `full_join` function. 


```r
politics$country <- politics$country_name

data <- politics %>%
  full_join(carbon_long, by = c("country", "year")) %>%
  full_join(gdp_long, by = c("country", "year"))

head(data)
```

```
##   country_name year v2x_libdem          v2x_regime                region
## 1  Afghanistan 2007      0.227 Electoral Autocracy Eastern Mediterranean
## 2  Afghanistan 2014      0.224 Electoral Autocracy Eastern Mediterranean
## 3  Afghanistan 2012      0.224 Electoral Autocracy Eastern Mediterranean
## 4  Afghanistan 2003      0.096    Closed Autocracy Eastern Mediterranean
## 5  Afghanistan 2015      0.227 Electoral Autocracy Eastern Mediterranean
## 6  Afghanistan 2009      0.226 Electoral Autocracy Eastern Mediterranean
##       country emissions    gdp
## 1 Afghanistan      2270 10.800
## 2 Afghanistan      9810  0.837
## 3 Afghanistan     10800 11.200
## 4 Afghanistan      1200  8.040
## 5 Afghanistan        NA  2.110
## 6 Afghanistan      6770 17.300
```

## Summary Statistics

To take a look at some summary statistics, we can use the built-in R functions.
  * `mean(joindata$emissions, na.rm = T)` - mean of `emissions`
  * `table(joindata$country)` to know the number of observations per country
  * `summary(joindata)` - summary statistics of the columns
  * `summary(joindata$emissions)` - summary statistics of the `emissions` variable
  
We can also look at some summary statistics by region. We use the `group_by()` function to group the observations. We use the `summarize()` function to calculate summary statistics. In the code below, `avg_emissions` is the column name I will assign to the mean emissions. 


```r
data %>% group_by(region) %>%
  summarize(avg_emissions = mean(emissions, na.rm = T))
```

```
## # A tibble: 8 x 2
##   region                  avg_emissions
##   <chr>                           <dbl>
## 1 ""                             75649.
## 2 "Africa"                       17118.
## 3 "Americas"                     75294.
## 4 "Eastern Mediterranean"        80377.
## 5 "Europe"                      145051.
## 6 "South-East Asia"             217400.
## 7 "Western Pacific"             538052.
## 8  <NA>                          70314.
```

I can request for multiple summary statistics in one command too.


```r
data %>% group_by(region) %>%
  summarize(n = n(),
            avg_emissions = mean(emissions, na.rm = T),
            median_emissions = median(emissions, na.rm = T))
```

```
## # A tibble: 8 x 4
##   region                      n avg_emissions median_emissions
##   <chr>                   <int>         <dbl>            <dbl>
## 1 ""                        413        75649.           110000
## 2 "Africa"                 1228        17118.             1770
## 3 "Americas"                754        75294.            11100
## 4 "Eastern Mediterranean"   609        80377.            36400
## 5 "Europe"                 1414       145051.            49150
## 6 "South-East Asia"         261       217400.            35700
## 7 "Western Pacific"         464       538052.            38800
## 8  <NA>                   47603        70314.             3140
```

The `modelsummary` package allows you to produce very nice summary statistic plots for tidy data. You can read more [here](https://vincentarelbundock.github.io/modelsummary/articles/datasummary.html). 


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
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2006.1 </td>
   <td style="text-align:right;"> 8.4 </td>
   <td style="text-align:right;"> 1992.0 </td>
   <td style="text-align:right;"> 2006.0 </td>
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
</g><defs><clipPath id="cpMC4wMHw0OC4wMHwyLjg4fDEyLjAw"><rect x="0.00" y="2.88" width="48.00" height="9.12"></rect></clipPath></defs><g clip-path="url(#cpMC4wMHw0OC4wMHwyLjg4fDEyLjAw)"><rect x="1.78" y="3.22" width="3.17" height="8.44" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="4.95" y="6.02" width="3.17" height="5.64" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="8.13" y="6.01" width="3.17" height="5.66" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="11.30" y="5.96" width="3.17" height="5.70" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="14.48" y="5.96" width="3.17" height="5.70" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="17.65" y="5.96" width="3.17" height="5.70" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="20.83" y="5.96" width="3.17" height="5.70" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="24.00" y="5.93" width="3.17" height="5.74" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="27.17" y="5.93" width="3.17" height="5.74" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="30.35" y="5.89" width="3.17" height="5.77" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="33.52" y="5.89" width="3.17" height="5.77" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="36.70" y="5.89" width="3.17" height="5.77" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="39.87" y="5.89" width="3.17" height="5.77" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="43.05" y="5.89" width="3.17" height="5.77" style="stroke-width: 0.38; fill: #000000;"></rect></g></svg>
</td>
  </tr>
  <tr>
   <td style="text-align:left;"> v2x_libdem </td>
   <td style="text-align:right;"> 858 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0.4 </td>
   <td style="text-align:right;"> 0.3 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 0.3 </td>
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
</g><defs><clipPath id="cpMC4wMHw0OC4wMHwyLjg4fDEyLjAw"><rect x="0.00" y="2.88" width="48.00" height="9.12"></rect></clipPath></defs><g clip-path="url(#cpMC4wMHw0OC4wMHwyLjg4fDEyLjAw)"><rect x="1.53" y="7.19" width="2.51" height="4.47" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="4.03" y="3.86" width="2.51" height="7.80" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="6.54" y="3.22" width="2.51" height="8.44" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="9.04" y="5.79" width="2.51" height="5.87" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="11.55" y="6.25" width="2.51" height="5.41" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="14.05" y="6.47" width="2.51" height="5.20" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="16.56" y="6.58" width="2.51" height="5.08" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="19.06" y="7.26" width="2.51" height="4.40" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="21.57" y="6.48" width="2.51" height="5.18" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="24.08" y="8.83" width="2.51" height="2.84" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="26.58" y="8.30" width="2.51" height="3.36" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="29.09" y="9.20" width="2.51" height="2.46" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="31.59" y="8.07" width="2.51" height="3.60" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="34.10" y="8.41" width="2.51" height="3.25" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="36.60" y="9.73" width="2.51" height="1.93" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="39.11" y="6.37" width="2.51" height="5.29" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="41.61" y="4.16" width="2.51" height="7.50" style="stroke-width: 0.38; fill: #000000;"></rect><rect x="44.12" y="9.25" width="2.51" height="2.41" style="stroke-width: 0.38; fill: #000000;"></rect></g></svg>
</td>
  </tr>
</tbody>
</table>

We need to add an argument `type = categorical` to see summary statistics for non-numeric data.


```r
datasummary_skim(data, type = "categorical")
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
   <td style="text-align:left;"> v2x_regime </td>
   <td style="text-align:left;"> Closed Autocracy </td>
   <td style="text-align:right;"> 857 </td>
   <td style="text-align:right;"> 1.6 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Electoral Autocracy </td>
   <td style="text-align:right;"> 1693 </td>
   <td style="text-align:right;"> 3.2 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Electoral Democracy </td>
   <td style="text-align:right;"> 1505 </td>
   <td style="text-align:right;"> 2.9 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Liberal Democracy </td>
   <td style="text-align:right;"> 1086 </td>
   <td style="text-align:right;"> 2.1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> region </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 413 </td>
   <td style="text-align:right;"> 0.8 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:right;"> 1228 </td>
   <td style="text-align:right;"> 2.3 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Americas </td>
   <td style="text-align:right;"> 754 </td>
   <td style="text-align:right;"> 1.4 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Eastern Mediterranean </td>
   <td style="text-align:right;"> 609 </td>
   <td style="text-align:right;"> 1.2 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Europe </td>
   <td style="text-align:right;"> 1414 </td>
   <td style="text-align:right;"> 2.7 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> South-East Asia </td>
   <td style="text-align:right;"> 261 </td>
   <td style="text-align:right;"> 0.5 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Western Pacific </td>
   <td style="text-align:right;"> 464 </td>
   <td style="text-align:right;"> 0.9 </td>
  </tr>
</tbody>
</table>

# Data Visualization

## Basic R

```r
plot(data$year, data$emissions)
```

![](intro-to-r_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

## ggpplot

How has carbon emissions changed over time? 

```r
data %>% group_by(year) %>%
  summarize(total_emissions = sum(emissions)) %>%
  ggplot(aes(x = year, y = total_emissions)) +
  geom_line(size = 1.5)
```

```
## Warning: Removed 270 row(s) containing missing values (geom_path).
```

![](intro-to-r_files/figure-html/unnamed-chunk-4-1.png)<!-- -->


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

```
## Warning: Removed 270 row(s) containing missing values (geom_path).
```

![](intro-to-r_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

Are certain countries contribute more emissions than others? 

```r
data %>% 
  ggplot(aes(x = year, y = emissions, group = country)) +
  geom_line(size = 1) +
  theme_classic()
```

```
## Warning: Removed 35157 row(s) containing missing values (geom_path).
```

![](intro-to-r_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

Change the transparency of the lines due to overlapping lines

```r
data %>% 
  ggplot(aes(x = year, y = emissions, group = country)) +
  geom_line(size = 1, 
            alpha = 0.4) +
  theme_classic()
```

```
## Warning: Removed 35157 row(s) containing missing values (geom_path).
```

![](intro-to-r_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

Compare USA to the rest of the world

```r
# data %>% 
#   ggplot(aes(x = year, y = emissions, group = country, color = region)) +
#   geom_line(size = 1) +
#   scale_colour_manual(values = c("grey", "black")) +
#   theme_classic()
```

## Visualize carbon emissions over the years for the top 10 countries with the highest emissions in 2014


```r
# Create a new object to identify the 10 countries with highest emissions in 2014
# top10countries <- data %>%
#   filter(year == 2014) %>%
#   mutate(rank = dense_rank(desc(emissions))) %>%
#   filter(rank <= 10) %>%
#   arrange(rank)
# 
# # filter original data for countries in top10
# top10data <- data %>%
#   filter(country %in% pull(top10countries, country)) 
# 
# ggplot(top10data, aes(x = year, y = emissions, color = country)) + 
#   geom_line() + 
#   theme_classic()
```

# Data Analysis

By the end of this section, the students will be able to:

* Calculate basic summary statistics
* Run hypothesis tests
* Run regressions
* Format regression results

## Summary Statistics

```r
# usa %>% summarise(mean_emissions = mean(emissions, na.rm = T), 
#                   mean_temp = mean(temp, na.rm = T),
#                   sd_emissions = sd(emissions, na.rm = T),
#                   sd_temp = sd(temp, na.rm = T))
```

## Correlation

```r
# usa <- usa %>% drop_na()
# 
# usa %>%
#   summarize(r = cor(x = emissions,
#                     y = temp,
#                     method = "pearson")) %>%
#   pull(r)
# 
# cortest <- cor.test(pull(usa, emissions), 
#                     pull(usa, temp))
# 
# # see the structure of the data
# # str(cortest)
# cortest$statistic
# cortest$p.value
# # tidy the results
# tidy(cortest)
```


```r
# politics %<>% rename(country = country_name)
# data2 <- data %>% full_join(politics, by =c("country", "year"))
# data2$gdp2 <- data2$gdp * data2$gdp
```

## Regressions

```r
# run the regression

# reg1 <- lm(emissions ~ gdp, data = usa)
# # stargazer(reg1, type = 'html')
# summary(reg1)
# 
# reg2 <- lm(emissions ~ log(energy) + gdp + gdp2 +v2x_libdem, data = data2)
# summary(reg2)
```


```r
# sum(residuals(reg2)^2)
# anova(reg2)
```
