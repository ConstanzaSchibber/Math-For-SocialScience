Lab 2: Reading and Visualizing Data
================
Constanza F. Schibber
9/7/2017

- [Reading Data: The Basics](#reading-data-the-basics)
- [A Working Example](#a-working-example)
  - [Logical Statements and Variable
    Generation](#logical-statements-and-variable-generation)
  - [Cleaning Data](#cleaning-data)
  - [Visualizing Data](#visualizing-data)
    - [The `plot` Function](#the-plot-function)
    - [Layering plots](#layering-plots)
  - [Practice Problem](#practice-problem)

# Reading Data: The Basics

There are a lot of files types. To read datasets, we have to use the
correct function depending on the type of file we are trying to open.
The following are the most common file types used for datasets and the R
`code` to open them. You have to specify where the file is located by
including the file path in the function. The path could be to where the
file is saved in your computer or a website.

| Types of files               |   R code (a path /a file name in the parentheses)    |
|------------------------------|:----------------------------------------------------:|
| space-delimited files        |                `read.table("     ")`                 |
| comma-separated values files |                 `read.csv("     ")`                  |
| tab-delimited files          | `read.table(" ", sep='\t')` or `read.delim("     ")` |
| STATA data files             |        `library(foreign); read.dta("     ")`         |
| SPSS data files              |        `library(foreign); read.spss("     ")`        |

# A Working Example

Our working example will be a subset of Poe et al.’s (1999) **Political
Terror Scale** data on human rights, which is an update of the data in
Poe and Tate (1994). Whereas their complete data cover 1976–1993, we
focus solely on the year 1993. The eight variables this dataset contains
are:

1.  *country*: A character variable listing the country by name.
2.  *democ*: The country’s score on the Polity III democracy scale.
    Scores range from 0 (least democratic) to 10 (most democratic).
3.  *sdnew*: The U.S. State Department scale of political terror. Scores
    range from 1 (low state terrorism, fewest violations of personal
    integrity) to 5 (highest violations of personal integrity).
4.  *military*: A dummy variable coded 1 for a military regime, 0
    otherwise.
5.  *gnpcats*: Level of per capita GNP in five categories: 1 = under
    \$1000, 2 = \$1000–\$1999, 3 = \$2000–\$2999, 4 = \$3000–\$3999, 5 =
    over \$4000.
6.  *lpop*: Logarithm of national population.
7.  *civ_war*: A dummy variable coded 1 if involved in a civil war, 0
    otherwise. 8. *int_war*: A dummy variable coded 1 if involved in an
    international war, 0 otherwise.

``` r
#load human rights data in space-separated format from the web
hmnrghts<-read.table("hmnrghts.txt", header=TRUE, na="NA")

#print all of the data (It takes a LOT of space!)
#hmnrghts

#print the top of the data
head(hmnrghts)
```

    ##      country democ sdnew military   gnpcats  lpop civ_war int_war
    ## 1 afganistan    NA     5        1     <1000 16.67       1       0
    ## 2    albania     8     2        0     <1000 15.04       0       0
    ## 3    algeria     0     5        0 1000-1999 17.12       0       0
    ## 4     angola     0     5        0     <1000 16.20       1       0
    ## 5  argentina     8     2        0     >4000 17.33       0       0
    ## 6  australia    10     1        0     >4000 16.69       0       0

``` r
#get the variable names
names(hmnrghts)
```

    ## [1] "country"  "democ"    "sdnew"    "military" "gnpcats"  "lpop"     "civ_war" 
    ## [8] "int_war"

``` r
colnames(hmnrghts)
```

    ## [1] "country"  "democ"    "sdnew"    "military" "gnpcats"  "lpop"     "civ_war" 
    ## [8] "int_war"

``` r
rownames(hmnrghts)
```

    ##   [1] "1"   "2"   "3"   "4"   "5"   "6"   "7"   "8"   "9"   "10"  "11"  "12" 
    ##  [13] "13"  "14"  "15"  "16"  "17"  "18"  "19"  "20"  "21"  "22"  "23"  "24" 
    ##  [25] "25"  "26"  "27"  "28"  "29"  "30"  "31"  "32"  "33"  "34"  "35"  "36" 
    ##  [37] "37"  "38"  "39"  "40"  "41"  "42"  "43"  "44"  "45"  "46"  "47"  "48" 
    ##  [49] "49"  "50"  "51"  "52"  "53"  "54"  "55"  "56"  "57"  "58"  "59"  "60" 
    ##  [61] "61"  "62"  "63"  "64"  "65"  "66"  "67"  "68"  "69"  "70"  "71"  "72" 
    ##  [73] "73"  "74"  "75"  "76"  "77"  "78"  "79"  "80"  "81"  "82"  "83"  "84" 
    ##  [85] "85"  "86"  "87"  "88"  "89"  "90"  "91"  "92"  "93"  "94"  "95"  "96" 
    ##  [97] "97"  "98"  "99"  "100" "101" "102" "103" "104" "105" "106" "107" "108"
    ## [109] "109" "110" "111" "112" "113" "114" "115" "116" "117" "118" "119" "120"
    ## [121] "121" "122" "123" "124" "125" "126" "127" "128" "129" "130" "131" "132"
    ## [133] "133" "134" "135" "136" "137" "138" "139" "140" "141" "142" "143" "144"
    ## [145] "145" "146" "147" "148" "149" "150" "151" "152" "153" "154" "155" "156"
    ## [157] "157" "158"

``` r
#descriptive statistics for full dataset
summary(hmnrghts)
```

    ##    country              democ            sdnew          military     
    ##  Length:158         Min.   : 0.000   Min.   :1.000   Min.   :0.0000  
    ##  Class :character   1st Qu.: 0.000   1st Qu.:1.000   1st Qu.:0.0000  
    ##  Mode  :character   Median : 7.000   Median :2.000   Median :0.0000  
    ##                     Mean   : 5.323   Mean   :2.481   Mean   :0.1538  
    ##                     3rd Qu.:10.000   3rd Qu.:4.000   3rd Qu.:0.0000  
    ##                     Max.   :10.000   Max.   :5.000   Max.   :1.0000  
    ##                     NA's   :31       NA's   :2       NA's   :2       
    ##    gnpcats               lpop          civ_war           int_war       
    ##  Length:158         Min.   :11.29   Min.   :0.00000   Min.   :0.00000  
    ##  Class :character   1st Qu.:14.65   1st Qu.:0.00000   1st Qu.:0.00000  
    ##  Mode  :character   Median :15.89   Median :0.00000   Median :0.00000  
    ##                     Mean   :15.66   Mean   :0.09615   Mean   :0.02564  
    ##                     3rd Qu.:16.85   3rd Qu.:0.00000   3rd Qu.:0.00000  
    ##                     Max.   :20.89   Max.   :1.00000   Max.   :1.00000  
    ##                     NA's   :2       NA's   :2         NA's   :2

``` r
#descriptive statistics for log of population (a variable in the dataset)
summary(hmnrghts$lpop)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##   11.29   14.65   15.89   15.66   16.85   20.89       2

Class Questions:

1.  The dataset includes *lpop*, but you want to know the minimum and
    maximum population size. How would you do that?
2.  Explain the table of the variable *dem.civ*

## Logical Statements and Variable Generation

``` r
#create indicator for more democratic societies in a civil war
hmnrghts$dem.civ <- as.numeric(hmnrghts$civ_war==1 & hmnrghts$democ>5.3)

#view which countries fit both criteria
hmnrghts$country[hmnrghts$dem.civ==1]
```

    ## [1] NA            "columbia"    NA            "india"       NA           
    ## [6] "philippines" NA            "turkey"      NA

``` r
#view the frequency of values on this indicator
table(hmnrghts$dem.civ)
```

    ## 
    ##   0   1 
    ## 149   4

``` r
#Class Question: How many values of democracy are missing?
table(is.na(hmnrghts$democ))
```

    ## 
    ## FALSE  TRUE 
    ##   127    31

``` r
#Is our dataset saved as a matrix?
is.matrix(hmnrghts)
```

    ## [1] FALSE

``` r
#Is our dataset saved as a data frame?
is.data.frame(hmnrghts)
```

    ## [1] TRUE

## Cleaning Data

The data has a lot of missing values. Although it is not best practice,
we can delete the observations (rows) with missing data like this:

``` r
#listwise deletion of any observation missing any variable
hmnrghts.trim <- na.omit(hmnrghts)
```

We can also subset the data either by filtering by a value of a column
or by only selecting a number of columns.

``` r
#subset to countries with a democracy score of 6-10
dem.rights <- subset(hmnrghts, subset=democ>5)

#subset to country, democracy, and wealth as only variables
dem.wealth<-subset(hmnrghts,select=c(country, democ, gnpcats))

#subset to all variables except the war indicators
no.war <- subset(hmnrghts,select=-c(civ_war,int_war))
```

We can also recode variables or create new variables:

``` r
#convert logged population to raw population
hmnrghts$pop <- exp(hmnrghts$lpop)

#ordinal variable of how many types of wars a country is involved in
hmnrghts$war.ord<-hmnrghts$civ_war+hmnrghts$int_war

#create numeric ordinal scale of national wealth in six lines 
# The following is the "harder" way to recode the variable. It is easier to use the "recode" function in the "car" library (see below)
hmnrghts$gnp.ord <- NA
hmnrghts$gnp.ord[hmnrghts$gnpcats=="<1000"]<-1
hmnrghts$gnp.ord[hmnrghts$gnpcats=="1000-1999"]<-2
hmnrghts$gnp.ord[hmnrghts$gnpcats=="2000-2999"]<-3
hmnrghts$gnp.ord[hmnrghts$gnpcats=="3000-3999"]<-4
hmnrghts$gnp.ord[hmnrghts$gnpcats==">4000"]<-5
```

The car library provides an easy way of recoding variables:

``` r
#install and load the "car" package
#install.packages("car")
library(car)
```

    ## Loading required package: carData

``` r
#create numeric ordinal scale of national wealth with recode command (car package)
hmnrghts$gnp.ord.2<-recode(hmnrghts$gnpcats,'"<1000"=1;"1000-1999"=2;"2000-2999"=3;"3000-3999"=4;">4000"=5')
```

## Visualizing Data

### The `plot` Function

``` r
#See the total sum of arguments of `plot`
args(plot.default)
```

    ## function (x, y = NULL, type = "p", xlim = NULL, ylim = NULL, 
    ##     log = "", main = NULL, sub = NULL, xlab = NULL, ylab = NULL, 
    ##     ann = par("ann"), axes = TRUE, frame.plot = axes, panel.first = NULL, 
    ##     panel.last = NULL, asp = NA, xgap.axis = NA, ygap.axis = NA, 
    ##     ...) 
    ## NULL

``` r
#univariate
plot(hmnrghts$democ) 
```

![](02Lab_PLS801_files/figure-gfm/plot%20univariate%20and%20bivariate-1.png)<!-- -->

``` r
#a bivariate scatterplot
plot(y=hmnrghts$democ, x=hmnrghts$lpop)
```

![](02Lab_PLS801_files/figure-gfm/plot%20univariate%20and%20bivariate-2.png)<!-- -->

``` r
#Using more arguments of plot. Never use the default!
plot(y=hmnrghts$democ, x=hmnrghts$lpop,
     ylab="Polity III Democracy Scale",         # label the y-axis
     xlab="Logarithm of National Population",   # label the x-axis
     asp=1,                                     # aspect ratio=1
     pch=20,                                    # change type of point
     col='purple')                              # color                         
```

![](02Lab_PLS801_files/figure-gfm/Using%20more%20arguments%20of%20plot-1.png)<!-- -->

``` r
#You can also reset the tick marks on the axes 
plot(y=hmnrghts$democ, x=hmnrghts$lpop,
     ylab="Polity III Democracy Scale",        
     xlab="Logarithm of National Population",   
     axes=F)                   # axes = FALSE means we do not want them
axis(1, at=c(14, 16, 18, 20), labels= round(c(14, 16, 18, 20),0),    # specify x-axis 
     cex.axis=.7)  
axis(2)  # specify y-axis 
box()
```

![](02Lab_PLS801_files/figure-gfm/Changing%20the%20axes,%20adding%20a%20box,%20adding%20a%20line-1.png)<!-- -->

**Class Question: Can you put the row number of national population
instead of the log(pop) on the x-axis?**

![](02Lab_PLS801_files/figure-gfm/Transformation%20of%20x-1.png)<!-- -->

### Layering plots

``` r
plot("n", #this gives you an empty plot
     xlim=c(0,100), 
     ylim=c(1, 80),
     xlab="My x Variable",
     ylab="My y Variable")
```

    ## Warning in xy.coords(x, y, xlabel, ylabel, log): NAs introduced by coercion

``` r
abline(a=1,  b=.5, lty=1) # A line with intercept a and slope b
abline(a=10, b=.5, lty=2) # A line with intercept a and slope b
abline(a=20, b=.5, lty=3) # A line with intercept a and slope b
abline(a=30, b=.5, lty=4) # A line with intercept a and slope b
abline(a=40, b=.5, lty=5) # A line with intercept a and slope b
abline(a=50, b=.5, lty=5, lwd=1) # A line with intercept a and slope b
abline(a=60, b=.5, lty=5, lwd=2) # A line with intercept a and slope b
abline(a=70, b=.5, lty=5, lwd=3) # A line with intercept a and slope b
points(x=1, y=20, pch=1, col='blue')
points(x=5, y=20, pch=2, col='cyan1')
points(x=10, y=20, pch=3, col='cyan2')
points(x=15, y=20, pch=4, col='azure4')
points(x=20, y=20, pch=5, col='bisque3')
points(x=25, y=20, pch=6, col='blue')
points(x=30, y=20, pch=7, col='blueviolet')
points(x=35, y=20, pch=8, col='brown')
points(x=40, y=20, pch=9, col='cadetblue')
points(x=45, y=20, pch=10, col='chartreuse')
points(x=50, y=20, pch=11, col='chocolate1')
```

![](02Lab_PLS801_files/figure-gfm/Layers%20of%20a%20plot-1.png)<!-- -->

**Add more point types in different colors.** You can go [pick some
other colors](http://research.stowers.org/mcm/efg/R/Color/Chart/) and
[types of
points](http://www.sthda.com/english/wiki/r-plot-pch-symbols-the-different-point-shapes-available-in-r).
\*\* Add lines with different slopes (positive, negative, big, small,
zero) and intercepts.\*\* Understand how the lines move with the changes
you make. You might have to change the length of the x-axis and y-axis
if the lines do not appear on the panel.

## Practice Problem

As a practice dataset, we will download and open a subset of the 2004
American National Election Study used by Hanmer and Kalkan (2013). This
dataset is available from the Dataverse referenced on page vii or in the
chapter content link on page 13. These data are in Stata format, so be
sure to load the correct library and use the correct command when
opening. (Hint: When using the proper command, be sure to specify the
convert.factors=F option within it to get an easier-to-read output.) The
variables in this dataset all relate to the 2004 U.S. presidential
election, and they are: a respondent identification number (caseid),
retrospective economic evaluations (retecon), assessment of George W.
Bush’s handling of the war in Iraq (bushiraq), an indicator for whether
the respondent voted for Bush (presvote), partisanship on a seven-point
scale (partyid), ideology on a seven-point scale (ideol7b), an indicator
of whether the respondent is white (white), an indicator of whether the
respondent is female (female), age of the respondent (age), level of
education on a seven-point scale (educ1_7), and income on a 23-point
scale (income). (The variable exptrnout2 can be ignored.)

1.  Once you have loaded the data, do the following to check your work:

<!-- -->

1)  If you ask R to return the variable names, what does the list say?
    Is it correct?
2)  Using the `head` command, what do the first few lines look like?
3)  If you use the `fix` command, do the data look like an appropriate
    spreadsheet?

<!-- -->

2.  Use the `summary` command on the whole data set. What can you learn
    immedi- ately? How many missing observations do you have?

3.  Create a bivariate plot between `ideol7b` and `partyid`. Do not do
    the default!
