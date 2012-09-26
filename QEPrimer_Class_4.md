# Class 4: Confidence intervals for univariate data

## Set reading for this class

Crawley chapters four and five.

**What you must remember before next term**
 
*  How to calculate a standard error
*  The difference between the standard error and the standard deviation
*  The concept underlying the sampling distribution of the mean
*  The use of the t-distribution for small (n<30) samples
*  How to calculate a confidence interval
 
 **What you will look at in greater depth on the course**
 
*  The logic underlying statistical inference from samples
*  The relationship between confidence intervals, hypothesis tests and statistical models
*  The interpretation of confidence intervals
*  Diagnostics for confidence interval calculations
*  Confidence intervals for non-normal data 

### Calculating the standard error

In the last class you calculated the standard deviation, step by step. Load and attach the data you were working with, if you have not already done so by reloading the R workspace%
*Note that if you reload data or attach the same data twice you will be warned by R. This will not usually cause a problem, but you should be aware that if the data are different there may be the potential for ambiguity. Be careful*

 


```r
d <- read.csv("http://tinyurl.com/QEcol2013/Mussels.csv")
attach(d)
```





The standard deviation summarises the variability in the data (the deviations from the mean) in one single value. This can be very useful for descriptive purposes.

A rather more subtle concept is the standard error. The subtlety arises because the standard error is used for inference. Remember that statistical inference involves estimating some properties of the population from which our sample has been drawn. The standard error (of the mean) provides us with information regarding the confidence we can place on the estimate of the population mean.

The standard error is very easily calculated if we have the sample standard deviation. It is simply the standard deviation divided by the square root of the sample size.

$SE_{\tilde{x}}=\frac{s}{\sqrt{n}}$



```r
n <- length(Lshell)
se <- sd(Lshell)/sqrt(n)
se
```

```
## [1] 1.396
```





However the interpretation of the standard error is more subtle. The standard error is based on the concept of the sampling distribution. It is in effect the standard deviation of the distribution of the means we would get if we continually drew samples from a target population. 


### The sampling distribution of the mean

The sampling distribution of the mean is the distribution that you would obtain if you took a large number of samples of the same size as the one you actually obtained and looked at the distribution of all the mean values.

To make this clearer let's try a simple experiment.

In R we can draw random samples from a column of data easily. Lets take a sample of 10 mussel shells at random, look at their lengths and calculate the mean.



```r
samp <- sample(Lshell, 10)
samp
```

```
##  [1] 106.0 119.5 108.2  89.8 124.2 111.5 109.2  97.0 112.6 115.8
```

```r
mean(samp)
```

```
## [1] 109.4
```




We could repeat this experiment many times. You could do it again by repeating the code.



```r
samp <- sample(Lshell, 10)
samp
```

```
##  [1]  91.1 102.3  96.8 111.9  94.7 103.3  85.6  73.8 115.5 129.9
```

```r
mean(samp)
```

```
## [1] 100.5
```




You can see that the result is not the same, as the sample was taken at random.

We can tell R to take 1000 samples like this from the original vector, calculate the mean from each and then look at the distribution of all the resulting thousand values. This would simulate the spread in values for the mean that we would obtain if we carried out a lot of small surveys that took only 10 mussel shells from the same population




```r
resamp <- replicate(1000, mean(sample(Lshell, 10)))
hist(resamp, breaks = 20, xlim = c(90, 120), main = "Sampling distribution of the means of 10 shells")
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 



Now if we draw samples of 30 shells instead of 10 and calculate the mean for each, what would we expect? Let's try this and plot the result.



```r
resamp <- replicate(1000, mean(sample(Lshell, 30)))
hist(resamp, breaks = 25, xlim = c(90, 120), main = "Sampling distribution of the means of 30 shells")
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6.png) 



A sample size of 50 should reduce the spread of mean values still further. 



```r
resamp <- replicate(1000, mean(sample(Lshell, 50)))
hist(resamp, breaks = 40, xlim = c(90, 120), main = "Sampling distribution of the means of 50 shells")
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7.png) 



Notice that in all cases the histogram suggests a symmetrical, normal distribution. The distribution becomes narrower as the sample size increases.

So the outcome of all this is to demonstrate that the standard error is a manner of formalising this so called ``sampling'' process mathematically. It provides a measure of the confidence we have in an estimate of the mean value calculated from our sample. 

For large samples, a 95\% confidence interval for the mean is produced by multiplying the standard error by approximately 2 (the precise value is 1.96 for infinitely large samples). We expect the true mean for the population to fall within this interval 95\% of the time. Notice that this is not a measure of variability (there is only one true mean). It is an estimate of a parameter expressed with uncertainty. So there is a very important distinction between the standard deviation and the standard error. 

We can reduce the standard error by increasing the sample size, as we divide by the square root of n. However we can't reduce the standard deviation by drawing a larger sample. The sd is a measure of the natural variability which is always present. All that happens to the standard deviation when we take a small sample is that we have a worse estimate of it, as the differences between individuals may vary randomly depending on which we happen to pick. 

Recall the way we simulated some artificial data in R. To get each simulated height we used the mean + the simulated variability with known standard deviation



```r
mean <- 176
variability <- rnorm(1, sd = 6)  #Each measurement will differ from the mean
height <- mean + variability  #The values we get are a combination of the two
```




The intrinsic variability is often referred to as the ``error'' in statistics, but it this is not necessarily an error in the colloquial sense of a mistaken measurement. The true population mean is fixed, it does not vary. However our estimate of it does, unless we measure every individual in the population. We draw inferences about it from the sample. This is the basis of many classical statistical tests.


### Calculating confidence intervals

Whenever you see a standard error in the published literature remember that the rough rule of thumb is that two standard errors represent a 95\% confidence interval for the mean. However it is only a rough estimate. As samples become smaller you need to multiply the standard error by a larger value.

Small sample (n<30) inference relies on the t distribution. The t-distribution is wider than the normal distribution and corrects for the fact that we have to estimate the population standard deviation from the sample.

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 



The t distribution becomes fatter as the sample size becomes smaller (degrees of freedom = n-1) so we need to multiply the SE by a larger value in order to obtain the same confidence interval. Notice that small samples therefore lead to a ``double whammy''. The SE is large because n is in the denominator when we calculate it from the sd, and the value for t is larger because we have a worse estimate of the standard deviation from the sample.

So to get a 95\% confidence interval with a sample of size 10 we multiply the standard error by t as found by the R function below, instead of using 1.96 which would be the case for a large sample.



```r
t <- qt(0.975, 10 - 1)
t
```

```
## [1] 2.262
```




This cuts off the tails of the t-distribution.

To calculate a confidence interval step by step in order to illustrate the concept we need the following elements.



```r
SD <- sd(Lshell)
SD
```

```
## [1] 14.84
```

```r
Mean <- mean(Lshell)
Mean
```

```
## [1] 106.8
```

```r
n <- length(Lshell)
n
```

```
## [1] 113
```

```r
SE <- SD/sqrt(n)
SE
```

```
## [1] 1.396
```

```r
t <- qt(0.975, n - 1)
t
```

```
## [1] 1.981
```




The confidence interval is then calculated using the standard error of the mean and the value for t that corresponds to the cut off point of the distribution.



```r
Mean - SE * t
```

```
## [1] 104.1
```

```r
Mean + SE * t
```

```
## [1] 109.6
```




**See Crawley, chapter 4 for more details.**

To get a confidence interval in one step uning R we can run a one sample t-test.



```r
t.test(Lshell)
```

```
## 
## 	One Sample t-test
## 
## data:  Lshell 
## t = 76.51, df = 112, p-value < 2.2e-16
## alternative hypothesis: true mean is not equal to 0 
## 95 percent confidence interval:
##  104.1 109.6 
## sample estimates:
## mean of x 
##     106.8 
## 
```




The output also shows the result of testing the null hypothesis that the true mean is actually zero. This is not a sensible test in this case but we can just ignore the ``test'' part of the output and look at the confidence interval that has been calculated for us.

The code below also finds the confidence interval. It involves fitting a simple statistical model. You will see why this works later in the course.



```r
confint(lm(Lshell ~ 1))
```

```
##             2.5 % 97.5 %
## (Intercept) 104.1  109.6
```





### Obtaining a confidence interval in SPSS}
 
*  You can find the confidence interval for a single variable in SPSS by running a one sample t-test
 
![alt text](primerfigs/spss9.png)
 
*  As in R the output shows the result of testing the null hypothesis that the true mean is actually zero. You will see more about hypothesis testing in the next section. 
 
![alt text](primerfigs/spss10.png)


### Exercises}

The following lines read in measurements on crow humerus lengths and makes them available for use in R after attaching the data frame. 



```r
d <- read.csv("http://tinyurl.com/QEcol2013/Crows.csv")
attach(d)
```




The variable to work with is Gl. These are measurements of the overall length of the bones in mm.



```r
Gl
```

```
##  [1] 54.56 58.24 63.12 63.44 63.60 63.84 63.86 65.50 65.74 66.04 66.18
## [12] 66.38 66.76 66.98 67.04 67.22 67.37 67.60 67.60 67.82 67.88 68.12
## [23] 68.34 68.56 68.56 68.58 68.70 69.40 69.82 70.16 71.10 71.84 71.90
## [34] 72.14 72.96 73.20 60.08 61.54 61.60 62.26 63.48 63.60 63.64 64.02
## [45] 64.04 64.20 64.20 64.28 64.34 64.68 64.96 65.08 65.16 65.40 65.54
## [56] 65.70 65.80 66.08 66.12 66.22 66.68 66.84 66.92 67.24 67.26 67.30
## [67] 67.32 67.34 68.04 68.06 68.32 68.44 68.64 68.74 68.84 69.02 69.16
## [78] 69.22 69.82 69.88 70.10 70.20 70.42 70.50 71.04
```




Produce simple boxplots and histograms in R. Calculate the mean, standard deviation, standard error and the 95\% confidence interval for the bone lengths. Import the data into SPSS and carry out the same operations.


 
