# Class 9-10: One way analysis of variance and Kruskall Wallace tests


## Introduction

This class will take two weeks to complete. It introduces some elements of the more complete, model based analyses, that we will look at next term. Notice how the analysis looks at more complex data than the previous, simpler, test based analyses. In this case we carry out multiple tests at once, but also draw inference from the pooled data. This is a characteristic of more advanced techniques. The more statistical methods you know the easier it can be to analyse large sets of data efficiently and quickly. 


**What you must remember before next term**

*  The underlying motivation for analysis of variance
*  The interpretation of the F-ratio
*  The assumptions underlying analysis of variance
*  Making and interpreting multiple comparisons
*  The dangers of ``data dredging''


**What you will look at in greater depth on the course**

*  Anova as a general linear model
*  The principle of parsimony
*  Analysing effect sizes
*  Determining model complexity
*  Running diagnostic tests of assumptions

We will look again at the Mussels data once more. 



```r
d <- read.csv("http://tinyurl.com/QEcol2013/Mussels.csv")
attach(d)
```





### Multiple comparisons

Now, it may have occurred to you when running t-tests that the process could become very repetitive if you wanted to compare multiple sites. There is also another issue that was hinted at. The number of combinations of sites (or any factor levels) for comparison increases quickly. You can use R to find out how many combinations there would be using binomial theory.



```r
levels(Site)
```

```
## [1] "Site_1" "Site_2" "Site_3" "Site_4" "Site_5" "Site_6"
```

```r
choose(6, 2)
```

```
## [1] 15
```




Running 15 separate t-tests would be quite a lot of work. So in this sort of case we often ask a more general question first. We test whether there is any significant differences between any of the sites.


### Visualising between group variation using boxplots

A good first step is to look at the variation in the data for each site using boxplots.



```r
boxplot(Lshell ~ Site, col = "grey")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 


The problem with comparing data using boxplots is that they show all the variability in the data, so there is often a large overlap between the boxes. 


### Plotting confidence intervals for each group

We have already seen that the standard error for the mean and the confidence intervals for the mean that are calculated from it are a function of sample size. So we may want to plot the confidence intervals for the mean in order to spot differences more clearly. To do this easily in R requires the package gplots to be installed and loaded.


```r
library(gplots)
```







```r
plotmeans(Lshell ~ Site, con = F)
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 



If there is clear space between confidence intervals a significance test will be significant. If confidence intervals overlap slightly the test may, or may not be significant depending on the extent of the overlap. If the overlap is large the tests will never be significant.

However before we start going into detail we should test a simple hypothesis. Could the variation in means between sites simply be due to chance? To do that we compare the variability in means to the overall variability in the data.


### Fitting a model

Crawley provides an excellent in depth explanation of the logic behind analysis of variance in chapter 9. I will not repeat all the details. In R to ``fit'' the analysis of variance as a model we can write aov(Lshell\textasciitilde{}Site) and then ask for a summary.



```r
mod <- aov(Lshell ~ Site)
summary(mod)
```

```
##              Df Sum Sq Mean Sq F value  Pr(>F)    
## Site          5   5525    1105    6.17 4.6e-05 ***
## Residuals   107  19153     179                    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1 
```




The result suggests that there is significant variation in mean shell length between sites, providing the assumptions of the test hold.


### The F ratio and degrees of freedom

The key to understanding analysis of variance is to understand the F ratio. The calculations produce a sum of squares and a mean square that are attributed to two sources of variation in the data. For this reason we often talk about ``partitioning the variance''. The Site component is the amount of variation attributable to variations in the mean shell lengths between sites. The residual variation is the amount of variability around these mean values. You can see that the mean square for the Site term in the model is much larger than the mean square for the Residuals term. In fact it is just over 6 times larger. We know that because the table includes the F value, which is the ratio of the two mean squares. The table also contains information regarding the number of groups and the amount of replication. These are the degrees of freedom. The degrees of freedom for ``Site'' is n-1. There are six sites so there are five degrees of freedom. The degrees of freedom for the residuals are the total number of measurements minus the number of sites (factor levels). So we have 113-6=107. If you are wondering how this works, the simple explanation is that we subtract the number of mean values that we use in the calculation of the sum of squares. In the case of the site, one overall mean is used (nsites-1). In the case of the residuals the mean of each site is subtracted from the observations for each site (nobs-6).

So we can now make a simple statement that we back up with the statistical analysis. There is statistically significant variability in mean shell lengths between sites F(5, 107) = 6.17, p <0.001.


### Homogeneity of variance

You might have spotted an issue with the test, particularly if you have been reading the text books thoroughly. The traditional analysis of variance assumes homogeneity of variances. The boxplots suggest that there is a lot of variation in between sites in the amount of variation in shell length. A test for homogeneity of variance that is often recommended is Bartlett's test



```r
bartlett.test(Lshell ~ Site)
```

```
## 
## 	Bartlett test of homogeneity of variances
## 
## data:  Lshell by Site 
## Bartlett's K-squared = 18.53, df = 5, p-value = 0.002352
## 
```




As always, a low p-value suggests that the null hypothesis can be rejected, which in this case is homogeneity of variances. So on technical grounds the test we have just conducted is not quite right. One of the assumptions is not met. This can be a major issue for more complex designs. It is easy to let R make a correction for this when running a one way anova by asking for a oneway test (Welch's test).



```r
oneway.test(Lshell ~ Site)
```

```
## 
## 	One-way analysis of means (not assuming equal variances)
## 
## data:  Lshell and Site 
## F = 9.656, num df = 5.00, denom df = 31.19, p-value = 1.207e-05
## 
```




The test has now taken into account the issue and it still gives a significant result. Using Welch's procedure is a useful backup to reinforce and defend your conclusions when the assumption of homogeneity of variance is violated. We will look at this and similar issues in more detail in the course. However you still need to look at the pattern of differences more carefully.


### Determining where the differences lie

The problem with the simple conclusion drawn from analysis of variance with or without Welch's correction is that from a scientific perspective is that it is hardly an unsurprising discovery. We would have expected some difference, particularly if we look at so many different sites. It is much more likely that we are really interested in specific differences between sites. However this raises an additional issue. If we have a hypothesis before we start regarding which site is most likely to be different we could run a single test. However if we are looking at fifteen separate comparisons there is a problem. The conventional significance value is set at 0.05. In other words one chance in twenty of obtaining the data (or more extreme) under the null hypothesis. If we run a lot of tests we increase the chances of at least one being significant even if the null holds. 

In fact it is quite easy to calculate the probability of getting at least one significant result if we run 15 tests. The easy way is to calculate the probability of all the tests being negative and subtract from 1.



```r
1 - 0.95^15
```

```
## [1] 0.5367
```




So there is a 54\% chance of getting at least one significant result at the 0.05 cut off level even if the null is true for every comparison. This is a bit like buying a large number of lottery tickets. If you buy enough you increase your overall chances of winning even though the chances of any single ticket winning remains the same%
*What is the probability of getting at least one significant result at the 0.05 level if you run twenty tests? Without thinking it is tempting to say that it is one. However this is not the correct answer*

One way of reducing the number of tests is to compare all the means with a control group. This is done in an experiment with treatments, and is the contrasts are thus known as treatment contrasts. We can obtain these in R from the output of aov using summary.lm.



```r
summary.lm(mod)
```

```
## 
## Call:
## aov(formula = Lshell ~ Site)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -44.91  -8.34   1.03   9.23  30.55 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   100.77       2.62   38.40   <2e-16 ***
## SiteSite_2      8.47       3.75    2.26    0.026 *  
## SiteSite_3      6.04       5.41    1.12    0.267    
## SiteSite_4     -3.62       5.41   -0.67    0.505    
## SiteSite_5     18.70       3.93    4.76    6e-06 ***
## SiteSite_6      2.47       3.75    0.66    0.511    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1 
## 
## Residual standard error: 13.4 on 107 degrees of freedom
## Multiple R-squared: 0.224,	Adjusted R-squared: 0.188 
## F-statistic: 6.17 on 5 and 107 DF,  p-value: 4.58e-05 
## 
```




This shows that only site2 and site5 are significantly different from site 1. If you go back to the confidence interval plot and look at the overlap you can see how this pattern emerges.


### Bonferoni corrections

Treatment contrasts method assumes that there is a planned contrast and there is something unique about site1. Another way of looking at the issue is to effectively make comparisons between all the pairs of sites. In this case we MUST compensate in some way for all the test. There are a lot of ways of doing this. The simplest is called the Bonferoni correction. This just involves changing the critical value by dividing by the number of tests. So if we call a p-value of 0.05 significant, but run 15 tests we would look for values of 0.05/15= 0.0033 before claiming a significant result.


### Tukey's honest significant difference

A slightly more subtle method is called Tukey's Honest Significant Difference (HSD) test. We can run this in R for all pairwise comparisons and plot the results. The confidence intervals and p-values are all adjusted.

This produces a lot of output, as you would expect. It effectively runs the fifteen separate t-tests while making allowances for multiple comparisons. The output also includes 95\% confidence intervals for the differences between the means. If these confidence intervals include zero then the test will not be significant. The easiest way to see the pattern is to plot the results. 



```r
TukeyHSD(mod)
```

```
##   Tukey multiple comparisons of means
##     95% family-wise confidence level
## 
## Fit: aov(formula = Lshell ~ Site)
## 
## $Site
##                  diff     lwr    upr  p adj
## Site_2-Site_1   8.467  -2.409 19.342 0.2201
## Site_3-Site_1   6.037  -9.661 21.735 0.8738
## Site_4-Site_1  -3.619 -19.317 12.078 0.9849
## Site_5-Site_1  18.697   7.306 30.089 0.0001
## Site_6-Site_1   2.471  -8.405 13.346 0.9859
## Site_3-Site_2  -2.430 -18.201 13.342 0.9977
## Site_4-Site_2 -12.086 -27.857  3.685 0.2356
## Site_5-Site_2  10.231  -1.262 21.723 0.1104
## Site_6-Site_2  -5.996 -16.978  4.986 0.6105
## Site_4-Site_3  -9.656 -29.069  9.757 0.7005
## Site_5-Site_3  12.660  -3.471 28.792 0.2124
## Site_6-Site_3  -3.566 -19.338 12.205 0.9862
## Site_5-Site_4  22.317   6.185 38.448 0.0015
## Site_6-Site_4   6.090  -9.681 21.861 0.8718
## Site_6-Site_5 -16.227 -27.719 -4.734 0.0011
## 
```

```r
plot(TukeyHSD(mod))
```

![plot of chunk unnamed-chunk-11](figure/unnamed-chunk-11.png) 


Notice that the comparison that we made between site1 and site2 that was significant using either a single t-test or treatment contrasts now is not shown as significant after the correction has been made for making numerous unplanned comparisons. If you really wanted to claim that the difference was significant you would have to be able to justify that you had thought that site2 ought to be particularly different before obtaining the data, and that the result was not just obtained by so called ``data dredging''.

So, this section of the primer has begun to reveal some of the subtle aspects of data analysis. Under the more rigorous procedure only site 5 really stands out as being different from the rest. The fact that different ways of looking at the data can apparently lead to different conclusions is one of the aspects of statistics that worries many students (and researchers). Next term we will look at ways of avoiding common statistical pitfalls, together with methods to extract all the important information from a data set in order to fully address your scientific question.


### The Kruskal Wallace non parametric test.

Just as the Wilcoxon/Mann Whitney test can look at differences between groups without making any assumptions regarding the distribution of the residuals, the Kruskal Wallace test can be used in place of a one way anova if you are really worried about the assumption of normality. The main disadvantage of a Kruskall Wallace test is that it is not easy to communicate the absolute size of any differences that are detected. The test does not lead to the development of a statistical model.



```r
kruskal.test(Lshell ~ Site)
```

```
## 
## 	Kruskal-Wallis rank sum test
## 
## data:  Lshell by Site 
## Kruskal-Wallis chi-squared = 27.88, df = 5, p-value = 3.835e-05
## 
```




In R multiple comparisons can be run after installing the ``pgirmess'' package.


```r
library(pgirmess)
```







```r

kruskalmc(Lshell ~ Site)
```

```
## Multiple comparison test after Kruskal-Wallis 
## p.value: 0.05 
## Comparisons
##               obs.dif critical.dif difference
## Site_1-Site_2  20.584        26.94      FALSE
## Site_1-Site_3  20.154        38.88      FALSE
## Site_1-Site_4   2.596        38.88      FALSE
## Site_1-Site_5  44.023        28.22       TRUE
## Site_1-Site_6   4.184        26.94      FALSE
## Site_2-Site_3   0.430        39.06      FALSE
## Site_2-Site_4  23.180        39.06      FALSE
## Site_2-Site_5  23.439        28.47      FALSE
## Site_2-Site_6  16.400        27.20      FALSE
## Site_3-Site_4  22.750        48.08      FALSE
## Site_3-Site_5  23.869        39.96      FALSE
## Site_3-Site_6  15.970        39.06      FALSE
## Site_4-Site_5  46.619        39.96       TRUE
## Site_4-Site_6   6.780        39.06      FALSE
## Site_5-Site_6  39.839        28.47       TRUE
```




You should see that the results coincide with those from Tukey's tests. Site five is again shown as clearly different.

There is no equivalent implemented in SPSS, so in order to run multiple comparisons you have to use multiple Mann Whitney tests with a Bonferoni correction.

You may also want to use the ``notch=T'' option when plotting boxplots to obtain an approximate confidence interval for the median. The R help states ``If ‘notch’ is ‘TRUE’, a notch is drawn in each side of the boxes. If the notches of two plots do not overlap this is ‘strong evidence’ that the two medians differ (Chambers \_et al.\_, 1983, p. 62).''



```r
boxplot(Lshell ~ Site, notch = T, col = "grey")
```

```
## Warning: some notches went outside hinges ('box'): maybe set notch=FALSE
```

![plot of chunk unnamed-chunk-15](figure/unnamed-chunk-15.png) 


You should compare this with the results obtained above. They coincide.

There is no equivalent in SPSS.


### One way analysis of variance in SPSS

Analysis of variance is one of the most commonly used statistical techniques in all branches of science and SPSS is very good at running ANOVAs in a comprehensive manner. Typical extensions of the technique involve two way analysis of factorial designs. SPSS includes many options when running ANOVAs in order to ensure rigor. All these options can also be implemented in R. The advantage of using SPSS for classical ANOVA is that the software reminds you of the options that are available. The disadvantage could be ``information overload''. Choosing options may appear daunting. In fact many of the differences are subtle and need only concern you when you need to present the results from a planned experiment. In ecological research much of our data is obtained from field surveys. There are many statistical challenges with such data that we will look at in detail next term. F

*  Start your analysis using the chart builder. You should always look at your data before carrying out any statistical tests. You can obtain box and whisker plots for each of the sites by dragging the variables into place.

![alt text](primerfigs/box.png)

*  This is the result in the output window. You may want to click on the figure and edit it further if you are going to include it in a report.

![alt text](primerfigs/box1.png)

*  Plotting confidence intervals allows you to draw statistical inference from a figure. This is a very useful step. In SPSS, slightly counter-intuitively, you find the confidence interval plots under the ``bar'' menu. Do not try to use the ``high-low'' chart.

![alt text](primerfigs/box2.png)

*  This is the result.

![alt text](primerfigs/box3.png)

*  You may want to change the scale and add some faint grid lines in order to help compare the position of the confidence intervals.

![alt text](primerfigs/box4.png)

*  Now find the one way analysis of variance option. This is under the ``compare means'' menu.

![alt text](primerfigs/anova.png)

*  Lshell is the dependant variable. Site is a factor. If you click on the options button you can select Welch's procedure in order to run the one-way test that does not assume homogeneity of variance. You can also run a test for homogeneity of variance.

![alt text](primerfigs/anova2.png)

*  There are many rather options for Post Hoc tests. There are only subtle differences between most of them. Use the ones that you now understand and ignore the other options for the moment. Dunnet's test does not assume equality of variance and is a variation on Welch's test for multiple comparisons. You may want to try this option along with the Bonferoni correction, but for simplicity here I will just show the output from Tukey's tests.

![alt text](primerfigs/anova1.png)

*  SPSS provides you with the descriptives for each site along with confidence intervals.

![alt text](primerfigs/anova3.png)

*  The test for homogeneity of variances suggests a significant difference in variance between sites.

![alt text](primerfigs/anova4.png)

*  Here is the standard Anova table.

![alt text](primerfigs/anova5.png)

*  The analysis that corrects for the problem with the variances is provided if you asked for it. Notice that the denominator degrees of freedom has been reduced.

![alt text](primerfigs/anova6.png)

*  The results of Tukey's test that SPSS provides are the same as the R output, but there is more duplication, as each block holds all the comparisons for each site.

![alt text](primerfigs/anova7.png)


### Kruskall Wallace in SPSS

*  The Kruskal Wallace test can be found under the non-parametric tests menu. Select ``K independent samples''.

![alt text](primerfigs/ks.png)

*  Add the variables in the usual way. You need to give SPSS the range of groups that will be compared.

![alt text](primerfigs/ks1.png)

*  The output is equivalent to that produced by R.

![alt text](primerfigs/ks2.png)

*  Multiple comparisons can easily be done one by one using Mann Whitney, but this is rather tedious and not shown. If you do this do not forget to use the Bonferoni correction.


