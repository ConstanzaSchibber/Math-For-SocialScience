Integrals in R
================
Constanza F. Schibber
11/1/2017

## Example from Jeff Gill’s Book

Gill calculates the area under the intersection of two parabolas. Let’s
do the same calculation in `R`!

``` r
## Create the functions for voter 2 and voter 3

V2<-function(x){10-(3.5-x)^2*6}
V3<-function(x){10-(5-x)^2*2.5}

## 
## Problem 1. Where do the parabolas cross x=0
##

## Find where the parabola for voter 2 intersects with x=0

# Intersection above the mean of V2
V2.upper<-uniroot(V2, lower=3.5, upper=10)

# Intersection below the mean of V2
V2.lower<-uniroot(V2, lower=0, upper=3.5)


# Intersection above the mean of V3
V3.upper<-uniroot(V3, lower=5, upper=10)

# Intersection below the mean of V3
V3.lower<-uniroot(V3, lower=0, upper=5)

##
## Problem 2 - Where do the parabolas intersect? 
##

V2.V3<-uniroot(function(x){3.5*x^2-17*x+11}, lower=3.5, upper=5)
```

With all the information we can create a figure and shade the area we
want to calculate.

``` r
# values 
rule<-seq(from=0, to=10, by=0.1)

# for polygon input - shade on figure
shade.x<-seq(from =V3.lower$root,to=V2.upper$root, by=0.1)
shade.y<-seq(V3(shade.x))

# plot
plot("n", xlim=c(0,8), ylim=c(0,10), xlab="x", ylab="Voter Utility")
```

    ## Warning in xy.coords(x, y, xlabel, ylabel, log): NAs introduced by coercion

``` r
# add parabolas for voter 2 and voter 3
lines(rule, V2(rule), col='red', lwd=3)
lines(rule, V3(rule), col='purple', lwd=3)
# shade area under the curves
polygon(c(V3.lower$root, V2.V3$root, V2.V3$root), c(0, 0, V3(V2.V3$root)), col="purple", density=50)
polygon(c(V2.V3$root, V2.upper$root, V2.V3$root), c(0, 0, V3(V2.V3$root)), col="red", density=30)
# add line at x=0
abline(h=0, lty=3, col='gray')
#segments(V2.V3$root, 0, V2.V3$root, 8, lwd=3, lyt=3, col='yellow')
text(2, 8, "Voter 2", col='red')
text(7, 8, "Voter 3", col='purple')
mtext("3", side=1, at=V3.lower$root, cex=.8)
mtext("4.09", side=1, at=V2.V3$root, cex=.8)
mtext("4.79", side=1, at=V2.upper$root, cex=.8)
```

![](Lecture_IntegralsinR_files/figure-gfm/Figure-1.png)<!-- -->

Now, we can calcuate the area under the curve using `integrate` which
comes in `base` R.

``` r
# calculate area under voter 3
IntegrateV3<-integrate(V3, lower=V3.lower$root, upper=V2.V3$root)
IntegrateV3
```

    ## 4.848776 with absolute error < 5.4e-14

``` r
# calculate area under voter 2
IntegrateV2<-integrate(V2, lower=V2.V3$root, upper=V2.upper$root)
IntegrateV2
```

    ## 3.129907 with absolute error < 3.5e-14

``` r
# add the areas for the final result
4.848776+3.129907
```

    ## [1] 7.978683

There is a package called `pracma` you can install. It also calculates
integrals and it works better with more complex functions.

``` r
##
#library(pracma)
#integral(V3, xmin=V3.lower$root, xmax=V2.V3$root)
#integral(V2, xmin=V2.V3$root, xmax=V2.upper$root)
```
