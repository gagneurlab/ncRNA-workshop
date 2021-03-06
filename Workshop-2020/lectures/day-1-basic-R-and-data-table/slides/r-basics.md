---
title       : 'Make your paper figures professionally: Scientific data analysis and visualization with R'
author      : Vangelis Theodorakis, Fatemeh Behjati, Julien Gagneur, Marcel Schulz
subtitle    : Grammar of graphics and Basic Plotting
framework   : io2012
highlighter : highlight.js
hitheme     : tomorrow
widgets     : [mathjax, bootstrap, quiz]
ext_widgets : {rCharts: ["libraries/highcharts", "libraries/nvd3"]}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---
<style>
.eighteen > article > p {
   font-size: 18px !important;
}
</style>

<!-- Center image on slide -->
<script src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js"></script>
<script type='text/javascript'>
$(function() {
    $("p:has(img)").addClass('centered');
});
</script>
</script>
<script type = 'text/javascript'>
$('p:has(img.build)').addClass('build')
</script>

<script type='text/javascript'>
// parameters
var sections = ["RStudio and R basics",
"Data Wrangling using Data.table",
"Analysis execution flow",
"Reproducible science with Rmarkdown reports"];

var title = "Overview";
var fontsize = "20pt"
var unselected_color = "#888888"
// function
function toc(cur) {
  // header
  document.write("<h2>"+title+"</h2>");
  // find current
  ind = sections.indexOf(cur);
  if (ind==-1 && cur.length>0) {
     document.write("<br/>Error: section not defined '"+cur+"'");
     return;
  }
  // write all out
  document.write("<br/><ul>");
  for (i = 0; i < sections.length; i++) { 
    if (cur=="") 
      // all the same
      document.write("<li style='font-size:"+fontsize+"'>"+sections[i]+"</li>");
    else {
      if (i==ind)
        document.write("<li style='font-size:"+fontsize+"'><b>"+sections[i]+"</b></li>");
      else
        document.write("<li style='color:"+unselected_color+";font-size:"+fontsize+"'>"+sections[i]+"</li>");
    }
  }
  document.write("</ul>");
}
</script>





<!-- START LECTURE -->

## Day 1 - RStudio and R basics

<img src="assets/img/syllabus-day-one.png" title="plot of chunk syllabus" alt="plot of chunk syllabus" width="800px" height="400px" />

---

<script type='text/javascript'>toc("")</script>

---

<script type='text/javascript'>toc("RStudio and R basics")</script>

---

## Assignments

All big (programming) journeys start with a small step (or assignment). In R assignments are done using the `<-` operator :


```r
objectName <- value
```

It is also possible to assign with the *equal* sign.


```r
objectName = value
```

---

## Assignments


```r
x <- 5 # Both methods have the same outcome
y = 5
x
```

```
## [1] 5
```

```r
y
```

```
## [1] 5
```

---

## Assignments

However, the "equal" sign is used for argument passing to functions. Thus if nesting, the equal sign will be interpreted as a argument assignment and might throw an error:


```r
## We want to measure the running time of the inner product of a large vector and 
## assign the outcome of the function to a variable simultaneously
system.time(a <- t(1:1e6)%*%(1:1e6))
```

```
##    user  system elapsed 
##   0.019   0.007   0.026
```

```r
system.time(a = t(1:1e6)%*%(1:1e6)) ## This would cause an error
```

```
## Error in system.time(a = t(1:1e+06) %*% (1:1e+06)): unused argument (a = t(1:1e+06) %*% (1:1e+06))
```

Right Alt + - gives a quick shortcut to add **<-** ;)

---

## Vectors

Vectors are 1-dimensional data structures which can contain one or more variables, regardless of their type.

Usually created with `c()` (from *concatenation*):

```r
c(1, 5, 8, 10)
```

```
## [1]  1  5  8 10
```

```r
str(c(1, 5, 8, 10))
```

```
##  num [1:4] 1 5 8 10
```

```r
length(c(1, 5, 8, 10))
```

```
## [1] 4
```

---

## Vectors

Vectors are 1-dimensional data structures which can contain one or more variables, regardless of their type.

Usually created with `c()` (from *concatenation*):

```r
c("a", "B", "cc")
```

```
## [1] "a"  "B"  "cc"
```

```r
c(TRUE, FALSE, c(TRUE, TRUE))
```

```
## [1]  TRUE FALSE  TRUE  TRUE
```

```r
c(1, "B", FALSE)
```

```
## [1] "1"     "B"     "FALSE"
```

---

## Vectors

There are multiple ways to create a numeric sequence depending on the desired result.

Usually for an integer sequence *from:to* is enough. For different results we may need to use `seq()` function utilizing its arguments to increase by a *step* of our choice or to split the range depending on the desired length of the output.


```r
1:10 
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

```r
seq(from = 1, to = 10, by = .3)
```

```
##  [1]  1.0  1.3  1.6  1.9  2.2  2.5  2.8  3.1  3.4  3.7  4.0  4.3  4.6  4.9  5.2
## [16]  5.5  5.8  6.1  6.4  6.7  7.0  7.3  7.6  7.9  8.2  8.5  8.8  9.1  9.4  9.7
## [31] 10.0
```

---

## Vectors


```r
seq(from = 1, to = 10, length.out = 12)
```

```
##  [1]  1.000000  1.818182  2.636364  3.454545  4.272727  5.090909  5.909091
##  [8]  6.727273  7.545455  8.363636  9.181818 10.000000
```

Keep in mind that sometimes different approaches, give different data types as results so when weird things happen, always check the documentation (devil is in the details)!

---

## Vectors

For non numeric values e.g. logical, character, `rep()` function comes also in handy. 

The *replicate* function `rep()` replicates a vector a certain number of *times* and concatenates them: 

```r
rep(c(TRUE, FALSE), times = 5)
```

```
##  [1]  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE
```

To replicate *each* entry of the input vector at the time:

```r
rep(c(TRUE, FALSE), each = 5)
```

```
##  [1]  TRUE  TRUE  TRUE  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE
```

---

## Vectors

Vector elements are accessed via the ```[``` operator:

```r
## Create an A,B,C,D,E vector
x <- LETTERS[1:5]
x
```

```
## [1] "A" "B" "C" "D" "E"
```

```r
## access the third entry
x[3]
```

```
## [1] "C"
```

---

## Vectors

Vector elements are accessed via the ```[``` operator:


```r
## modify the third entry
x[3] <- 'Z'
x
```

```
## [1] "A" "B" "Z" "D" "E"
```

---

## Vectors

Elements in vectors can have names. 
Using names instead of index to access entries in a vector make code more robust to re-ordering, sub-setting, and changes in data input.  
Names can be created at initialization or set afterwards with `names()`. 


```r
x <- c(a = 1, b = 2, c = 3)
x
```

```
## a b c 
## 1 2 3
```

```r
names(x) <- c("A", "B", "C")
x
```

```
## A B C 
## 1 2 3
```

---

## Vectors

Names don't have to be unique, but should preferably be, as sub-setting by names will only return the first match

```r
x <- c(a = 1, a = 2, b = 3)
x["a"]
```

```
## a 
## 1
```

---

## Vectors

Not all elements need to have names.

```r
c(a = 1, 2, 3)
```

```
## a     
## 1 2 3
```

---

## Combining vectors vectors

To combine 2 vectors we just need to wrap them inside a 3rd vector.

```r
x <- c(1:5)
y <- c(6:10)
z <- c(x, y)
z
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

---

## Subsetting vectors


```r
x <- LETTERS[1:5]
x
```

```
## [1] "A" "B" "C" "D" "E"
```

```r
## By positive integers
x[c(1,3)]
```

```
## [1] "A" "C"
```

```r
## By negative integers
x[-c(1,3)]
```

```
## [1] "B" "D" "E"
```

---

## Subsetting vectors


```r
## By logical vectors
x[c(TRUE, TRUE, TRUE, FALSE, FALSE)]
```

```
## [1] "A" "B" "C"
```

```r
## By logical vectors
x[x <= "C"]
```

```
## [1] "A" "B" "C"
```

---

## Subsetting vectors

`NAs` in the input vector will always produce an `NA` in the output:

```r
x[c(1,2,NA)]
```

```
## [1] "A" "B" NA
```

---

## Subsetting vectors

We can also sub-setting by character vectors, if names are present:

```r
names(x) <- letters[1:5]
x[c("a", "b")]
```

```
##   a   b 
## "A" "B"
```


---

## Exercise 1

---

## Factors

* A factor is a is used to store categorical data. The distinct categories are called 'levels'.

* They belong to a special `class()`, *factor*, which makes them behave differently from regular integer vectors.

* Factors cannot be altered in the same way as vectors. Only their levels can be altered.

* Factors are built on top of integer vectors: the values are integer indexes in the dictionary of levels.

* They occupy less space in memory than characters, since they are stored as integers.

---

## Factors



```r
x <- factor(c("red", "yellow", "red","green", "green"))
x
```

```
## [1] red    yellow red    green  green 
## Levels: green red yellow
```

Level | Integer 
------|--------
green  | 1 
red | 2 
yellow | 3 

---

## Factors

* Factors are typically constructed with `factor()`. By default the levels are the unique values, sorted by alphanumerical order. 

```r
x <- factor(c("red", "yellow", "red","green", "green"))
x
```

```
## [1] red    yellow red    green  green 
## Levels: green red yellow
```

---

## Factors

`levels()` gives the levels in ascending order of the underlying encoding integers.

```r
levels(x)
```

```
## [1] "green"  "red"    "yellow"
```

---

## Factors

The order of the levels can be forced:

```r
x <- factor(c("red", "yellow", "red","green", "green"), levels=c("green", "yellow", "red"))
x
```

```
## [1] red    yellow red    green  green 
## Levels: green yellow red
```

```r
levels(x)
```

```
## [1] "green"  "yellow" "red"
```

---

## Factors

The order of the levels is then used for all function requiring comparisons, e.g. sorting 

```r
sort(x)
```

```
## [1] green  green  yellow red    red   
## Levels: green yellow red
```

---

## Factors

Only level values can be used:

```r
x <- factor(c("red", "yellow", "red","green", "green"))
x[2] <- "blue"
x
```

```
## [1] red   <NA>  red   green green
## Levels: green red yellow
```

**Be aware** that R does not prevent you from combining factors:

Do not try to combine factors, especially if levels are not the same!


```r
c(x, factor("blue"))
```

```
## [1]  2 NA  2  1  1  1
```

---

## Exercise 2

---

## Math

R supports various mathematical and logical operations between scalars, vectors etc.

* Basic mathematical operations: `+`, `*`, `-`, `/`
* Boolean operations:
  * **&** element-wise AND
  * **|** element-wise OR
  * **!** NOT

---

## Binary comparison

- Element-wise Binary comparison return a vector of same length as the input
  - **==** element-wise equality
  - **!=** element-wise inequality
  - **<**, **<=** element-wise smaller (or equal)
  - **>**, **>=** element-wise greater (or equal)


```r
1:5 == 1:5
```

```
## [1] TRUE TRUE TRUE TRUE TRUE
```

---

## Binary comparison

* Binary comparison with a single Boolean statement as output
  * `identical(x, y)` exact equality
  * `all(x)` are all TRUE?

```r
identical(1:5, 1:5)
```

```
## [1] TRUE
```

---

<script type='text/javascript'>toc("Data Wrangling using Data.table")</script>

---

## Data Wrangling using Data.table

Data Wrangling includes tasks of processing raw data into interesting formats....

Data.tables are a modern implementation of tables in R and we will exclusively used data.tables in this course.They are a memory efficient and faster implementation of data.frame (basic R table type) offering:

* sub-setting
* ordering
* merging

---

## Data Wrangling using Data.table

It accepts all data.frame functions, it has a shorter and more flexible syntax (not so straightforward in the beginning but pays off) and saves time on two fronts:

* programming (easier to code, read, debug and maintain)
* computing (fast and memory efficient)

The general form of data.table syntax is:

    DT[ i,  j,  by ] # + extra arguments
        |   |   |
        |   |    -------> grouped by what?
        |    -------> what to do?
         ---> on which rows?

The way to read this out loud is: "Take DT, subset rows by i, then compute j grouped by by. Here are some basic usage examples expanding on this definition.

---

## Loading data as a data.table

We can read files from disk and process them using data.table. 
The easiest way to do so is to use the function `fread()`.


```r
flights <- fread('path_to_file/flightsLAX.csv')
```


Calling `head()` on the table showes us the first few lines of the table and we observe that reading the file was succesfull.

```r
head(flights)
```

```
##    YEAR MONTH DAY DAY_OF_WEEK AIRLINE FLIGHT_NUMBER TAIL_NUMBER ORIGIN_AIRPORT
## 1: 2015     1   1           4      AA          2336      N3KUAA            LAX
## 2: 2015     1   1           4      AA           258      N3HYAA            LAX
## 3: 2015     1   1           4      US          2013      N584UW            LAX
## 4: 2015     1   1           4      DL          1434      N547US            LAX
## 5: 2015     1   1           4      AA           115      N3CTAA            LAX
## 6: 2015     1   1           4      UA          1545      N76517            LAX
##    DESTINATION_AIRPORT DEPARTURE_TIME AIR_TIME DISTANCE ARRIVAL_TIME
## 1:                 PBI              2      263     2330          741
## 2:                 MIA             15      258     2342          756
## 3:                 CLT             44      228     2125          753
## 4:                 MSP             35      188     1535          605
## 5:                 MIA            103      255     2342          839
## 6:                 IAH            112      156     1379          607
```

---

## Creating a data.table

To create a data.table, give a name to the columns and we populate them.


```r
library(data.table)
DT <- data.table(x = rep(c("a", "b", "c"), each = 3),
                 y = c(1, 3, 6), 
                 v = 1:9)
DT # note how column y was recycled
```

```
##    x y v
## 1: a 1 1
## 2: a 3 2
## 3: a 6 3
## 4: b 1 4
## 5: b 3 5
## 6: b 6 6
## 7: c 1 7
## 8: c 3 8
## 9: c 6 9
```

---

## Creating a data.table

Any matrix-like object can be transformed to a `data.table` using the `as.data.table()` function


```r
class(iris)
```

```
## [1] "data.frame"
```

```r
iris.dt <- as.data.table(iris)
class(iris.dt)
```

```
## [1] "data.table" "data.frame"
```

---

## Inspecting tables

A first step in any analysis should involve inspecting the data we just read in. 
This often starts by looking the head of the table as we did above.

The next information we are often interested in is the size of our data set.
We can use the following commands to obtain it.

```r
nrow(flights)   # ncol(flights)
```

```
## [1] 389369
```

```r
dim(flights)
```

```
## [1] 389369     13
```

---

## Accessing a data.table by `rows`

Data.tables can be subsetted by indices, if we for example care about the second element of the table.
We can do the following.

```r
flights[2, ]   # Access the 2nd row (also flights[2] or flights[i = 2])
```

```
##    YEAR MONTH DAY DAY_OF_WEEK AIRLINE FLIGHT_NUMBER TAIL_NUMBER ORIGIN_AIRPORT
## 1: 2015     1   1           4      AA           258      N3HYAA            LAX
##    DESTINATION_AIRPORT DEPARTURE_TIME AIR_TIME DISTANCE ARRIVAL_TIME
## 1:                 MIA             15      258     2342          756
```

---

## Accessing a data.table by `rows`

Data.tables can be subsetted by indices, if we for example care about the second element of the table.
We can do the following.


```r
flights[1:2]   # Access multiple consecutive rows.
```

```
##    YEAR MONTH DAY DAY_OF_WEEK AIRLINE FLIGHT_NUMBER TAIL_NUMBER ORIGIN_AIRPORT
## 1: 2015     1   1           4      AA          2336      N3KUAA            LAX
## 2: 2015     1   1           4      AA           258      N3HYAA            LAX
##    DESTINATION_AIRPORT DEPARTURE_TIME AIR_TIME DISTANCE ARRIVAL_TIME
## 1:                 PBI              2      263     2330          741
## 2:                 MIA             15      258     2342          756
```

---

## Accessing a data.table by `rows`

Data.tables can be subsetted by indices, if we for example care about the second element of the table.
We can do the following.


```r
flights[c(3, 5)]  # Access multiple rows.
```

```
##    YEAR MONTH DAY DAY_OF_WEEK AIRLINE FLIGHT_NUMBER TAIL_NUMBER ORIGIN_AIRPORT
## 1: 2015     1   1           4      US          2013      N584UW            LAX
## 2: 2015     1   1           4      AA           115      N3CTAA            LAX
##    DESTINATION_AIRPORT DEPARTURE_TIME AIR_TIME DISTANCE ARRIVAL_TIME
## 1:                 CLT             44      228     2125          753
## 2:                 MIA            103      255     2342          839
```

---

## Accessing a data.table by `columns`

Additionally we can do the same for columns:

```r
head(flights[, 2 ])
```

```
##    MONTH
## 1:     1
## 2:     1
## 3:     1
## 4:     1
## 5:     1
## 6:     1
```

---

## Accessing a data.table by `columns`

Additionally we can do the same for columns:



```r
head(flights[, c(3, 5)])
```

```
##    DAY AIRLINE
## 1:   1      AA
## 2:   1      AA
## 3:   1      US
## 4:   1      DL
## 5:   1      AA
## 6:   1      UA
```

---

## Accessing a data.table by `columns`

Alternatively to subsetting by indices we can subset columns by their names.


```r
head(flights[, .(MONTH)])
```

```
##    MONTH
## 1:     1
## 2:     1
## 3:     1
## 4:     1
## 5:     1
## 6:     1
```

---

## Accessing a data.table by `columns`

Alternatively to subsetting by indices we can subset columns by their names.


```r
head(flights[, .(DAY, AIRLINE)])
```

```
##    DAY AIRLINE
## 1:   1      AA
## 2:   1      AA
## 3:   1      US
## 4:   1      DL
## 5:   1      AA
## 6:   1      UA
```

---

## Accessing a data.table by `rows and columns`

Based on the `dt[i, j, by]` syntax you can target specific rows and columns. However it is not advisable to access a column by its number. **Use the column name instead**.


```r
flights[1:5, TAIL_NUMBER]    # Access column x (also DT$x or DT[j=x]).
```

```
## [1] "N3KUAA" "N3HYAA" "N584UW" "N547US" "N3CTAA"
```

---

## Accessing a data.table by `rows and columns`

When accessing many columns, we probably want to return a data.table instead of a vector. For that, we need to provide R with a list, so we use ``list(colA, colB)``, or its simplified version ``.(colA, colB)``.


```r
# Note that 1 and 3 were coerced into strings because a vector can have only 1 type
flights[1:2, c(FLIGHT_NUMBER, ORIGIN_AIRPORT)]   
```

```
## [1] "2336" "258"  "LAX"  "LAX"
```

---

## Accessing a data.table by `rows and columns`

When accessing many columns, we probably want to return a data.table instead of a vector. For that, we need to provide R with a list, so we use ``list(colA, colB)``, or its simplified version ``.(colA, colB)``.


```r
# Access a specific subset. Data.frame: DF[1:2, c("x","y")] 
flights[1:2, list(TAIL_NUMBER, ORIGIN_AIRPORT)]
```

```
##    TAIL_NUMBER ORIGIN_AIRPORT
## 1:      N3KUAA            LAX
## 2:      N3HYAA            LAX
```

```r
# Same as before.
flights[1:2, .(TAIL_NUMBER, ORIGIN_AIRPORT)]
```

```
##    TAIL_NUMBER ORIGIN_AIRPORT
## 1:      N3KUAA            LAX
## 2:      N3HYAA            LAX
```

---

## Sub-setting rows according to some condition

A often more usefull way is to subset rows using conditions!
We can subset a data table using the following operators:

* `==`
* `<` 
* `>`
* `!=`
* `%in%`

---

## Sub-setting rows according to some condition

For example if we are interested in analysing all the flights operated by "AA" (American Airlines) we can do that using the following command:


```r
head(flights[AIRLINE == "AA"])
```

```
##    YEAR MONTH DAY DAY_OF_WEEK AIRLINE FLIGHT_NUMBER TAIL_NUMBER ORIGIN_AIRPORT
## 1: 2015     1   1           4      AA          2336      N3KUAA            LAX
## 2: 2015     1   1           4      AA           258      N3HYAA            LAX
## 3: 2015     1   1           4      AA           115      N3CTAA            LAX
## 4: 2015     1   1           4      AA          2410      N3BAAA            LAX
## 5: 2015     1   1           4      AA          1515      N3HMAA            LAX
## 6: 2015     1   1           4      AA          1686      N4XXAA            LAX
##    DESTINATION_AIRPORT DEPARTURE_TIME AIR_TIME DISTANCE ARRIVAL_TIME
## 1:                 PBI              2      263     2330          741
## 2:                 MIA             15      258     2342          756
## 3:                 MIA            103      255     2342          839
## 4:                 DFW            600      150     1235         1052
## 5:                 ORD            557      202     1744         1139
## 6:                 STL            609      183     1592         1134
```

---

## Sub-setting rows according to some condition

Alternatively if we are interested in all flights from any destination to the airports in NYC (JFK and LGA), we can subset the rows using the following command:

```r
head(flights[DESTINATION_AIRPORT %in% c("LGA", "JFK")])
```

```
##    YEAR MONTH DAY DAY_OF_WEEK AIRLINE FLIGHT_NUMBER TAIL_NUMBER ORIGIN_AIRPORT
## 1: 2015     1   1           4      B6            24      N923JB            LAX
## 2: 2015     1   1           4      DL           476      N196DN            LAX
## 3: 2015     1   1           4      AA           118      N788AA            LAX
## 4: 2015     1   1           4      VX           404      N621VA            LAX
## 5: 2015     1   1           4      UA           275      N598UA            LAX
## 6: 2015     1   1           4      B6           124      N943JB            LAX
##    DESTINATION_AIRPORT DEPARTURE_TIME AIR_TIME DISTANCE ARRIVAL_TIME
## 1:                 JFK            620      279     2475         1413
## 2:                 JFK            650      274     2475         1458
## 3:                 JFK            650      268     2475         1436
## 4:                 JFK            728      268     2475         1512
## 5:                 JFK            806      277     2475         1606
## 6:                 JFK            814      279     2475         1612
```

---

## Sub-setting rows according to some condition


Additionally we can concatenate multiple condition using the logical OR `|` or AND `&` operator.
If we for example want to inspect all flights departing between 6am and 7am operated by American Airlines we can use the following statement:

```r
head(flights[AIRLINE == "AA" & DEPARTURE_TIME > 600 & DEPARTURE_TIME < 700])
```

```
##    YEAR MONTH DAY DAY_OF_WEEK AIRLINE FLIGHT_NUMBER TAIL_NUMBER ORIGIN_AIRPORT
## 1: 2015     1   1           4      AA          1686      N4XXAA            LAX
## 2: 2015     1   1           4      AA          1361      N3KYAA            BNA
## 3: 2015     1   1           4      AA          2420      N3ERAA            LAX
## 4: 2015     1   1           4      AA           338      N3DJAA            LAX
## 5: 2015     1   1           4      AA          2424      N3DLAA            LAX
## 6: 2015     1   1           4      AA           222      N3JSAA            LAX
##    DESTINATION_AIRPORT DEPARTURE_TIME AIR_TIME DISTANCE ARRIVAL_TIME
## 1:                 STL            609      183     1592         1134
## 2:                 LAX            607      255     1797          847
## 3:                 DFW            619      149     1235         1119
## 4:                 SFO            644       55      337          803
## 5:                 DFW            641      146     1235         1149
## 6:                 BOS            650      271     2611         1442
```

---

## Grouping by rows (DT[i, j, ``by``])

The ``by = `` option allows us to apply a function to groups wtihin a data.table. For example, we can use the ``by = `` to compute the mean flight time per airline:


```r
# Compute the mean air time of every airline
flights.with.mean <- flights[, .(mean_AIRTIME = mean(AIR_TIME, na.rm=TRUE)), by = AIRLINE]
head(flights.with.mean)
```

```
##    AIRLINE mean_AIRTIME
## 1:      AA    219.48133
## 2:      US    210.39488
## 3:      DL    207.07201
## 4:      UA    211.62008
## 5:      OO     68.02261
## 6:      AS    141.01870
```

---

## Grouping by rows (DT[i, j, ``by``])
This way we can easily spot that one airline conducts on average shorter flights.


```r
# Compute the mean and standard deviation of the air time of every airline
flights.with.mean.sd <- flights[, .(mean_AIRTIME = mean(AIR_TIME, na.rm=TRUE),
                                    sd_AIR_TIME = sd(AIR_TIME, na.rm=TRUE)), by = AIRLINE]
head(flights.with.mean.sd)
```

```
##    AIRLINE mean_AIRTIME sd_AIR_TIME
## 1:      AA    219.48133    92.88972
## 2:      US    210.39488   105.22483
## 3:      DL    207.07201    88.90857
## 4:      UA    211.62008    94.83246
## 5:      OO     68.02261    41.06504
## 6:      AS    141.01870    51.80642
```

Although we could write ``flights[i = 5, j = AIRLINE]``, we usually ommit the ``i =`` and ``j =`` from the syntax, and write ``flights[5, ARILINE]`` instead. However, for clarity we usually include the ``by =`` in the syntax.

---

## Counting occurences (the `.N` command)

The ``.N`` is a special in-built variable that counts the number observations within a table.

Evaluating ``.N`` alone is equal to `nrow()` of a table.

```r
flights[, .N]
```

```
## [1] 389369
```

```r
nrow(flights)
```

```
## [1] 389369
```

---

## Counting occurences (the `.N` command)

But the ``.N`` command becomes a lot more powerful when used with grouping or conditioning.
We already saw earlier how we can use it to count the number of occurrences of elements in categorical columns.


```r
# Get the number of flights for each AIRLINE
flights.by.company <- flights[, .N, by = 'AIRLINE']
head(flights.by.company)
```

```
##    AIRLINE     N
## 1:      AA 65483
## 2:      US  7374
## 3:      DL 50343
## 4:      UA 54862
## 5:      OO 73389
## 6:      AS 16144
```

---

## Counting occurences (the `.N` command)

Remembering the data.table definition: "Take **DT**, subset rows using **i**, then select or calculate **j**, grouped by **by**",
we can build even more powerful statements using all three elements.


```r
# For each airline, get the number of observations to NYC (JFK)
flights.by.company.filtered <- flights[DESTINATION_AIRPORT == "JFK", .N, by = 'AIRLINE']
head(flights.by.company.filtered)
```

```
##    AIRLINE    N
## 1:      B6 2488
## 2:      DL 2546
## 3:      AA 3804
## 4:      VX 1652
## 5:      UA 1525
```

---

## Creating new columns (the `:=` command)

The ``:=`` operator updates the data.table you are working on, so writing DT <- DT[,... := ...] is redundant. This operator, plus all ``set`` functions (TODO clarify), change their input by *reference*. No copy of the object is made, that is why it is faster and uses less memory.


```r
# Add a new column called SPEED whose value is the DISTANCE divided by AIR_TIME
flights[, SPEED := DISTANCE / AIR_TIME * 60]
head(flights)
```

```
##    YEAR MONTH DAY DAY_OF_WEEK AIRLINE FLIGHT_NUMBER TAIL_NUMBER ORIGIN_AIRPORT
## 1: 2015     1   1           4      AA          2336      N3KUAA            LAX
## 2: 2015     1   1           4      AA           258      N3HYAA            LAX
## 3: 2015     1   1           4      US          2013      N584UW            LAX
## 4: 2015     1   1           4      DL          1434      N547US            LAX
## 5: 2015     1   1           4      AA           115      N3CTAA            LAX
## 6: 2015     1   1           4      UA          1545      N76517            LAX
##    DESTINATION_AIRPORT DEPARTURE_TIME AIR_TIME DISTANCE ARRIVAL_TIME    SPEED
## 1:                 PBI              2      263     2330          741 531.5589
## 2:                 MIA             15      258     2342          756 544.6512
## 3:                 CLT             44      228     2125          753 559.2105
## 4:                 MSP             35      188     1535          605 489.8936
## 5:                 MIA            103      255     2342          839 551.0588
## 6:                 IAH            112      156     1379          607 530.3846
```

---

## Creating new columns (the `:=` command)

Having computed a new column using the ``:=`` operator we can use it for further analyses.

```r
flights[, .(mean_AIR_TIME = mean(AIR_TIME, na.rm=TRUE), 
            mean_SPEED = mean(SPEED, na.rm=TRUE),
            mean_DISTANCE = mean(DISTANCE, na.rm=TRUE)
            ), by=AIRLINE] 
```

```
##     AIRLINE mean_AIR_TIME mean_SPEED mean_DISTANCE
##  1:      AA     219.48133   461.2839     1739.2331
##  2:      US     210.39488   452.1641     1658.2581
##  3:      DL     207.07201   466.0330     1656.2165
##  4:      UA     211.62008   464.2928     1693.5504
##  5:      OO      68.02261   349.5549      437.2337
##  6:      AS     141.01870   439.0120     1040.0340
##  7:      B6     309.79568   484.8242     2486.1489
##  8:      NK     179.55828   450.0221     1402.1591
##  9:      VX     185.36374   433.0870     1432.5384
## 10:      WN     105.19976   409.3803      760.2593
## 11:      HA     307.95961   497.3118     2537.8107
## 12:      F9     159.94041   461.0684     1235.6664
## 13:      MQ     102.15210   435.5580      737.0000
```

---

## Creating new columns (the `:=` command)

Now we can see that the flights by the carrier "OO" are not just shorter, but also slow. 
This could for example lead us to the hypothesis, that "OO" is a small regional carrier, which operates slower planes.

Additionally we can use the ``:=`` operator to remove columns. If we for example observe that tail numbers are not important for our analysis we can remove them with the following statement:


```r
flights[, FLIGHT_NUMBER := NULL]
head(flights)
```

```
##    YEAR MONTH DAY DAY_OF_WEEK AIRLINE TAIL_NUMBER ORIGIN_AIRPORT
## 1: 2015     1   1           4      AA      N3KUAA            LAX
## 2: 2015     1   1           4      AA      N3HYAA            LAX
## 3: 2015     1   1           4      US      N584UW            LAX
## 4: 2015     1   1           4      DL      N547US            LAX
## 5: 2015     1   1           4      AA      N3CTAA            LAX
## 6: 2015     1   1           4      UA      N76517            LAX
##    DESTINATION_AIRPORT DEPARTURE_TIME AIR_TIME DISTANCE ARRIVAL_TIME    SPEED
## 1:                 PBI              2      263     2330          741 531.5589
## 2:                 MIA             15      258     2342          756 544.6512
## 3:                 CLT             44      228     2125          753 559.2105
## 4:                 MSP             35      188     1535          605 489.8936
## 5:                 MIA            103      255     2342          839 551.0588
## 6:                 IAH            112      156     1379          607 530.3846
```
Here we observe, that the tail numbers are gone from the data.table

---

## Exercise 3

---

<script type='text/javascript'>toc("Analysis execution flow")</script>

---

## Conditional Statements

A typical way to control the flow of the execution of your code is to have conditional statements.

The typical setup is:


```r
if(statement_to_test) {
  print("Statement is true")
} else {
  print("Statement is false")
}
```

---

## Conditional Statements

Long version:

```r
if(1 < 2) {
  print("TRUE")
} else {
  print("FALSE")
}
```

```
## [1] "TRUE"
```

---

## Conditional Statements

Short version if the To-Do step fits into one line:

```r
{
if(1 > 2) print("TRUE")
else print("FALSE")
}
```

```
## [1] "FALSE"
```

The curly brackets around the if-else statement are only needed in a script to tell R to which *if* the *else* belongs to.

---

## Conditional Statements

You can also have multiple statement tests. If more than one statements can be satisfied the first that got evaluated will be executed:

```r
x <- 1
if(x >= 0) {
  print("X is 0, or maybe not?")
} else if(x == 1) {
  print("X is 1")
} else {
  print("X is something else")
}
```

```
## [1] "X is 0, or maybe not?"
```

---

## Conditional Statements

You can also chain *if-else* statements:

```r
x <- 5
if(x > 0) {
  if(x > 3) {
    if(x == 5) {
      cat("X is 5")
    } else {
      cat("X is greater than 3")
    }  
  } else {
    cat("X positive and less than or equal to 3")
  }
} else {
  cat("X less than or equal to 0")
}
```

R also supports switch statements in order to avoid multiple `else if` statements, but this is for more advanced users.

---

## For loops

R supports looping through the traditional *for* and *while* statements and with the use of the *apply family* functions.

Syntax:
```
for(index in sequence) {
  do something
}
```

---

## For loops

Again, if the To-Do step is a one-liner, curly brackets can be dropped

```r
a <- NULL
for(i in 1:10) a <- c(a, sum(1:i))
a
```

```
##  [1]  1  3  6 10 15 21 28 36 45 55
```

---

## For loops

A sequence can be any vector, including lists

```r
myList <- list(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
a <- integer(10)
for(i in myList) a[i] <- sum(1:i)
a
```

```
##  [1]  1  3  6 10 15 21 28 36 45 55
```

However, usually looping using *for/while* statements is more time consuming compared to the the functions of the *apply family*, but this is for more advanced users. 

---

## Functions

Function are a nice way to break down a big problem (bake a pizza) to smaller, manageable sub-problems (make the dough, make the sauce, add toppings, bake the pizza). They offer code re-usage (save time for important things, like wine drinking), easier testing of functionality and cleaner code!

---

## Functions

The syntax to write your own functions is the following:


```r
myFunction <- function(args1, args2 = default, ...) {
  body
  return(result)
}
```

* If the `return` statement is missing, the result of the last line will be returned
* The parameters can be assigned default values
* If the default value is a vector, the first value will be used by default

---

## Functions

* The *three dots* (`...`) in the function arguments are called **ellipsis** and are used as a placeholder for any arguments passed to a function call inside. This is useful when you don't want to push all parameters from embodied function to your functions or allow for generic functions without pre-specifying argument names.


```r
myFunction <- function(args1, args2 = c("option1", "option2"), ...) {
  result <- doSomething(args1, ...)
  return(result)
}
myFunction(args1, args2, param1)
```

--- 

## Exercise 4

---

<script type='text/javascript'>toc("Reproducible science with Rmarkdown reports")</script>

---

## R Markdown

* This is an R Markdown presentation. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>. 

* Simply go to File --> New File --> R Markdown

* Select PDF and you get a template.

* You most likely won't need more commands than in on the first page of [this cheat sheet](https://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf).

* When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document.  

---

## Exercise 5

---

## Useful references

* These slides are mostly based on Hadley Wickham's book [Advanced R](http://adv-r.had.co.nz/)

* [**Everything else** from Hadley Wickham](http://hadley.nz/)

* In-depth documentations:

  * [Introduction to R](https://cran.r-project.org/doc/manuals/r-release/R-intro.pdf)

  * [R language definition](https://cran.r-project.org/doc/manuals/r-release/R-lang.pdf)

  * [R Internals](https://cran.r-project.org/doc/manuals/r-release/R-ints.pdf)

* Last but not least:

  * [Stackoverflow](http://stackoverflow.com/questions/tagged/r)
