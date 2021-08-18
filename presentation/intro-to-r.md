---
title: "Intro to R (Summer 2021)"
output:
  html_document:
    toc: TRUE
    toc_float: TRUE
    theme: lumen
    keep_md: TRUE
---



The main objective of this R bootcamp is to introduce R programming to incoming MFRE students. The content of this session is adapted from [Software Carpentry](https://datacarpentry.org/r-socialsci/) and Dr. Nick Huntington-Klein's Teaching Econometrics with R [slides](https://rpubs.com/NickCHK/RTeach2020). If you spot any errors or issues or any feedback, please send me a message on Canvas or at krisha.lim[at]ubc.ca. 

To learn more about R, you can refer to Jenny Bryan's [STAT545](https://stat545.com/) course notes and Hadley Wickham's [R for Data Science](https://r4ds.had.co.nz/) books. If you are used to Stata, here's some [notes](https://www.matthieugomez.com/statar/) on the different syntax between Stata and R. 

# Before we get started

* Programming in R is one of the tools taught in the MFRE program to support econometrics, economics, and business analysis applications. 
* R is the language. RStudio is the IDE. If I say let's use R, I mean let's run R using RStudio. 
* Stay organized. It is good to have one working directory per project. You can check for the working directory using the `getwd()` command. The best practice is to use R projects and the `{here}` package. We will talk about this topic in person.
  * The `{here}` package will determine the top-level of your current project, which is your working directory. In my case it is "H:/Workshops/2021-r-bootcamp". The `here()` function allows me to build the path relative to this directory. So instead of typing "H:/Workshops/2021-r-bootcamp/data/emissions.csv" to refer to the "emissions.csv" file saved in the "data" folder of my working directory (or "data/emission.csv" if I set the working directory), I can use `here("data", "emissions.csv")`.  
  * I discourage you from using`setwd("insert_file_path_here")` because this file path will only work on your computer. Read more about this issue [here](https://www.tidyverse.org/blog/2017/12/workflow-vs-script/). 


# Packages

There are thousands of packages (bundles of codes) available on [CRAN](https://cran.r-project.org/). If you are struggling to accomplish a certain task, it is very likely that someone has already created a function or a package to do it for you. 

To install a package, use the command `install.package("package_name")`. Do not forget the quotation marks! To install multiple packages at once, you can use the code `install.packages(c("package_1", "package_2", ...))`. 

A big difference between R and Stata is that you have to load the package you will (or plan to use) use every time you start a new R session. To load a package, use the command `library(package_name)`. The quotation marks are now optional. You can also write a function to load multiple libraries at once.

Sometimes, you may encounter the code `require(package_name)`. Like `library(package_name)`, it will load the already installed package. The main difference between `require()` and `library()` is that `library()` returns an error if the package you are calling is not yet installed, whereas `require()` will return only a warning. More information on the difference [here](https://www.r-bloggers.com/2016/12/difference-between-library-and-require-in-r/#:~:text=The%20require()%20is%20designed,if%20the%20package%20is%20loaded.&text=It%20is%20better%20to%20use,during%20the%20package%20loading%20time). 

The `pacman` package allows you to install and load packages in R more efficiently. The function `p_load()` checks whether a package is already installed, and if not, installs the package and loads it. The function can also be applied to several package at once, so you save a few (or many) lines of codes. 

Let's now load the packages we will use for the rest of the bootcamp! 


```r
install.packages("pacman")
library(pacman)
# require(pacman)
p_load(tidyverse, readxl, googlesheets4, readstata13, magrittr, cansim, stats, broom, modelsummary, flextable, here, lubridate)
```
 


# Interacting with R 

We can get output from R console by typing math expressions. 


```r
1 + 1 
2 + 4 * 1^3
100 %/% 60 # How many whole hours in 100 minutes?
100 %% 60 # How many minutes are left over?
```

We can also get output from R by typing logic statements in the console.


```r
1 > 2 & 1 > 0.5 # The "&" stands for "and"
1 >2 | 1 > 0.5 # The "|" stands for "or"
3 + 4 == 4 + 3 # The "==" stands for "equal to"
3 + 4 != 4 + 3 # The "!" is negation
```

To do useful things, we assign values to objects. `<-` is the assignment operator in R. Objects are one of the main differences of R with Stata. In R, you can work with many objects in the same session.

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

# Data object types

There are different types of objects in R. The common ones we will use are: numeric, character, factor, and logical. There are different functions (or commands) in R to examine objects. Common examples are `class()`, `typeof()`, `str()`, `length()`. 


```r
numeric_var <- 1.5
character_var <- "one"
factor_var <- factor(1, labels = "one")
logical_var <- TRUE
```

# Vectors

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

  * `length()` to inspect the number of elements in a vector
  * `class()` to inspect the type or class of an object
  * `str()` to inspect the structure of an object and see a preview of its elements, which is useful when working with large and complex objects
  

```r
length(countries)
```

```
## [1] 4
```

```r
class(countries) 
```

```
## [1] "character"
```

```r
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

You can also convert one data type to another. You may find these functions useful when you load numeric data but R reads it as character. 


```r
emissions <- as.character(emissions) 
class(emissions)
```

```
## [1] "character"
```

```r
emissions <- as.numeric(emissions) 
class(emissions)
```

```
## [1] "numeric"
```

## Subsetting Vectors

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

We can also use `%in%` to check whether an object is contained within or matches with a list of items. 


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

# Factors

Factors deal with categorical data. They are stored as integers with labels, which can be ordered (e.g. birth order, high/medium/low) or unordered (e.g. color, country). Factors create a structured relation between the different levels or values of a categorical variable. While factors look like character vectors, R treats them as integer vectors. 

The pre-defined set of values are called levels. By default, R sorts these levels in alphabetical order. For example, we specify a factor called `regions` with 3 levels. R will assign `1` to the level `"Africa"`, `2` to `"Americas"`, and `3` to "`Europe"`, even though the first element in the vector is `"Americas"`. You can see the levels with the function `levels()`. To determine the number of levels, you can use the function `nlevels()`. 


```r
regions <- factor(c("Americas", "Americas", "Europe", "Africa", "Europe"))
levels(regions) 
```

```
## [1] "Africa"   "Americas" "Europe"
```

```r
nlevels(regions)
```

```
## [1] 3
```

Sometimes, the order of the factors matters, such as when you have a rating of low, medium, or high. To order the values, you would have to use the `levels()` argument inside `factor()`. Specifying the levels is helpful when you plot since R will organize the plots according to the specified levels, instead of arranging it alphabetically. 


```r
responses <- factor(c("low", "low", "high", "medium", "high", "low"))
responses # current order 
```

```
## [1] low    low    high   medium high   low   
## Levels: high low medium
```

```r
plot(responses)
```

![](intro-to-r_files/figure-html/factors_levels-1.png)<!-- -->

```r
responses <- factor(responses, 
  levels = c("low", "medium", "high"))
responses # after re=ordering 
```

```
## [1] low    low    high   medium high   low   
## Levels: low medium high
```

```r
plot(responses)
```

![](intro-to-r_files/figure-html/factors_levels-2.png)<!-- -->

To make the factor an ordered factor, we have to add the `ordered = TRUE` argument inside the `factor()` function. When you print the factor, you will see the sign `<` to denote ranking. 


```r
responses_ordered <- factor(responses, 
                            ordered = TRUE)
responses_ordered
```

```
## [1] low    low    high   medium high   low   
## Levels: low < medium < high
```

On the back end, the factor levels "low", "medium", and "high" are represented by integers 1, 2, and 3. The advantage of using factors is that factors are more informative than just integers. From one look at the data, you know which one is "high" instead of guessing if 1 or 3 represents "high". This advantage is especially seen when you have many levels. 

Let's say for some reason you want to recode "medium" to "not sure", you use the `levels()` function and indicate the level you want to recode inside square brackets.


```r
levels(responses)[2] <- "not sure"
responses
```

```
## [1] low      low      high     not sure high     low     
## Levels: low not sure high
```

When your data is stored in factors, you can use the `plot()` function to see the number of observations at each factor level. 


```r
plot(responses)
```

![](intro-to-r_files/figure-html/factorsplot-1.png)<!-- -->

# Dates

We will not go into detail with handling dates in R in this session but here are some notes. Last year, when working with time series data, we used the `{xts}` and `{zoo}` packages.

When we load data with dates, R will not always recognize the date column as dates. For example, R may read that column as a character or numeric vector. We will need to convert that column into dates using functions like `as_date()` from the `{lubridate}` package. Once the vector is recognized as dates, we can use other functions such as `day()`, `month()`, and `year()` to extract information from the vector. The argument `format()` tells the function how to parse the characters and identify the month, day, and year. Specifying the wrong format can lead to parsing errors or incorrect results. You can read more about handling date formats [here](https://www.r-bloggers.com/2013/08/date-formats-in-r/).



```r
dates <- c("2021-08-01", "2021-08-02", "2021-08-03")
str(dates)
```

```
##  chr [1:3] "2021-08-01" "2021-08-02" "2021-08-03"
```

```r
dates_converted <- as_date(dates, format = "%Y-%m-%d")
class(dates_converted)
```

```
## [1] "Date"
```

```r
months(dates_converted)
```

```
## [1] "August" "August" "August"
```

```r
year(dates_converted)
```

```
## [1] 2021 2021 2021
```

We can also create a Date class object using the `seq()` function, which generates a sequence of dates starting from the date we provided (`2021/08/01`) with increments indicated in the `by =` option and ends once the length of the sequence indicated in the `length =` option is achieved.


```r
dates_seq <- seq(as.Date("2021/08/01"), length = 5, by = "days")
dates_seq
```

```
## [1] "2021-08-01" "2021-08-02" "2021-08-03" "2021-08-04" "2021-08-05"
```

# Matrices

So far we have looked at one-dimensional objects. Matrices are two-dimensional objects in R and another common R object we will work with. Elements must be of the same data type and are arranged in rows and columns.

We can construct a matrix using the `matrix()` function. We can check the attributes of a matrix using the `class()` and `dim()` functions.  


```r
k <- matrix(nrow = 3, ncol = 2)
k
```

```
##      [,1] [,2]
## [1,]   NA   NA
## [2,]   NA   NA
## [3,]   NA   NA
```

```r
class(k)
```

```
## [1] "matrix" "array"
```

```r
dim(k)
```

```
## [1] 3 2
```

Matrices are filled column-wise. The code below creates a 2x3 matrix called `l` filled the values 1 to 6. 


```r
l <- matrix(1:6, nrow = 2, ncol = 3)
l 
```

```
##      [,1] [,2] [,3]
## [1,]    1    3    5
## [2,]    2    4    6
```

We can also create a matrix using existing vectors. 


```r
# the 2 vectors must be of the same size length
countries <- c("Canada", "Kenya", "United States")

# Creates a matrix and assigns it to object m
m <- matrix(c(emissions, countries), nrow = 3)
m
```

```
##      [,1]      [,2]           
## [1,] "53700"   "Canada"       
## [2,] "14300"   "Kenya"        
## [3,] "5250000" "United States"
```

The `nrow = 3` argument tells R that the matrix has 3 rows. If we don't specify `nrow = 3`, then it will just create 1 column with the number of rows equal to the number of elements of the vectors. 


```r
m_1 <- matrix(c(emissions, countries))
m_1
```

```
##      [,1]           
## [1,] "53700"        
## [2,] "14300"        
## [3,] "5250000"      
## [4,] "Canada"       
## [5,] "Kenya"        
## [6,] "United States"
```

If we specify `nrow = 5` for example, the matrix will have 5 rows, and the values of some elements will repeat. You will also get a warning. 


```r
m_5 <- matrix(c(emissions, countries), nrow = 5)
```

```
## Warning in matrix(c(emissions, countries), nrow = 5): data length [6] is not a
## sub-multiple or multiple of the number of rows [5]
```

```r
m_5
```

```
##      [,1]      [,2]           
## [1,] "53700"   "United States"
## [2,] "14300"   "53700"        
## [3,] "5250000" "14300"        
## [4,] "Canada"  "5250000"      
## [5,] "Kenya"   "Canada"
```

We can also add column and row names to the matrix using the `colnames()` and `rownames()` functions, respectively


```r
colnames(m) <- c("emissions", "year")
rownames(m) <- c("c1", "c2", "c3")
m
```

```
##    emissions year           
## c1 "53700"   "Canada"       
## c2 "14300"   "Kenya"        
## c3 "5250000" "United States"
```

We can also construct a matrix using the `cbind()` function. In FRE501, Dr. Vercammen uses this function to construct a matrix.
  

```r
matrix <- cbind(emissions, countries)
matrix
```

```
##      emissions countries      
## [1,] "53700"   "Canada"       
## [2,] "14300"   "Kenya"        
## [3,] "5250000" "United States"
```

```r
class(matrix)
```

```
## [1] "matrix" "array"
```

If you use the `cbind()` function to create a matrix, the columns in the matrix takes the names of the R objects. We can also change the column and row names using the `colnames()` and `rownames()` functions. If we want to change a specific column (or row) number, we can specify the index number in brackets. 


```r
# change name of the first column 
colnames(matrix)[1] <- "emissions_new"

# add row names
rownames(matrix) <- c("c1", "c2", "c3")

# print column and names only
colnames(matrix)
```

```
## [1] "emissions_new" "countries"
```

```r
rownames(matrix)
```

```
## [1] "c1" "c2" "c3"
```

Just like vectors, we use brackets to subset matrices. Because matrices are two dimensional objects (rows and columns) while vectors are only one dimensional, we now need to indicate the row and column positions of the values we want to extract. The sytax would be `matrix[row_position, column_position]`. If we leave row position blank, R assumes that we are asking for the whole row. 


```r
# extract first element of the second column
matrix[1,2]
```

```
## [1] "Canada"
```

```r
# extract first row
matrix[1,]
```

```
## emissions_new     countries 
##       "53700"      "Canada"
```

```r
# extract first row by row name - don't forget the comma after
matrix["c2",]
```

```
## emissions_new     countries 
##       "14300"       "Kenya"
```

```r
# extract first column 
matrix[,1]
```

```
##        c1        c2        c3 
##   "53700"   "14300" "5250000"
```

```r
# extract by column name - don't forget the comma in front!
matrix[, c("emissions_new")]
```

```
##        c1        c2        c3 
##   "53700"   "14300" "5250000"
```

```r
# extract first two rows of the first column
matrix[1:2, 1]
```

```
##      c1      c2 
## "53700" "14300"
```

We will get an error if you combine two vectors of different size.


```r
countries <- c(countries, "China")

will_not_work <- cbind(emissions, countries)
```

```
## Warning in cbind(emissions, countries): number of rows of result is not a
## multiple of vector length (arg 1)
```

We will also get an error if we attempt to create a matrix using 2 vectors of different types (i.e. one is numeric and another is character).


```r
emission_type <- c("carbon dioxide", "carbon dioxide")

will_not_work_too <- cbind(emissions, emission_type)
```

```
## Warning in cbind(emissions, emission_type): number of rows of result is not a
## multiple of vector length (arg 2)
```

# Lists

A list is a flexible R object that is a collection of different objects.


```r
first_list <- list(a = 1:5, b = 6:10, c = c("food", "resource", "economics"))
first_list 
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
str(first_list)
```

```
## List of 3
##  $ a: int [1:5] 1 2 3 4 5
##  $ b: int [1:5] 6 7 8 9 10
##  $ c: chr [1:3] "food" "resource" "economics"
```

We can extract sub-objects using `[]` or `[[]]` or `$`. A common example of a list object we will interact with often is regression objects (more on this later!). 


```r
first_list["a"] # output is a list
```

```
## $a
## [1] 1 2 3 4 5
```

```r
first_list[["a"]] # output is character vector
```

```
## [1] 1 2 3 4 5
```

```r
first_list$c #output is character vector
```

```
## [1] "food"      "resource"  "economics"
```

# Data Frames

In MFRE, we will work a lot with data frames. A data frame is a list composed of vectors of equal length. We can think of a data frame as an Excel spreadsheet that contains columns of different data types, and all columns have the same number of rows. Data frames can store different data types in each column. For example, the first column can be a character vector, and the second column is a numeric vector. 

We can create a data frame object using the function `data.frame()`.


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

We can also convert the matrix we created earlier into a data frame using the `as.data.frame()` function.


```r
matrix_df <- as.data.frame(matrix)
```

Here are some functions to inspect the elements of a data frame. 

  * `dim()` to know the dimension of a data frame (can also be used in a matrix)
  * `str()` to know the structure of a data frame
  * `head()` and `tail()` to view the first or last 5 rows of the data frame 
  * `names()` to know the column names of a data frame
  

```r
dim(first_df)
```

```
## [1] 3 2
```

```r
str(first_df)
```

```
## 'data.frame':	3 obs. of  2 variables:
##  $ countries: chr  "Canada" "Kenya" "United States"
##  $ emissions: num  53700 14300 5250000
```

```r
head(first_df) 
```

```
##       countries emissions
## 1        Canada     53700
## 2         Kenya     14300
## 3 United States   5250000
```

```r
names(first_df)
```

```
## [1] "countries" "emissions"
```

We can extract elements inside the data frame using `[]`, `[[]]`, and `$`. 


```r
first_df["countries"] #output is a data frame
```

```
##       countries
## 1        Canada
## 2         Kenya
## 3 United States
```

```r
first_df[["countries"]] #output is a character vector
```

```
## [1] "Canada"        "Kenya"         "United States"
```

```r
first_df$countries #output is a character vector 
```

```
## [1] "Canada"        "Kenya"         "United States"
```

I would say `data_frame$variable` is the most common syntax you will encounter in most MFRE courses. Because you can work with multiple objects in R, you will have to specify which data frame the variable you want is located in. 

## Tibbles

A special type of data frame you may encounter is tibbles. You can read more about it [here](https://r4ds.had.co.nz/tibbles.html). 

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
second_tibble <- tibble(countries = c("Peru", "Mexico", "China"),
                        emissions = c(61700, 480000, 10300000))
```

# Functions

Functions are scripts that automate complicated commands. We can write your own function, or we can also use functions that are available in R packages. A function gets one or more inputs called *arguments*. Functions often, but not always, return a value. 

One example of a function is the `sqrt()`. If we type in `?sqrt()` in the console, we will see the documentation on the lower right panel under the 'Help` tab. We learn that the input (argument) must be numeric, and running the function will return the square root of the number. 


```r
a <- sqrt(100)
a
```

```
## [1] 10
```

In the example above, the value 100 is given to the `sqrt()` function. The `sqrt()` function calculates the square root, and returns the value assigned to the object `a`. 

Arguments differ per function, and we can look up the documentation using the `?function_name` command in the console. Some functions take arguments that must be specified by the user, and if not, will use a default value. These are called options. Options are ways to change how the function works. If working with a new function, it is useful to learn the defaults and how the author of the package programmed the functions.

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

If we provide the arguments in the same order as they are defined, i.e. `round(x, digits = 0)`, then we don't need to include `digits =` anymore. 


```r
round(3.14159, 2)
```

```
## [1] 3.14
```

You can find some built in R functions [here](https://www.statmethods.net/management/functions.html). Common ones we will use are `summary()`, `mean()`, `median()`, `min()`, `max()`. The code below shows how one can take the mean of the emissions variable in the `first_df` data frame.  


```r
mean(first_df$emissions)
```

```
## [1] 1772667
```

For these statistical functions, we will have to indicate how the function will treat missing values. In R, missing data are represented in vectors as `NA`. If we don't specify how the function will treat missing values, the function will return `NA`. 

Let's return to our `first_df` object. Let's say we want to add Peru's emissions, but we don't have the data yet. We can use the `add_row()` function to do this step. Notice that I also have a `%>%` operator there. It is called the pipe operator. The pipe operator allows us to express a series of operators clearly (more info [here](https://r4ds.had.co.nz/pipes.html)). It takes the output on the left of the `%>%` and pass it to the function on the right. In the code below, `first_df` (left of the `%>%`) is passed on to another function, which is the `add_row` function. Then we are assigning it back to `first_df`, overwriting our initial data. If we don't use the assignment operator `<-`, then we have not overwritten `first_df` and Peru will not appear when we print `first_df`. 


```r
first_df %>% 
  add_row(countries = "Peru", emissions = NA)
```

```
##       countries emissions
## 1        Canada     53700
## 2         Kenya     14300
## 3 United States   5250000
## 4          Peru        NA
```

```r
# Since we did not use an assignment operator, Peru is not saved in first_df
first_df
```

```
##       countries emissions
## 1        Canada     53700
## 2         Kenya     14300
## 3 United States   5250000
```

```r
first_df <- first_df %>% 
  add_row(countries = "Peru", emissions = NA)

first_df 
```

```
##       countries emissions
## 1        Canada     53700
## 2         Kenya     14300
## 3 United States   5250000
## 4          Peru        NA
```
  
Now, let's try to take the mean of emissions. 


```r
mean(first_df$emissions)
```

```
## [1] NA
```

Notice that you `NA` as the output. This feature makes it harder to overlook cases where you are dealing with missing data. If you take a look at the documentation (`?mean`), you can see that the default is `na.rm = FALSE`. By adding in `na.rm = TRUE`, then you are asking R to calculate the mean and that NA values should be ignored. 


```r
mean(first_df$emissions, na.rm = T)
```

```
## [1] 1772667
```

If you want to count the number of missing values for emissions, we can run the code `sum(is.na(first_df$emissions))`. If we want to count the number of non-missing values for emissions, we can run the code `sum(!is.na(first_df$emissions))`. 

Let's say we now know Peru's emissions and want to modify the value. We can use the `mutate()` and `ifelse()` functions. Because we do not use the assignment operator, if we print `first_df`, Peru will still have a missing value in  `first_df`.


```r
first_df %>% 
  mutate(emissions = ifelse(countries == "Peru", 100, emissions))
```

```
##       countries emissions
## 1        Canada     53700
## 2         Kenya     14300
## 3 United States   5250000
## 4          Peru       100
```

Alternatively, we can also use the fact that Peru's emissions is missing. We can read the code below as follows: For the emissions variable in the `first_df` data frame (`first_df$emissions`), if the emissions variable is missing (`[is.na(first_df$emissions)]`), then replace it with 100. Because we have the assignment operator, when we print `first_df`, we will see that Peru's emissions is now 100. *Note: You may see this coding style more often in FRE 501.*


```r
first_df$emissions[is.na(first_df$emissions)] <- 100 
```

## Writing your own functions

Sometimes, you will find it useful to write your own functions to automate certain tasks and to reuse them later on. Read more about how to write your own functions [here](http://environmentalcomputing.net/writing-simple-functions/). If you find yourself repeating the same steps (in your current or across multiple scripts), writing a function may help you save time and also avoid mistakes. 

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

Let's take a look at an example. R does not have a built in function to calculate standard error. Recall that the formula of the standard error is $SE =  \sqrt{var/ n}$

Let's say we want to calculate the standard error for emissions.


```r
sqrt(var(first_df$emissions, na.rm = T) / length(first_df$emissions))
```

```
## [1] 1306874
```

Not too bad! But what if you want to calculate the standard error for multiple variables, then you'd have to write the code above multiple times. Even if you copy paste the code and change the variable names, this process may be prone to errors. A better approach would be to make it a function. Following the syntax, we can write the standard error function as follows. 


```r
std_error <- function(x){
  sqrt(var(x, na.rm = T) / length(x))
}
```

Here, `std_error` is the function name. It takes `x` as the argument, which in our case would be a vector. Then every time we pass a vector to this function, it will calculate the formula provided in the body. 

Now let's add a new column that contains the GDP of these countries. Then let's use the `std_error()` function to compute the standard deviation. 


```r
first_df <- first_df %>%
  add_column(gdp = c(50300, 1090, 52100, 6110))

#always good to check if you wrote the function correctly
std_error(first_df$emissions) 
```

```
## [1] 1306874
```

```r
std_error(first_df$gdp)
```

```
## [1] 13783.99
```

## Importing a function

In some cases, we may write a function and save it in a different script. We can then import that script into your current script using the `source()` function. 

In this example below, we first specify the model parameters (a, b, m0, etc.). Then we call the function get_delta saved in the `price_function.R` script saved in the `code` folder in my working directory using the `source()` and `here()` commands. You will notice a `get_delta()` function appear in your `Environment` tab. Now, you can run the function as if you wrote it in this script. 

Other times `source()` may be helpful is when you have a "main" or "master" script that will run all the scripts in your project. For example, you might have a cleaning script, analysis script, figures script, and then a "main" script that runs all of these individual scripts. 


```r
# assign values to model parameters 
a <- 16.21
b <- 3.50
m0 <- -0.22
m1 <- 0.03
S0 <- 2.015
H1 <- 14.38
S_bar <- 2.015
v <- c(a, b, m0, m1, S0, H1, S_bar)

# call the script that contains the `get_delta()` function. 
source(here("code", "price_function.R"))

# run the function
del <- get_delta(v)
del
```

```
##           del0       del1      del2
## [1,]  9.545454 -0.4212615 0.4005162
## [2,]  9.760180 -0.4248723 0.4039492
## [3,]  9.919621 -0.4321249 0.4108446
## [4,] 10.025144 -0.4430814 0.4212615
## [5,] 10.077655 -0.4578358 0.4352893
## [6,] 10.077603 -0.4465144 0.4530481
## [7,] 10.024987 -0.4390203 0.4746902
## [8,]  9.919357 -0.4352893 0.5004010
```
