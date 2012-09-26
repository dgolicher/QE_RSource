# Class 8: Correlation

**What you must remember before next term**

*  How to visualise correlations using scatterplots
*  The underlying components of the correlation coefficient
*  The meaning of covariance
*  The difference between the significance of a correlation and the strength of a correlation 

**What you will look at in greater depth on the course**

*  The relationship between correlation analysis and regression
*  Links between correlation and causation
*  Methods that look at multiple correlations

So far we have looked at differences between groups. However we are often likely to be interested in the way two numeric variables change together. There are many types of analyses that address this. In ecology we are often most interested in the functional form of the relationship between two variables. For example we may want to look in detail at how the feeding rate of waders depends on the density of invertebrates. 

The most basic question we can ask about a relationship between two variables is simply ``does a relationship exist?'' This is hypothesis testing. Simply establishing that there is a relationship is often not enough, but it is at least a starting point.


### Scatterplots

We will work with the Mussels data again.



```r
d <- read.csv("http://tinyurl.com/QEcol2013/Mussels.csv")
attach(d)
```




The easiest way to spot a relationshipbetween two numerical variables is to plot the data as a scatterplot. The mussel data set contains measurements on shell length and body tissue volume in ml. We would expect a clear relationship on \emph{a priori} grounds between the two, but we can look at it easily by plotting.



```r
plot(BTVolume ~ Lshell)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 


One way of thinking about the relationship is by cutting the plot into four quarters, splitting each axis by a line representing the mean value along it. If there is a strong positive correlation between the variables you would expect more points to fall in the top right quarter and the bottom left.



```r
plot(Lshell, BTVolume)
points(mean(Lshell), mean(BTVolume), cex = 3, pch = 21, bg = 2)
abline(h = mean(BTVolume))
abline(v = mean(Lshell))
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 


If the two variables are not correlated the scatterplot will look more like this.




```r
x <- rnorm(100)
y <- rnorm(100)
plot(x, y)
points(mean(x), mean(y), cex = 3, pch = 21, bg = 2)
abline(h = mean(y))
abline(v = mean(x))
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 


The strength of the relationship can be measured by calculating the correlation coefficient. 


### Covariance

The key to understanding how to calculate the correlation coefficient is understanding how we get a covariance. The covariance is the sum of the product of the deviations of x and y from their respective means divided by n-1.


$cov(x,y)=\frac{1}{n-1}\sum(x-{\bar{x})(y-\bar{y})}$

Taking this step by step helps to understand the equation.

We can calculate the deviations of the x values from their mean and the y values from their mean. Then find the product.



```r
n <- length(Lshell)
xdev <- Lshell - mean(Lshell)
ydev <- BTVolume - mean(BTVolume)
prod <- xdev * ydev
```







```r
xdev <- round(xdev, 2)
ydev <- round(ydev, 2)
prod <- round(prod, 2)
head(data.frame(xdev, ydev, prod))
```

```
##     xdev  ydev   prod
## 1  15.27 11.19 170.75
## 2  -6.73 -6.81  45.89
## 3  -6.13 -4.81  29.53
## 4  -4.53 -5.81  26.37
## 5 -11.93 -7.81  93.26
## 6  10.07 -5.81 -58.52
```





If there is a positive correlation negative values will line up with other negative values and positive with positive leading to positive product values. When these are added together the covariance will be large.

If there is no correlation negative values will be just as likely to line up with negative values as positive. So the overall product will be small. 

If the correlation is negative the products will tend to be negative as the signs will cross.

To get the covariance we now just need to add up all the products and divide by n-1



```r
sum(prod)/(n - 1)
```

```
## [1] 131.7
```




We can check if this is right using the built in R function.



```r
cov(Lshell, BTVolume)
```

```
## [1] 131.7
```





### The correlation coefficient

The problem with the covariance is that its value will vary depending on the data values. So it is not very useful as a comparative measure. We need to standardise it. This is the correlation coefficient which is defined as

$r=\frac{cov(x,y)}{\sqrt{s_{x}^{2}}s_{y}^{2}}$

The denominator is simply the square root of the product of the two variances. 

We can get there in R with.



```r
cov(Lshell, BTVolume)/sqrt(var(Lshell) * var(BTVolume))
```

```
## [1] 0.8777
```





The quick way, of course, is simply to use the built in function.



```r
cor(Lshell, BTVolume)
```

```
## [1] 0.8777
```





### The coefficient of determination

The correlation coefficient r always takes values between -1 and 1. Large negative values suggest a strong negative correlation and large positive values a strong positive correlation. To assess simply the overall strength of the correlation we take the square of the correlation coefficient. The coefficient of determination is also provided by many other analyses.



```r
R2 <- cor(Lshell, BTVolume)^2
R2
```

```
## [1] 0.7704
```




R² is known as the coefficient of determination. R² can in some circumstance be interpreted as the amount of variance "explained"" by the relationship. It takes values from 0 to 1. We will come back to this later.

It is difficult to provide rules for deciding what is a large and small value for R². This depends very much on the natural variability of the system under study. In general terms values of R² greater than 0.8 show strong correlation and values below 0.3 suggest weak correlation.


### Significance of the correlation coefficient.

If you have followed the account of hypothesis testing presented earlier you may have realised that obtaining a correlation coefficient is not enough. If the sample size is small then even moderate sized correlation coefficients may turn out to be due to chance. We can test how likely it would be to obtain a correlation coefficient as large (or larger) than the one we have under the null hypothesis by calculating a statistic and comparing its value to the known distribution for the statistic under the null hypothesis. One way of doing this is by calculating another version of the t statistic.


$t=\frac{r}{\sqrt{(1-r^2)/(N-2)}}$

Notice that this formula includes the correlation coefficient r and the coefficient of determination (r²) but also takes into account the sample size (N-2). The significance of the value is automatically looked up by R when you run the following command.



```r
cor.test(Lshell, BTVolume)
```

```
## 
## 	Pearson's product-moment correlation
## 
## data:  Lshell and BTVolume 
## t = 19.3, df = 111, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0 
## 95 percent confidence interval:
##  0.8271 0.9142 
## sample estimates:
##    cor 
## 0.8777 
## 
```





### Scatterplots with trend lines

Linear regression is a model based statistical technique that is extremely useful. We will look at regression and other linear models in much more detail next term. Linear regression can be used to find the line of best fit that shows up the relationship between the variables.



```r
plot(BTVolume ~ Lshell)
abline(lm(BTVolume ~ Lshell))
```

![plot of chunk unnamed-chunk-13](figure/unnamed-chunk-13.png) 


You might notice that the pattern of scatter around the line varies slightly over the range. This is an issue for regression that we will return to.


### Rank correlation

You should have noticed that the calculations needed to find the correlation coefficient and test its significance rely on sums of squares. They are thus based on the assumption that the variables being correlated are normally distributed. They also are assuming a straight line relationship between the variables. Both these assumptions are easily violated. A much more robust approach is to use rank correlation. This ``works'' regardless of the distribution of the data and only assumes a monotonic relationship between the variables i.e. that an increase in the value of one variable implies an increase in the other. We will look at this in more detail next term. To obtain the rank correlation use



```r
cor.test(Lshell, BTVolume, method = "spearman")
```

```
## Warning: Cannot compute exact p-values with ties
```

```
## 
## 	Spearman's rank correlation rho
## 
## data:  Lshell and BTVolume 
## S = 26143, p-value < 2.2e-16
## alternative hypothesis: true rho is not equal to 0 
## sample estimates:
##    rho 
## 0.8913 
## 
```




In general terms when the assumptions of Pearson's correlation are met the two analyses will produce very similar results, as they do here.


### Correlation using SPSS

*  You can produce a scatterplot using the chart builder. Note that SPSS has a lot of options for grouping data. These are useful with more complex data sets. Drag the variables into the right part of the graph window.

![alt text](primerfigs/scat.png)

*  Once you have your figure in the output window click on it to open the editor.

![alt text](primerfigs/scat1.png)

*  You can then add a regression line. Notice that there are also options for adding confidence intervals to the regression. It is a very good idea to use these options, particularly if the relationship is not very strong. We will look at this in the course. In this case the relationship is very clear and is best shown by a single line.

![alt text](primerfigs/scat2.png)

*  Here is the result in the output window. Notice that SPSS helpfully adds the R\texttwosuperior{} value to the figure.

![alt text](primerfigs/scat3.png)

*  To run a correlation you select correlate. In this case you are looking at bivariate correlations.

![alt text](primerfigs/cor.png)

*  Add the variables to the window. In more complex situations you often want to look at correlations between multiple variables. These form a correlation matrix. For example if the variables are called A,B and C you can obtain the correlation between A\&B, A\&C and B\&C in one go. The matrix is symmetrical around the diagonal formed by the correlations between A\&A, B\&B and C\&C, which of course all have the value of 1.

![alt text](primerfigs/cor1.png)

*  Notice in the output that SPSS first provides you with some useful information about each variable (this may save you the trouble of obtaining these stats using the descriptives menu) and then gives the correlations for each variable in the form of a matrix. There is duplication and you only need to look at the top left hand corner or the bottom left hand corner to obtain all the information.

![alt text](primerfigs/cor2.png)

*  The output also shows the non-parametric correlations if you asked for them. Kendall's Tau has very similar properties to Spearman's rank correlation but may be preferred on technical grounds in some circumstances. The interpretation is the same.

![alt text](primerfigs/cor3.png)

