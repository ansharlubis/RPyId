Let’s make it functional part 1: R series
================

**2ndMeeting - Saturday, November 23, 2019**

**Tutors and Organizers:**

  - Pande Putu Erawijantari
  - Felix Salim
  - Mia Fitria Utami

## Content:

  - Data types and structure (review)
      - Loop
      - Functions
      - Analysing multiple datasets Sources:
  - Composing function:
    <http://swcarpentry.github.io/r-novice-inflammation/02-func-R/index.html>
  - Application:
    <http://swcarpentry.github.io/r-novice-inflammation/03-loops-R/index.html>
  - Starting with data:
    <https://datacarpentry.org/R-ecology-lesson/02-starting-with-data.html>

### Data types and structure

R contains 6 data types and structures, which are:

  - character
  - numeric (real or decimal)
  - integer
  - logical
  - complex

Please read and try the example here:
<https://swcarpentry.github.io/r-novice-inflammation/13-supp-data-structures/>

### Functions

ref:
<http://swcarpentry.github.io/r-novice-inflammation/02-func-R/index.html>

Functions is a defined sections of program that perform a specific task.
Defining a functions is very usefull if you are going to perform similar
task for several times. For example, you have several data that you will
visualize as a barplot, then it is better to create a defined function
to plot a barplot. Below is the example to convert Fahrenheit into
Celsius from the example in [Creating
functions](http://swcarpentry.github.io/r-novice-inflammation/02-func-R/index.html).

Defining the function. Pay attention to the syntax of defining the
functions in R and Python below.

**R**

``` r
fahrenheit_to_celsius <- function(temp_F) {
  temp_C <- (temp_F - 32) * 5 / 9
  return(temp_C)
}
fahrenheit_to_celsius(100)
```

    ## [1] 37.77778

**Python**

``` python
def fahrenheit_to_celcius(temp_F):
  temp_C = ((float(temp_F)-32)*5/9)
  return(temp_C)
fahrenheit_to_celcius(100)
```

    ## 37.77777777777778

Now, let’s try our functions understanding in analysing multiple set of
data. We will use the same set of data that we use in the [fist
meeting](http://swcarpentry.github.io/r-novice-inflammation/data/r-novice-inflammation-data.zip)
Previously, we only try to analyze the `inflammation-01.csv`. We still
have 12 data to go. Let’s try to apply functions to analyse our
inflammation data. Step by step tutorial will be following the example
in
<http://swcarpentry.github.io/r-novice-inflammation/03-loops-R/index.html>
First please make sure your are working in the right directory as we
studied in the previous meeting. Next, Let’s create a function called
analyze that creates graphs of the minimum, average, and maximum daily
inflammation rates for a single data set. Apply that function to the
`inflammation-01.csv`

``` r
analyze <- function(filename) {
  # Plots the average, min, and max inflammation over time.
  # Input is character string of a csv file.
  dat <- read.csv(file = filename, header = FALSE)
  avg_day_inflammation <- apply(dat, 2, mean)
  plot(avg_day_inflammation)
  max_day_inflammation <- apply(dat, 2, max)
  plot(max_day_inflammation)
  min_day_inflammation <- apply(dat, 2, min)
  plot(min_day_inflammation)
}
#call function
analyze("data/inflammation-01.csv")
```

![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-3-2.png)<!-- -->![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-3-3.png)<!-- -->
What about using the same function to analyse the second
dataset?

``` r
analyze("data/inflammation-02.csv")
```

![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-4-2.png)<!-- -->![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-4-3.png)<!-- -->
One line do the magic thing right? Hmmm, but this is not the easier way
to do that. Let’s do some magic `Loops` to replicate the same action of
one function to all of 12 data that we have.

### For Loops

To replicate same thing for several times, we can use `For Loops`.
Suppose we want to print each word in a sentence. One way is to use six
print statements:

**Bad example first**

``` r
best_practice <- c("Let", "the", "computer", "do", "the", "work")
print_words <- function(sentence) {
  print(sentence[1])
  print(sentence[2])
  print(sentence[3])
  print(sentence[4])
  print(sentence[5])
  print(sentence[6])
}

print_words(best_practice)
```

    ## [1] "Let"
    ## [1] "the"
    ## [1] "computer"
    ## [1] "do"
    ## [1] "the"
    ## [1] "work"

Seems works, what’s wrong? Try this

``` r
print_words(best_practice[-6])
```

    ## [1] "Let"
    ## [1] "the"
    ## [1] "computer"
    ## [1] "do"
    ## [1] "the"
    ## [1] NA

R seems nice because it dose not throw an error, in python, this is a
big NO. Below is an example in python. You can see that it will not let
your code
continue.

``` python
best_practice=["Let", "the", "computer", "do", "the", "work"] #this is call `list` in python, we will talk about this later.
def print_words(sentence):
  print(sentence[0]) #remember python start with 0 but R start with 1
  print(sentence[1])
  print(sentence[2])
  print(sentence[3])
  print(sentence[4])
  print(sentence[5])

print_words(best_practice)
```

    ## Let
    ## the
    ## computer
    ## do
    ## the
    ## work

``` python
print_words(best_practice[-6])
```

    ## Error in py_call_impl(callable, dots$args, dots$keywords): IndexError: string index out of range
    ## 
    ## Detailed traceback: 
    ##   File "<string>", line 1, in <module>
    ##   File "<string>", line 5, in print_words

Now, let’s try using `For loops` instead and see how it works.

``` r
print_words <- function(sentence) {
  for (word in sentence) {
    print(word)
  }
}

print_words(best_practice)
```

    ## [1] "Let"
    ## [1] "the"
    ## [1] "computer"
    ## [1] "do"
    ## [1] "the"
    ## [1] "work"

Please try another example in
<http://swcarpentry.github.io/r-novice-inflammation/03-loops-R/index.html>
before we are going to process our multiple files.

## Processing Multiple Files

Sometimes we do not need to create function for specific task if it is
available in the programing languange. For example, to begin the
analysis on multiple files, we have to list what files we are going to
analyze. We do not need to write it ourselves because R already has a
function to do this called `list.files.` If we run the function without
any arguments, `list.files()`, it returns every file in the current
working directory. We can understand this result by reading the help
file `(?list.files)`. The first argument, path, is the path to the
directory to be searched, and it has the default value of `"."` (“.” is
shorthand for the current working directory). The second argument,
pattern, is the pattern being searched, and it has the default value of
NULL. Since no pattern is specified to filter the files, all files are
returned.

``` r
list.files(path = "data", pattern = "csv")
```

    ##  [1] "car-speeds-cleaned.csv" "car-speeds.csv"        
    ##  [3] "inflammation-01.csv"    "inflammation-02.csv"   
    ##  [5] "inflammation-03.csv"    "inflammation-04.csv"   
    ##  [7] "inflammation-05.csv"    "inflammation-06.csv"   
    ##  [9] "inflammation-07.csv"    "inflammation-08.csv"   
    ## [11] "inflammation-09.csv"    "inflammation-10.csv"   
    ## [13] "inflammation-11.csv"    "inflammation-12.csv"   
    ## [15] "sample.csv"             "small-01.csv"          
    ## [17] "small-02.csv"           "small-03.csv"

``` r
list.files(path = "data", pattern = "inflammation")
```

    ##  [1] "inflammation-01.csv" "inflammation-02.csv" "inflammation-03.csv"
    ##  [4] "inflammation-04.csv" "inflammation-05.csv" "inflammation-06.csv"
    ##  [7] "inflammation-07.csv" "inflammation-08.csv" "inflammation-09.csv"
    ## [10] "inflammation-10.csv" "inflammation-11.csv" "inflammation-12.csv"

> Remember that, if you will be working with larger project, it is
> better to specified sub-directories for better organizations. Refer to
> [A quick guide to organizing computational biology
> projects](http://swcarpentry.github.io/r-novice-inflammation/03-loops-R/index.html)
> for more advice.

To show the complete path in directory you can use the argument
`full.names = TRUE`

``` r
list.files(path = "data", pattern = "csv", full.names = TRUE)
```

    ##  [1] "data/car-speeds-cleaned.csv" "data/car-speeds.csv"        
    ##  [3] "data/inflammation-01.csv"    "data/inflammation-02.csv"   
    ##  [5] "data/inflammation-03.csv"    "data/inflammation-04.csv"   
    ##  [7] "data/inflammation-05.csv"    "data/inflammation-06.csv"   
    ##  [9] "data/inflammation-07.csv"    "data/inflammation-08.csv"   
    ## [11] "data/inflammation-09.csv"    "data/inflammation-10.csv"   
    ## [13] "data/inflammation-11.csv"    "data/inflammation-12.csv"   
    ## [15] "data/sample.csv"             "data/small-01.csv"          
    ## [17] "data/small-02.csv"           "data/small-03.csv"

Let’s test out running our analyze function by using it on the first
three files in the vector returned by `list.files`:

``` r
filenames <- list.files(path = "data",  
                        # Now follows a regular expression that matches:
                        pattern = "inflammation-[0-9]{2}.csv",
                        #          |            |        the standard file extension of comma-separated values
                        #          |            the variable parts (two digits, each between 0 and 9)
                        #          the static part of the filenames
                        full.names = TRUE)
filenames <- filenames[1:3]
for (f in filenames) {
  print(f)
  analyze(f)
}
```

    ## [1] "data/inflammation-01.csv"

![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-13-2.png)<!-- -->![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-13-3.png)<!-- -->

    ## [1] "data/inflammation-02.csv"

![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-13-4.png)<!-- -->![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-13-5.png)<!-- -->![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-13-6.png)<!-- -->

    ## [1] "data/inflammation-03.csv"

![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-13-7.png)<!-- -->![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-13-8.png)<!-- -->![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-13-9.png)<!-- -->

> In this lesson we saw how to use a simple for loop to repeat an
> operation. As you progress with R, you will learn that there are
> multiple ways to accomplish this. Sometimes the choice of one method
> over another is more a matter of personal style, but other times it
> can have consequences for the speed of your code. For instruction on
> best practices, see this [software carpentry supplementary
> lesson](http://swcarpentry.github.io/r-novice-inflammation/15-supp-loops-in-depth/)
> that demonstrates how to properly repeat operations in R.

> **Key point to remember** - creating `functions` can help us to to
> perform similar task repeatedly - `for loop` can execute a block of
> code a number of times - `for loop` and `functions` are useful for
> performing similar task in multiple datasets

We have done practicing in simple format of data. In real cases our data
usually are not that simple, e.g. tables consits of several columns and
rows (we will call this dataframe). In some cases, we also have to
combine multiple dataframe generated by multiple samples into one for
easier analysis, rather than repeat everything in each samples. Don’t
worry in the next meeting we will try to analyse “real data”. To help us
with that, we will utilize [Tidyverse](https://www.tidyverse.org/).

For those who finished the `for loop` and `function` excersise, you can
continue with content below\!

## Starting with Data

reference :
<https://datacarpentry.org/R-ecology-lesson/02-starting-with-data.html>

Please read the explanations about [starting with data in
R](https://datacarpentry.org/R-ecology-lesson/02-starting-with-data.html)
once you finish with the `for loop` and `function` excersise. The data
is about the species repartition and weight of animals caught in plots
in specific study area. The dataset is stored as a comma separated value
(CSV) file.

### Loading data as dataframe

We are going to use the R function `download.file()` to download the CSV
file that contains the survey data from Figshare, and we will use
`read.csv()` to load into memory the content of the CSV file as an
object of class data.frame. Inside the download.file command, the first
entry is a character string with the source URL
\<“<https://ndownloader.figshare.com/files/2292169>”\>. This source
URL downloads a CSV file from figshare. The text after the comma
(“data\_raw/portal\_data\_joined.csv”) is the destination of the file
on your local machine. You’ll need to have a folder on your machine
called “data\_raw” where you’ll download the file. So this command
downloads a file from Figshare, names it “portal\_data\_joined.csv,” and
adds it to a preexisting folder named “data\_raw”. To create a folder in
your directory using you terminal you can do `mkdir data_raw`, or inside
R.

    dir.create("data_raw")

``` r
download.file(url="https://ndownloader.figshare.com/files/2292169",
              destfile = "data_raw/portal_data_joined.csv")
```

You are now ready to load the data. Let’s load the data and store in the
variable named (“surveys”)

``` r
surveys <- read.csv("data_raw/portal_data_joined.csv")
```

This statement doesn’t produce any output because, as you might recall,
assignments don’t display anything. If we want to check that our data
has been loaded, we can see the contents of the data frame by typing its
name: `surveys`

Let’s check the top (the first 6 lines) of this data frame using the
function
    `head()`:

``` r
head(surveys)
```

    ##   record_id month day year plot_id species_id sex hindfoot_length weight
    ## 1         1     7  16 1977       2         NL   M              32     NA
    ## 2        72     8  19 1977       2         NL   M              31     NA
    ## 3       224     9  13 1977       2         NL                  NA     NA
    ## 4       266    10  16 1977       2         NL                  NA     NA
    ## 5       349    11  12 1977       2         NL                  NA     NA
    ## 6       363    11  12 1977       2         NL                  NA     NA
    ##     genus  species   taxa plot_type
    ## 1 Neotoma albigula Rodent   Control
    ## 2 Neotoma albigula Rodent   Control
    ## 3 Neotoma albigula Rodent   Control
    ## 4 Neotoma albigula Rodent   Control
    ## 5 Neotoma albigula Rodent   Control
    ## 6 Neotoma albigula Rodent   Control

> **Note** `read.csv` assumes that fields are delineated by commas. Not
> everyone using “,” as delimiter of the file. Sometimes you will prefer
> to use tab delimiter (TSV) as it easier to load/ read to Microsoft
> excel. If you want to read in this type of files in R, you can use the
> `read.csv2` function. It behaves exactly like read.csv but uses
> different parameters for the decimal and the field separators. If you
> are working with another format, they can be both specified by the
> user. Check out the help for `read.csv()` by typing `?read.csv` to
> learn more. There is also the `read.delim()` for in tab separated data
> files. It is important to note that all of these functions are
> actually wrapper functions for the main `read.table()` function with
> different arguments. As such, the surveys data above could have also
> been loaded by using read.table() with the separation argument as ,.
> The code is as follows: `surveys <-
> read.table(file="data_raw/portal_data_joined.csv", sep=",",
> header=TRUE)` The header argument has to be set to TRUE to be able to
> read the headers as by default `read.table()` has the header argument
> set to FALSE.

Read more about what is dataframe
[here](%22https://datacarpentry.org/R-ecology-lesson/02-starting-with-data.html#what_are_data_frames%22).

**Problem 1**

    - What is the class of the object "surveys" ?
    - How many rows and how many columns are in this object?
    - How many species have been recorded during these surveys?

[Answer to
Problem 1](https://github.com/erawijantari/RPyId/blob/master/Notes/20191130_RPyID2nd/R2ndMeeting_answer.md)

### Indexing and subseting dataframe

Our survey data frame has rows and columns (it has 2 dimensions), if we
want to extract some specific data from it, we need to specify the
“coordinates” we want from it. Row numbers come first, followed by
column numbers. However, note that different ways of specifying these
coordinates lead to results with different classes.

``` r
# first element in the first column of the data frame (as a vector)
surveys[1, 1]
```

    ## [1] 1

``` r
# first element in the 6th column (as a vector)
surveys[1, 6]   
```

    ## [1] NL
    ## 48 Levels: AB AH AS BA CB CM CQ CS CT CU CV DM DO DS DX NL OL OT OX ... ZL

    # first column of the data frame (as a vector)
    surveys[, 1]

    # first column of the data frame (as a data.frame)
    surveys[1] 

``` r
# first three elements in the 7th column (as a vector)
surveys[1:3, 7] 
```

    ## [1] M M  
    ## Levels:  F M

    # the 3rd row of the data frame (as a data.frame)
    surveys[3, ] 

``` r
# equivalent to head_surveys <- head(surveys)
head_surveys <- surveys[1:6, ] 
```

`:`is a special function that creates numeric vectors of integers in
increasing or decreasing order, test `1:10` and `10:1` for instance.

You can also exclude certain indices of a data frame using the “-”
    sign:

    surveys[, -1]          # The whole data frame, except the first column

    surveys[-c(7:34786), ] # Equivalent to head(surveys)

Data frames can be subset by calling indices (as shown previously), but
also by calling their column names directly:

    surveys["species_id"]       # Result is a data.frame
    surveys[, "species_id"]     # Result is a vector
    surveys[["species_id"]]     # Result is a vector
    surveys$species_id          # Result is a vector

Shows columns names in dataframe.

``` r
colnames(surveys)
```

    ##  [1] "record_id"       "month"           "day"            
    ##  [4] "year"            "plot_id"         "species_id"     
    ##  [7] "sex"             "hindfoot_length" "weight"         
    ## [10] "genus"           "species"         "taxa"           
    ## [13] "plot_type"

In RStudio, you can use the autocompletion (by presing `tab`) feature to
get the full and correct names of the columns.

**Problem
    2**

    1. Create a `data.frame` (surveys_200) containing only the data in row 200 of the surveys dataset.
    
    2. Notice how `nrow()` gave you the number of rows in a `data.frame`?
      - Use that number to pull out just that last row in the data frame.
      - Compare that with what you see as the last row using tail() to make sure it’s meeting expectations.
      - Pull out that last row using nrow() instead of the row number.
      - Create a new data frame (surveys_last) from that last row.
    
    3. Use nrow() to extract the row that is in the middle of the data frame. Store the content of this row in an object named surveys_middle.
    
    4. Combine nrow() with the - notation above to reproduce the behavior of head(surveys), keeping just the first through 6th rows of the surveys dataset.

[Answer to
Problem 2](https://github.com/erawijantari/RPyId/blob/master/Notes/20191130_RPyID2nd/R2ndMeeting_answer.md)

### Factors

When we did `str(surveys)` we saw that several of the columns consist of
integers. The columns `genus`, `species`, `sex`, `plot_type`, … however,
are of a special class called factor. Factors are very useful and
actually contribute to making R particularly well suited to working with
data. So we are going to spend a little time introducing them.

Factors represent categorical data. They are stored as integers
associated with labels and they can be ordered or unordered. While
factors look (and often behave) like character vectors, they are
actually treated as integer vectors by R. So you need to be very careful
when treating them as strings.

Once created, factors can only contain a pre-defined set of values,
known as levels. By default, R always sorts levels in alphabetical
order. For instance, if you have a factor with 2 levels:

``` r
sex <- factor(c("male", "female", "female", "male"))
```

R will assign `1` to the level “female” and `2` to the level “male”
(because f comes before m, even though the first element in this vector
is “male”). You can see this by using the function `levels()` and you
can find the number of levels using `nlevels()`:

``` r
levels(sex)
```

    ## [1] "female" "male"

``` r
nlevels(sex)
```

    ## [1] 2

Sometimes, the order of the factors does not matter, other times you
might want to specify the order because it is meaningful (e.g., “low”,
“medium”, “high”), it improves your visualization, or it is required
by a particular type of analysis. Here, one way to reorder our levels in
the sex vector would be:

``` r
sex # current order
```

    ## [1] male   female female male  
    ## Levels: female male

``` r
sex <- factor(sex, levels = c("male", "female"))
sex # after re-ordering
```

    ## [1] male   female female male  
    ## Levels: male female

In R’s memory, these factors are represented by integers (1, 2, 3), but
are more informative than integers because factors are self describing:
`"female"`, `"male"` is more descriptive than `1`, `2`. Which one is
“male”? You wouldn’t be able to tell just from the integer data.
Factors, on the other hand, have this information built in. It is
particularly helpful when there are many levels (like the species names
in our example dataset).

If you need to convert a factor to a character vector, you use
`as.character(x)`.

``` r
as.character(sex)
```

    ## [1] "male"   "female" "female" "male"

In some cases, you may have to convert factors where the levels appear
as numbers (such as concentration levels or years) to a numeric vector.
For instance, in one part of your analysis the years might need to be
encoded as factors (e.g., comparing average weights across years) but in
another part of your analysis they may need to be stored as numeric
values (e.g., doing math operations on the years). This conversion from
factor to numeric is a little trickier. The `as.numeric()` function
returns the index values of the factor, not its levels, so it will
result in an entirely new (and unwanted in this case) set of numbers.
One method to avoid this is to convert factors to characters, and then
to numbers.

``` r
year_fct <- factor(c(1990, 1983, 1977, 1998, 1990))
as.numeric(year_fct)               # Wrong! And there is no warning...
```

    ## [1] 3 2 1 4 3

``` r
as.numeric(as.character(year_fct)) # Works...
```

    ## [1] 1990 1983 1977 1998 1990

``` r
as.numeric(levels(year_fct))[year_fct]    # The recommended way.
```

    ## [1] 1990 1983 1977 1998 1990

Another method is to use the `levels()` function. Compare:

    Notice that in the `levels()` approach, three important steps occur:
    - We obtain all the factor levels using levels(year_fct)
    - We convert these levels to numeric values using as.numeric(levels(year_fct))
    - We then access these numeric values using the underlying integers of the vector year_fct inside the square brackets

When your data is stored as a factor, you can use the plot() function to
get a quick glance at the number of observations represented by each
factor level. Let’s look at the number of males and females captured
over the course of the
experiment:

``` r
## bar plot of the number of females and males captured during the experiment:
plot(surveys$sex)
```

![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-28-1.png)<!-- -->
In addition to males and females, there are about 1700 individuals for
which the sex information hasn’t been recorded. Additionally, for these
individuals, there is no label to indicate that the information is
missing or undetermined. Let’s rename this label to something more
meaningful. Before doing that, we’re going to pull out the data on sex
and work with that data, so we’re not modifying the working copy of the
data frame:

``` r
sex <- surveys$sex
head(sex)
```

    ## [1] M M        
    ## Levels:  F M

``` r
levels(sex)
```

    ## [1] ""  "F" "M"

``` r
levels(sex)[1] <- "undetermined"
levels(sex)
```

    ## [1] "undetermined" "F"            "M"

``` r
head(sex)
```

    ## [1] M            M            undetermined undetermined undetermined
    ## [6] undetermined
    ## Levels: undetermined F M

Showing numerical data in
histogram:

``` r
hist(surveys$hindfoot_length)
```

![](2nd_FunctionalProgram_files/figure-gfm/unnamed-chunk-33-1.png)<!-- -->

By default, when building or importing a data frame, the columns that
contain characters (i.e. text) are coerced (= converted) into factors.
Depending on what you want to do with the data, you may want to keep
these columns as character. To do so, `read.csv()` and \``read.table()
have an argument called`stringsAsFactors\` which can be set to FALSE.

In most cases, it is preferable to set `stringsAsFactors = FALSE` when
importing data and to convert as a factor only the columns that require
this data
type.

``` r
## Compare the difference between our data read as `factor` vs `character`.
print("stringsAsFactors = TRUE")
```

    ## [1] "stringsAsFactors = TRUE"

``` r
surveys <- read.csv("data_raw/portal_data_joined.csv", stringsAsFactors = TRUE)
str(surveys)
```

    ## 'data.frame':    34786 obs. of  13 variables:
    ##  $ record_id      : int  1 72 224 266 349 363 435 506 588 661 ...
    ##  $ month          : int  7 8 9 10 11 11 12 1 2 3 ...
    ##  $ day            : int  16 19 13 16 12 12 10 8 18 11 ...
    ##  $ year           : int  1977 1977 1977 1977 1977 1977 1977 1978 1978 1978 ...
    ##  $ plot_id        : int  2 2 2 2 2 2 2 2 2 2 ...
    ##  $ species_id     : Factor w/ 48 levels "AB","AH","AS",..: 16 16 16 16 16 16 16 16 16 16 ...
    ##  $ sex            : Factor w/ 3 levels "","F","M": 3 3 1 1 1 1 1 1 3 1 ...
    ##  $ hindfoot_length: int  32 31 NA NA NA NA NA NA NA NA ...
    ##  $ weight         : int  NA NA NA NA NA NA NA NA 218 NA ...
    ##  $ genus          : Factor w/ 26 levels "Ammodramus","Ammospermophilus",..: 13 13 13 13 13 13 13 13 13 13 ...
    ##  $ species        : Factor w/ 40 levels "albigula","audubonii",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ taxa           : Factor w/ 4 levels "Bird","Rabbit",..: 4 4 4 4 4 4 4 4 4 4 ...
    ##  $ plot_type      : Factor w/ 5 levels "Control","Long-term Krat Exclosure",..: 1 1 1 1 1 1 1 1 1 1 ...

``` r
print("stringsAsFactors = FALSE")
```

    ## [1] "stringsAsFactors = FALSE"

``` r
surveys <- read.csv("data_raw/portal_data_joined.csv", stringsAsFactors = FALSE)
str(surveys)
```

    ## 'data.frame':    34786 obs. of  13 variables:
    ##  $ record_id      : int  1 72 224 266 349 363 435 506 588 661 ...
    ##  $ month          : int  7 8 9 10 11 11 12 1 2 3 ...
    ##  $ day            : int  16 19 13 16 12 12 10 8 18 11 ...
    ##  $ year           : int  1977 1977 1977 1977 1977 1977 1977 1978 1978 1978 ...
    ##  $ plot_id        : int  2 2 2 2 2 2 2 2 2 2 ...
    ##  $ species_id     : chr  "NL" "NL" "NL" "NL" ...
    ##  $ sex            : chr  "M" "M" "" "" ...
    ##  $ hindfoot_length: int  32 31 NA NA NA NA NA NA NA NA ...
    ##  $ weight         : int  NA NA NA NA NA NA NA NA 218 NA ...
    ##  $ genus          : chr  "Neotoma" "Neotoma" "Neotoma" "Neotoma" ...
    ##  $ species        : chr  "albigula" "albigula" "albigula" "albigula" ...
    ##  $ taxa           : chr  "Rodent" "Rodent" "Rodent" "Rodent" ...
    ##  $ plot_type      : chr  "Control" "Control" "Control" "Control" ...

``` r
## Convert the column "plot_type" into a factor
surveys$plot_type <- factor(surveys$plot_type)
```

The automatic conversion of data type is sometimes a blessing, sometimes
an annoyance. Be aware that it exists, learn the rules, and double check
that data you import in R are of the correct type within your data
frame. If not, use it to your advantage to detect mistakes that might
have been introduced during data entry (for instance, a letter in a
column that should only contain numbers).

Learn more in this [RStudio
tutorial](https://support.rstudio.com/hc/en-us/articles/218611977-Importing-Data-with-RStudio).

## More challenge

See notebooks here, it also cover our next meeting topics.
<https://www.kaggle.com/rtatman/getting-started-in-r-load-data-into-r>.

## Further Reading

  - The core of R (deep understanding how R work):
    <https://adv-r.hadley.nz/>
  - R for Data Science: <https://r4ds.had.co.nz/>
  - Rmarkdown for reproducible documentation:
    <https://bookdown.org/yihui/rmarkdown/>
  - R graph cookbook: <https://r-graphics.org/>
  - Feeling gig for Regex (regular expression):
    <http://www.grymoire.com/Unix/Regular.html>
