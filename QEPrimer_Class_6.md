## Class 6: Non parametric tests

**What you must remember before next term**

*  The difference between the assumptions of parametric tests and non-parametric tests
*  The use of ranks for non-parametric tests.
*  How to run simple non parametric tests 

**What you will look at in greater depth on the course**

*  Limitations of non parametric tests.
*  The appropriate use of normality tests.
*  Improving the evidence value of non-parametric procedures.

Statistical text books usually begin by explaining hypothesis tests that are based on inference from the normal distribution, such as the t-tests in the previous section. However in reality a great many student dissertations end up relying heavily on non-parametric tests. Non parametric tests can be a ``get out of jail free card'' when the data does not meet the assumptions needed for parametric methods. They are extremely useful if you just want to test for an overall difference between groups. However there are some limitations with non parametric procedures. They do not naturally lead to greater insight through statistical model building, therefore they are less useful with larger data sets that may contain more information.

Look at this example data set on tellin shell counts in mud sample. Either start a new R session or ``detach'' the previous data you were working with.



```r
d <- read.csv("http://tinyurl.com/QEcol2013/ShellCounts.csv")
str(d)
```

```
## 'data.frame':	40 obs. of  2 variables:
##  $ ShellCount: int  8 3 29 10 0 1 3 12 2 6 ...
##  $ Site      : int  1 1 1 1 1 1 1 1 1 1 ...
```

```r
attach(d)
Site1 <- ShellCount[Site == 1]
Site2 <- ShellCount[Site == 2]
```






### Non-normal histograms

We can visualise the distribution of the counts for each site using the lattice library in R. 



```r
library(lattice)
histogram(~ShellCount | Site, data = d, col = "grey", nint = 10, layout = c(1, 
    2))
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 


Notice that the piece of code that produced the figure is longer than one line. You will have to be careful when pasting it into the console. If you do not complete a command in R you will see a + instead of the > prompt. If this occurs you need to paste in the second part of the command correctly. The command will then run and you will return to the > prompt. If this does not happen press escape or the stop button and start again.

We can also look at boxplots.



```r
boxplot(ShellCount ~ Site, data = d, col = "grey")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 


In order to safely run a t-tests, or any other related parametric analyses the histograms and boxplots should look approximately symmetrical, which is an indication that we can assume normality. In this case there are clearly problems.


### Comparing the mean with the median

If the data are symmetrical the means should be close to the medians.



```r
mean(Site1)
```

```
## [1] 5.35
```

```r
median(Site1)
```

```
## [1] 3
```

```r
mean(Site2)
```

```
## [1] 15.05
```

```r
median(Site2)
```

```
## [1] 10.5
```




The means are higher than the medians. This suggests right skew. 


### Tests for normality

As we did previously we can test whether the values that were obtained from each site are likely to have been drawn from a normal distribution using a Shapiro Wilks test. Small p-values mean that we are unlikely to have obtained the data (or data more extreme) by drawing them from normal distributions.



```r
shapiro.test(Site1)
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  Site1 
## W = 0.7049, p-value = 4.475e-05
## 
```

```r
shapiro.test(Site2)
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  Site2 
## W = 0.759, p-value = 0.0002247
## 
```




So, according to the test, one of the basic assumptions for a t-test is clearly violated. 

Fortunately there are ways around this. The most common solution when only a simple test (rather than a statistical model) is required is to use a non parametric procedure based on ranks.

The reason that rank based methods are more robust is that they are less sensitive to the effects of extreme outliers. The largest number has the same rank in the sequence 1,2,3, 100 as the sequence 1,2,3,4. The outlier is neutralised.

The underlying logic of a hypothesis test is the same for a non-parametric test as a parametric test. We calculate a statistic with a known distribution under the null hypothesis. We then see find how probable a value such as we have obtained (or one more extreme) would be under the null hypothesis.


### Wilcoxon unpaired rank sum test (Mann Whitney)

In order to use the Wilcoxon unpaired rank sum test (known as the Mann Whitney test in SPSS) we place all the values together and find their overall ranks. We then sum the overall ranks for each group. We would expect the sum to be quite similar under the null hypothesis. However if one or other group has a very small rank sum this would suggest that it has a different median value than the other group *The interpretation of the test in terms of differences between medians is not formally correct, but it is commonly stated as such and is good enough for most purposes.*

We can get the sum of the ranks in R like this.



```r
Ranks <- rank(ShellCount)
tapply(Ranks, Site, sum)
```

```
##   1   2 
## 300 520 
```




The lowest rank sum is then compared against the known distribution. We need to know the size of each group. In this case n=20 in both cases.



```r
pwilcox(300, 20, 20, lower = F)
```

```
## [1] 0.002809
```




The quick way to obtain the same result is



```r
wilcox.test(ShellCount ~ Site)
```

```
## Warning: cannot compute exact p-value with ties
```

```
## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  ShellCount by Site 
## W = 90, p-value = 0.002961
## alternative hypothesis: true location shift is not equal to 0 
## 
```




Notice that R has made a very slight technical continuity correction, but this does not alter the conclusion.


### Running a Mann-Whitney test in SPSS


*  Choose the test for two independent samples from the Non parametric tests menu item.

![alt text](primerfigs/mwtest.png)

*  As for the t-test you use Site as a grouping value. You need to tell SPSS which levels of the grouping variable you want to compare.

![alt text](primerfigs/mwtest1.png)

*  The test result contains the same components as you have already seen with the addition of a value for Mann Whitney's U. This is an alternative statistic which is calculated from the ranks and provides exactly the same p-value. Notice that the top table provides the sum of the ranks and the mean of the ranks. This can help to determine which sample has the smaller median, although you should already have looked at this using boxplots and descriptive statistics earlier in the analysis.

![alt text](primerfigs/mwtest2.png)


### Exercises}

Completing this exercise will help you to understand the next section. The example has been simplified and the data is illustrative. A researcher is interested in how the abundance of *Hediste diversicolor* (ragworm) in an estuary may be related to substrate. Fifty cores were taken from areas that were classified as sand and sixty from areas classified as mud. You should think about the issues involved in applying this classification before moving to the next section. You should also think about other statistical issues that might complicate the analysis of data obtained from a survey rather than a planned experiment.

Download the data.



```r
d <- read.csv("http://tinyurl.com/QEcol2013/HedisteCounts.csv")
str(d)
```

```
## 'data.frame':	110 obs. of  2 variables:
##  $ Substrate: Factor w/ 2 levels "Mud","Sand": 1 1 1 1 1 1 1 1 1 1 ...
##  $ Count    : int  3 16 0 20 0 0 12 19 4 6 ...
```

```r
attach(d)
```




Are these data suitable for a t-test. If not, why not? What null hypothesis can be tested? How does this statistical hypothesis relate to the scientific question? Run the most appropriate test and interpret the results.
