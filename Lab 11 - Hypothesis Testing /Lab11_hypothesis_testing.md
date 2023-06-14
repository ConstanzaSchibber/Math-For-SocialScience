Lab 12: Hypothesis Testing
================
Constanza F. Schibber
December 7, 2017

# Today’s Agenda

1.  Working Example
2.  Hypothesis Tests for Means: A One-Sided Test  
3.  Hypothesis Tests for Means: A Two-Sided Test
4.  Two-Sample t-Tests
5.  Practice

# 1. Working Example: LaLonde’s(1986) data

We used the dataset in Lab 9.

``` r
library(cem)
```

    ## Loading required package: tcltk

    ## Loading required package: lattice

    ## 
    ## How to use CEM? Type vignette("cem")

``` r
data(LL)
#help(LL)
##Alternatively, you can use the data you downloaded.
#LL<- read.csv(LL.csv)
```

# 2. Hypothesis Tests for Means: A One-Sided Test

- One-tailed test  
  + $H_0: \mu \le \mu_0$  
  $H_A: \mu > \mu_0$ or  
  + $H_0: \mu \ge \mu_0$  
  $H_A: \mu < \mu_0$

- Two-tailed test  
  + $H_0: \mu = \mu_0$  
  $H_A: \mu \neq \mu_0$

(*Note*:$\mu$ is the mean in the distribution we drew a sample from;
$\mu_0$ is some numeric value we set )

## Example 1. One sided Test.

For example, we may hypothesize that, in 1974, the population of
long-term unemployed Americans had an income larger than \$6,059, a
government estimate of the mean income for the overall population of
americans.

In this case, our hypothesis is: (Fill in the blank to write out our
hypothesis)  
$H_0:$  
$H_A:$

What will your $\alpha_0$ be?

### t-Test Step by Step

``` r
# alpha0 and t-value for one sided test
alpha0<-0.05
n<- length(na.omit(LL$re74)) # consider possible missing values
df<- n-1
t0<-qt(1-alpha0, df) #because mu > mu0
t0
```

    ## [1] 1.64697

``` r
pt(t0, df)
```

    ## [1] 0.95

``` r
# components
mu.0<-6059
xbar.mu0<-mean(LL$re74)-mu.0
sigma<-sd(LL$re74)
# t value
t<-(n)^(1/2)* xbar.mu0/sigma
t
```

    ## [1] -10.48888

``` r
# p value 
1-pt(t, df)
```

    ## [1] 1

To reject $H_0$, we need $U > c$.

$U=-10.48 < T_{n-1}^{-1}(0.95)=1.64$ so we fail reject the null
hypothesis at the 0.05 significance level.

The $p$-value is 1 which is larger than our $\alpha_0$.

## `t.test`

In , we use `t.test(data, test value, alternative="two.sided")`. For
`alternative`, choose either `two.sided`, `greater`, or `less`.

``` r
t.test (LL$re74, 
        mu=6059, 
        alternative="greater", ## alternative hypothesis is that the population mean to be less than the quantity presented
        conf.level = 0.95) 
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  LL$re74
    ## t = -10.489, df = 721, p-value = 1
    ## alternative hypothesis: true mean is greater than 6059
    ## 95 percent confidence interval:
    ##  3249.451      Inf
    ## sample estimates:
    ## mean of x 
    ##  3630.738

It shows that the value of our $t$-ratio is $-10.4889$ (with $df=721$),
and $p$-value is $1$.

## Example 2. One sided Test.

For example, we may hypothesize that, in 1974, the population of
long-term unemployed Americans had an income lower than \$6,059, a
government estimate of the mean income for the overall population of
americans.

In this case, our hypothesis is: (Fill in the blank to write out our
hypothesis)  
$H_0:$  
$H_A:$

What will your $\alpha_0$ be?

## t-Test Step by Step

``` r
# alpha0 and t-value for one sided test
alpha0<-0.05
n<- length(na.omit(LL$re74)) # consider possible missing values
df<- n-1
t0<-qt(alpha0, df) # This because H1: mu < mu0
t0
```

    ## [1] -1.64697

``` r
pt(t0, df)
```

    ## [1] 0.05

``` r
# components
mu.0<-6059
xbar.mu0<-mean(LL$re74)-mu.0
sigma<-sd(LL$re74)
# t value
t<-(n)^(1/2)* xbar.mu0/sigma
t
```

    ## [1] -10.48888

``` r
# p value 
pt(t, df)
```

    ## [1] 2.362483e-24

$-10.48 < T_{n-1}^{-1}(0.05)=-1.64$ so we reject the null hypothesis at
the 0.05 significance level. The $p$ value is smaller than $\alpha_0$.

## `t.test`

In , we use `t.test(data, test value, alternative="two.sided")`. For
`alternative`, choose either `two.sided`, `greater`, or `less`.

``` r
t.test (LL$re74, 
        mu=6059, 
        alternative="less", ## alternative hypothesis is that the population mean to be less than the quantity presented
        conf.level = 0.95) 
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  LL$re74
    ## t = -10.489, df = 721, p-value < 2.2e-16
    ## alternative hypothesis: true mean is less than 6059
    ## 95 percent confidence interval:
    ##      -Inf 4012.025
    ## sample estimates:
    ## mean of x 
    ##  3630.738

It shows that the value of our $t$-ratio is $-10.4889$ (with $df=721$),
and $p$-value is $2.2e-16$. Is the $p$-value lower than the $\alpha_0$
you set before conductingt the test? If so, you can reject the null
hypothesis and conclude that long-term unemployed Americans has
significantly lower income than \$6,059 in 1974.

# Two-Sample Difference of Means Test, Independent Samples

- One-tailed test  
  + $H_0: \mu_x \leq \mu_y$  
  $H_A: \mu_x > \mu_y$  
  or  
  + $H_0: \mu_x \geq \mu_y$  
  $H_A: \mu_x < \mu_y$

- Two-tailed test  
  + $H_0: \mu_x = \mu_y$  
  $H_A: \mu_x \neq \mu_y$

(*Note*:$\mu_x$ is the mean of the first sample $x$ and $\mu_y$ is the
mean of the second sample $y$.)

For example, we may hypothesize that income in 1978 was higher among
individuals who received the treatment of participating in the program
($y$), than it was among those who were control observations and did not
get to participate in the program ($x$). In this case, our hypothesis
is: (Fill in the blank to write out our hypothesis)

$H_0:$  
$H_A:$

## t-Test Step by Step

``` r
# alpha0 and t-value for one sided test
alpha0<-0.05
m<- length(na.omit(LL$re78[LL$treated==1]))
n<- length(na.omit(LL$re78[LL$treated==0]))
df<- m+n-2
t0<-qt(1-alpha0, df)
pt(t0, df)
```

    ## [1] 0.95

``` r
# components
xbar.ybar<-mean(LL$re78[LL$treated==0])-mean(LL$re78[LL$treated==1])
# S
S2<-function(x){
    row<-length(na.omit(x))
    pol<-matrix(NA, nrow=row)
  for(i in 1:row){
     pol[i]<-(x[i]-mean(x))^2
  }
return(sum(pol))
}
# Apply S
Sx2<-S2(LL$re78[LL$treated==1])
Sy2<-S2(LL$re78[LL$treated==0])
# t value
t<-((df)^(1/2)* xbar.ybar)/((1/m+1/n)^(1/2)*(Sx2+Sy2)^(1/2))
t
```

    ## [1] -1.877419

``` r
# p value 
pt(t, df)
```

    ## [1] 0.03043236

## `t.test`

In , we use
`t.test(values ~ groups, alternative="two.sided", var.equal=FALSE)`. For
`alternative`, choose either `two.sided`, `greater`, or `less`. For
`var.equal`, choose either `FALSE` or `TRUE`.

``` r
t.test (LL$re78 ~ LL$treated,     ## income in 1978 (re78) is being separated based on values of the treatment indicator
        alternative="less", ##the average income for the control group (`treated`=0) is less than the average income for the treatment group (`treated`=1) 
        var.equal=T)    ## equal variance is assumed  
```

    ## 
    ##  Two Sample t-test
    ## 
    ## data:  LL$re78 by LL$treated
    ## t = -1.8774, df = 720, p-value = 0.03043
    ## alternative hypothesis: true difference in means between group 0 and group 1 is less than 0
    ## 95 percent confidence interval:
    ##       -Inf -108.7906
    ## sample estimates:
    ## mean in group 0 mean in group 1 
    ##        5090.048        5976.352

``` r
#alternatively, we can write as following:
#t.test (re78 ~ treated, data=LL,   
#        alternative="less", 
#        var.equal=T)  
```

It shows that the value of our $t$-ratio is $-1.8774$ (with $df=720$),
and $p$-value is $.030$.

Hence, we reject the null hypothesis at the 0.05 significance level.
Note this test was assumed equal variance of the two samples.

# Practice Exercises

## Exercise 1

For more exercises, let’s use a different dataset. We are going to use
Alvarez et al.’s (2013) data (alpl2013.dta). The data are from a field
experiment in Salta, Argentina in which some voters cast ballots through
e-voting, and others voted in the traditional setting. See p.76-77 in
Monogan for descriptions of variables.

``` r
#download the data from Piazza and read 
library(foreign)
#alpl2013<- read.dta("alpl2013.dta")
```

1.  Consider the number of technomogical devices `tech`). Test the
    hypothesis that the average Salta voter has used more than three of
    these six devices. In this case, our hypothesis is: (Fill in the
    blank to write out our hypothesis)  
    $H_0:$  
    $H_A:$

2.  Conduct two independent sample difference of means tests:

<!-- -->

1.  Is there any difference between men and women (`male`) in how many
    technological devices they have used?
2.  Is there any difference in how positively voters view the voting
    experience (`eval_voting`) based on whether they used e-voting or
    traditional voting (`EV`)?

## Exercise 2: DeGroot

*Section 9.5., Exercise 1 (p.585)*

The following data comprises a sample of $n=10$ measures of ideology.
Assume that the ideology measures are a random sample from the normal
distribution with unknown mean and unknown variance ^2. Suppose that
greater than 1.2 represents conservative and we wish to test the
following: $H_0: \mu \le 1.2$  
\$H_A: \> 1.2 \$  
Perform the level \_a =0.05 test and the $p$-value. Provide an
interpretation of your results.

``` r
#data
ex1 <- c(0.86, 1.53, 1.57, 1.81, 0.99, 1.09, 1.29, 1.78, 1.29, 1.58)

#hypotheses test at alpha=0.05


# compute the p-value
```

# Exercise 3: Obama Feeling Thermometer

From the ANES data: Recode the variable `partyid`: The variable partyid
takes 5 values: 1 (democrat), 2 (republican), 3 (independent), 4 (other
party), 5 (no preference).

``` r
#anes<-read.csv("anes2008.csv")
#dim(anes)
# recode variable
#library(car)
#summary(as.factor(anes$partyid))
#party.id<-recode(anes$partyid, "4=3; 5=3")
#summary(as.factor(party.id))
# Only democrats and republicans
#anes.DR<-subset(anes, party.id!=3)
#dim(anes.DR)
```

Explain the following hypotheses, perform the corresponding tests, and
explain and interpret the results.

## Obama Feeling Thermometer: One sided t-Test (1)

$$H_0: \mu_R \geq \mu_D$$ $$H_1: \mu_R < \mu_D$$

## Obama Feeling Thermometer: One sided t-Test (2)

$$H_0: \mu_R \leq \mu_D$$ $$H_1: \mu_R > \mu_D$$

## Obama Feeling Thermometer: Two sided t-Test

$$H_0: \mu_R = \mu_D$$ $$H_1: \mu_R \neq \mu_D$$
