Lab 10: Random Variables
================
Constanza F. Schibber
November 30, 2019

# Today’s Agenda

1.  Law of Large Numbers
2.  The Central Limit Theorem and the Sampling Distribution of the Mean
3.  Sampling Distribution of the Variance of a Normal distribution

# Simulation for Law of Large Numbers

$$\bar{x_n} \xrightarrow{p} \mu$, as $n \rightarrow \infty$$

``` r
set.seed(89034)

LLN.sim<-function(n, event){
    mean(sample(event, size=n, replace=TRUE))  #take a sample (size=n) and calculate the mean value 
}
```

## Coin Flip

The following simulates a fair coin flip, where 0=H, 1=T , and plots the
results.  
How does the sample mean change as the sample size ($n$) increases?

**Increase n in the code below and re-run this code chunk. Look at the
changes in the figure**

``` r
coin.flip<-0:1   #This random variable takes a value, 0 or 1
n<-10
#par(mfrow=c(1,2))
plot(sapply(1:n, event=coin.flip, LLN.sim), type="l", xlab="Number of coin flips", ylab="Mean")
abline(h=0.5, col="red", lwd=3)    # Add a line at 0.5 (the expected value)
```

![](Lab10---Random-Variables_files/figure-gfm/LLN%20coin-1.png)<!-- -->

``` r
coin.flip<-0:1   #This random variable takes a value, 0 or 1
n<-100
#par(mfrow=c(1,2))
plot(sapply(1:n, event=coin.flip, LLN.sim), type="l", xlab="Number of coin flips", ylab="Mean")
abline(h=0.5, col="red", lwd=3)    # Add a line at 0.5 (the expected value)
```

![](Lab10---Random-Variables_files/figure-gfm/LLN%20coin%202-1.png)<!-- -->

## Fair Die

Let’s look at another example: Simulate a fair die.

**Increase `n` in the code below and re-run this code chunk. Look at the
changes in the figure.**

``` r
die<-1:6   #This random variable takes a value, 1,2,3,4,5 or 6
n<-10
plot(sapply(1:n, event=die, LLN.sim), type="l", xlab="Number of dice rolls", ylab="Mean")
abline(h=3.5, col="red", lwd=3)     # Add a line at 3.5  (the expected value) 
```

![](Lab10---Random-Variables_files/figure-gfm/LLN%20die-1.png)<!-- -->

``` r
die<-1:6   #This random variable takes a value, 1,2,3,4,5 or 6
n<-100
plot(sapply(1:n, event=die, LLN.sim), type="l", xlab="Number of dice rolls", ylab="Mean")
abline(h=3.5, col="red", lwd=3)     # Add a line at 3.5  (the expected value) 
```

![](Lab10---Random-Variables_files/figure-gfm/LLN%20die%202-1.png)<!-- -->

# The Central Limit Theorem and the Sampling Distribution of the Mean

$$\lim_{n \rightarrow \infty} Pr \left[\frac{\bar{X_n} - \mu}{\sigma/\sqrt{n}} \le x\right] = \Phi(x)$$

In the following example, let’s assume that samples are drawn from the
exponential distribution ($\lambda=1/5, n=10000$). The mean of this
exponetial distribution is $\frac{1}{\lambda} = 5$ and the varianance is
$\frac{1}{\lambda^2} = 25$.  
From this exponential distribution, we are drawing 1,000 random samples
with a fixed sample size ($n=30$), calculate the sample mean for each
sample, and plot them.

- What is the mean of this sampling distribution of the means?

- What is the variance of this sampling distribution of the means?

- What happens as $n$ increases?

``` r
# We will take samples from the exponential distribution
# value of lambda for the exponential distribution
lambda=0.2
M=10000
# the exponential distribution
exp.dist<-rexp(M, lambda)
# the mean of the exponential distribution is 1/lambda
mean(exp.dist)
```

    ## [1] 5.003318

``` r
# the variance of the exponential distribution is (1/lambda^2)
var(exp.dist)
```

    ## [1] 24.87586

``` r
#From this exponential distribution, we are drawing 1,000 samples with a fixed sample size (n=30), calculate the sample mean for each sample, and plot them. 

# empty vector to save means of samples
vector.of.means<- NULL

# Sample size n #The size of each sample is FIXED. In the Law of Large Numbers (LLN) we are increasing our sample size.
n<-30

# take the mean of N samples of the exponential distribution in order to create the sampling distribution 
N=1000
for(i in 1:N){
    vector.of.means<-c(vector.of.means, mean(rexp(n, lambda)))
}
```

Similarly, we can also get the sampling distribution of the sum.

``` r
# Sampling distribution of the sum
vector.of.sums<- NULL
for(i in 1:N){
    vector.of.sums<-c(vector.of.sums, sum(rexp(n, lambda)))
}
```

Now, let’s do a figure to compare the exponential distribution, the
sampling distribution of the mean, the sampling distribution of the sum,
and a histogram of one sample from the exponential distribution.

``` r
# To have this figure appear you need run the first line (par...) and then, click Zoom on the left and then select the code and run it. 
#plot
par(mfrow=c(2,2))
#Exponential Distribution from which samples are drawn
hist(exp.dist, col='blue', breaks=70, main='Exponential Distribution', freq=FALSE,xaxt="n", xlab="")
abline(v=mean(exp.dist), lwd=3, col='orange')
text(mean(exp.dist)+.5, .4, "mean")
axis(1, at=seq(from=0,to=max(exp.dist),by=5), labels=seq(from=0, to= max(exp.dist),by=5) )

#Sampling Distribution of the Mean
hist(vector.of.means, main="Density of the Sampling Distribution", breaks=100, xlab="", col="gray",freq=FALSE)
lines(density(vector.of.means), col="purple", lwd=3)
text(mean(vector.of.means)+0.5, .4, "mean")
abline(v=mean(vector.of.means), lwd=3, col='orange')

# Further, compare them to Distribution of one sample
hist(sample(rexp(n, lambda)),col="gray", main="Histogram of One Sample")

# Sampling Distribution of the Sum
hist(vector.of.sums,col="gray",main="Sampling Distribution of the Sum",breaks=100, xlab="",freq=FALSE)
lines(density(vector.of.sums), col="pink", lwd=3)
```

![](Lab10---Random-Variables_files/figure-gfm/CLT%20figure-1.png)<!-- -->

Comparing the mean and variance of the sampling distribution and the
mean and variance of the exponential distribution:

``` r
# Let's look at the mean and variance 
# Calculate the mean and variance of the sampling distribution 
# Is the mean (of the sampling distribution of the mean) equal to 1/lambda=5?
mean(vector.of.means)
```

    ## [1] 4.985136

``` r
#Is the variance (of the sampling distribution of the mean) equal to (1/lambda^2)/n = 25/30 = 0.84?  
var(vector.of.means)
```

    ## [1] 0.8438176

``` r
# Compare to the mean abd variance of the exponential distribution that samples were drawn
mean(exp.dist)
```

    ## [1] 5.003318

``` r
var(exp.dist)
```

    ## [1] 24.87586

``` r
var(exp.dist)/n  
```

    ## [1] 0.8291954

**Class Question:** What happens when the sample size ($n$) is 5, 50, or
100?

**Class Question:** Draw random samples from other distributions and see
what happens to the sampling distribution of the means when the sample
size increases.

We can create different distributions in as following (the default
values for parameters are shown):

| Distribution |  code                                 |
|--------------|---------------------------------------|
| Exponential  | `rexp(n, rate = 1 )`                  |
| Normal       | `rnorm(n, mean = 0 , sd = 1)`         |
| Uniform      | `runif(n, min = 0, max = 1)`          |
| Poisson      | `rpois(n, lambda)`                    |
| Cauchy       | `rcauchy(n, location = 0, scale = 1)` |
| Binomial     | `rbinom(n, size, prob)`               |
| Gamma        | `rgamma(n, rate = 1, scale = 1/rate)` |
| Chi-Squared  | `rchisq(n, df)`                       |
| Student t    | `rt(n, df)`                           |

we can write a function to compare the mean of the sample means for
diffent values of n. 

``` r
sampling.distribution<-function(sample.size, distribution, N.iter){
# empty vector to save means of samples
vector.of.means<- matrix(NA, nrow=N.iter)
for(i in 1:N.iter){
    sample.mean<-mean(sample(distribution, sample.size))
    vector.of.means[i]<-sample.mean
}
return(vector.of.means) 
}
```

Let’s apply the function.

``` r
#(1) Create a Distribution
# We will take samples from the exponential distribution
# value of lambda for the exponential distribution
lambda=0.2

# M=10000 iid samples from the exponential distribution
M=10000

# the exponential distribution
exp.dist<-rexp(M, lambda)

# the mean of the exponential distribution is 1/lambda
mean(exp.dist)
```

    ## [1] 4.984218

``` r
# the variance of the exponential distribution is (1/lambda^2)
var(exp.dist)
```

    ## [1] 25.09406

``` r
# (2) Select sample size
#From this exponential distribution, we are drawing 1,000 samples with a fixed sample size (n=30), calculate the sample mean for each sample, and plot them. 
n=30

# (3) take the mean of N samples of the exponential distribution in order to create the sampling distribution. Decide number of iterations or times you will sample from the distribution.
N.iter=1000

# (4) Apply function
vector.of.means<-sampling.distribution(n, exp.dist, N.iter)

# (5) Look at results

# Let's look at the mean and variance 
# Is the mean (of the sampling distribution of the mean) equal to 1/lambda=5?
mean(vector.of.means)
```

    ## [1] 4.992363

``` r
#Is the variance (of the sampling distribution of the mean) equal to (1/lambda^2)/n = 25/30 = 0.84?  
var(vector.of.means)
```

    ##           [,1]
    ## [1,] 0.8260406

``` r
#(6) Other, it turns out the sampling distribution of the sum is samples is also distributed normaly 
# Sampling distribution of the sum
vector.of.sums<- NULL
for(i in 1:N.iter){
    vector.of.sums<-c(vector.of.sums, sum(sample(exp.dist,n)))
}
```

Let’s try with different distribution. The below is an exmple of a
bimodal distribution.

``` r
# A bimodal distribution (example)
normal.1<- rnorm(M,0,1)
normal.2<- rnorm(M,10,1)
bimodal.dist<- c(normal.1, normal.2)

vector.of.means.bim<-sampling.distribution(15, bimodal.dist, N.iter)
mean(vector.of.means.bim)
```

    ## [1] 5.041215

``` r
var(vector.of.means.bim)
```

    ##          [,1]
    ## [1,] 1.708298

``` r
#plot
par(mfrow=c(1,2))
#Bimodal Distribution from which samples are drawn
hist(bimodal.dist, col='blue', breaks=70, main='Bimodal Distribution', freq=FALSE,xaxt="n", xlab="")
abline(v=mean(bimodal.dist), lwd=3, col='orange')
text(mean(bimodal.dist)+.5, .4, "mean")
axis(1, at=seq(from=0,to=max(bimodal.dist),by=5), labels=seq(from=0, to= max(bimodal.dist),by=5) )

#Sampling Distribution of the Mean
hist(vector.of.means.bim , main="Density of the Sampling Distribution", breaks=100, xlab="", col="gray",freq=FALSE)
lines(density(vector.of.means.bim), col="purple", lwd=3)
text(mean(vector.of.means.bim)+0.5, .4, "mean")
abline(v=mean(vector.of.means.bim ), lwd=3, col='orange')
```

![](Lab10---Random-Variables_files/figure-gfm/bimodal%20dist%20example-1.png)<!-- -->

``` r
##Try with differnt distribution.
```

**Class Question:** DeGroot Example 6.3.3 Read the Example and make sure
you understood.

``` r
2*pnorm(1.2, 0,1)-1
```

    ## [1] 0.7698607

# Sampling Distribution of the Variance of a Normal distribution

Next, for 1,000 random samples (n=30) drawn from the NORMAL distribution
($\mu=0$, $\sigma^2=2.3$), calculate the sample variances for each
sample, and plot them.

- What is the mean of this sampling distribution of the variance?  
- What is the variance of this sampling distribution of the variance?

``` r
mu<- 0
sigma<-sqrt(2.3)

M=10000
norm.dist<-rnorm(M, mu, sigma)

#From this normal distribution, we are drawing 1,000 samples with a fixed sample size (n=30), calculate the sample variance for each sample, and plot them. 

# empty vector to save variance of samples
vector.of.vars<- NULL

# Sample size n 
n<-5

# take the variance of N samples of the normal distribution.
N=1000
for(i in 1:N){
    vector.of.vars<-c(vector.of.vars, var(rnorm(n, mu, sigma)))
}

mean(vector.of.vars) #Does the mean of this sampling distribution approximates the variance of the normal distribution that samples were drawn?
```

    ## [1] 2.276749

``` r
var(vector.of.vars) 
```

    ## [1] 2.839652

``` r
#plot
par(mfrow=c(1,2))
#Normal Distribution that samples were drawn
hist(norm.dist, col='blue', breaks=70, main='Normal Distribution \n mean 0 and var 2.3 ', freq=FALSE,xaxt="n", xlab="")
axis(1, at=seq(from=min(norm.dist),to=max(norm.dist),by=5), labels=seq(from=min(norm.dist), to= max(norm.dist),by=5) )

#Sampling Distribution of the Variance 
hist(vector.of.vars, main="Density of the Sampling Distribution \n of the Variance", breaks=100, xlab="", col="gray",freq=FALSE)
lines(density(vector.of.vars), col="purple", lwd=3)
abline(v=mean(vector.of.vars), lwd=3, col='orange')
lines(density(rchisq(M, n-1)), col="yellow")
```

![](Lab10---Random-Variables_files/figure-gfm/sampling%20of%20variance,%20figure-1.png)<!-- -->
