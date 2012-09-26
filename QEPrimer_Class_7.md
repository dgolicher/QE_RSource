# Class 7: Chi-squared tests of association


**What you must remember before next term**

*  The type of data that are analysed using a chi-squared test of association.
*  The format of contingency tables
*  How to calculate proportions for rows and columns
*  How to calculate expected values
*  How to interpret a mosaic plot


**What you will look at in greater depth on the course**

*  General issues concerning categorisation
*  Analysis of data with low cell counts
*  Aggregation of categories
*  Other models for categorical data

One of the most commonly used statistical procedures is often known simply as ``chi-squared''. This may cause confusion, as Chi-squared is actually a statistic that is used in many different procedures. The chi-squared test is shorthand for Pearson's chi-squared test of independence. It is typically used in the analysis of contingency tables formed from two categorical variables.

Download the following data and compare it to the data used in the exercise in the previous section.

The data are in fact from exactly the same piece of fieldwork. However in this case the researcher simplified the data. Instead of counting all the ragworm in each core, only presence or absence was recorded. This might have been due to lack of time and resources, but it could have been a pragmatic decision based on previous experience or a pilot study. If most of the cores register complete absence, counting the precise number of worms where they occur provides relatively little additional information. Which other options might have been considered?



```r
options(digits = 2)
```




The count data looked like this. Notice that we can look at just the top of the data in R using head(d). In this case the head of the data frame contains only the observations taken on mud.



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
head(d)
```

```
##   Substrate Count
## 1       Mud     3
## 2       Mud    16
## 3       Mud     0
## 4       Mud    20
## 5       Mud     0
## 6       Mud     0
```





The simplified data can be loaded using these lines.



```r
d <- read.csv("http://tinyurl.com/QEcol2013/HedisteCat.csv")
attach(d)
```






```r
str(d)
```

```
## 'data.frame':	110 obs. of  2 variables:
##  $ Substrate: Factor w/ 2 levels "Mud","Sand": 1 1 1 1 1 1 1 1 1 1 ...
##  $ Cat      : Factor w/ 2 levels "Absent","Present": 2 2 1 2 1 1 2 2 2 2 ...
```

```r
head(d)
```

```
##   Substrate     Cat
## 1       Mud Present
## 2       Mud Present
## 3       Mud  Absent
## 4       Mud Present
## 5       Mud  Absent
## 6       Mud  Absent
```





### Why choose a chi-squared test?

The question of interest has not changed. We still want to know whether ragworm are found more frequently in muddy than sandy substrates. However now we don't have a numerical variable representing abundance. We only have categorical variables. Thus the answer to Dytham's question ``which test should I use?'' is in fact answered by looking at how the data have been recorded. Chi-squared tests are typically used when you have just two categorical variables. None of the procedures you have seen before could possibly be used with data in this form, even though you may still be addressing a similar scientific question. You may wish to consult the key provided by Dytham in chapter three, or the more concise key on pages 1:2 of Crawley to see how these authors suggest you decide on the statistical method. Crawley suggests log linear models for count data of this sort. The traditional chi-squared test of association is a commonly used alternative lo log linear modelling that is usually easier to understand for simple situations such as this.


### Tabulating the data

We cannot produce a lot of descriptive stats for a categorical variable. There are no means nor medians to find. Neither can we work with standard deviations and standard errors. Ranks do not make sense. All we can do is count how many observations are in each class. 

This can be carried out in R using the table function.



```r
SubstrateCount <- table(Substrate)
SubstrateCount
```

```
## Substrate
##  Mud Sand 
##   50   60 
```

```r
CatCount <- table(Cat)
CatCount
```

```
## Cat
##  Absent Present 
##      67      43 
```




We can see that there were 50 cores that were classified as mud and 60 as sand. Ragworm were absent from 67 cores and present in 43.


### Contingency tables

A more interesting way of looking at the data is to produce a contingency table. A contingency table contains counts of the number of observations that fall into the combination of categories shown at the sides (margins) of the table. In this case we have two categories for each variable. This makes a 2x2 contingency table which contains counts of the number of cores with ragworm absent that are classified as mud, the number of cores with ragworm absent that are classified as sand, the number of cores with ragworm present that are classified as mud and the number of cores with ragworm present that are classified as sand. Each of these combinations forms a cell in the table.



```r
observed <- table(Cat, Substrate)
observed
```

```
##          Substrate
## Cat       Mud Sand
##   Absent   23   44
##   Present  27   16
```




We can print out the table with the marginal totals.



```r
addmargins(table(Cat, Substrate))
```

```
##          Substrate
## Cat       Mud Sand Sum
##   Absent   23   44  67
##   Present  27   16  43
##   Sum      50   60 110
```






### Proportions

It is still a little difficult to spot a pattern. To find the proportion of presences and absences (dividing the counts by the for each substrate by the totals at the base of each column) we write.



```r
prop.table(table(Cat, Substrate), mar = 2)
```

```
##          Substrate
## Cat        Mud Sand
##   Absent  0.46 0.73
##   Present 0.54 0.27
```




So ragworms were present in 54\% of the mud samples but only 27\% of the sand samples.

What would we have expected if the proportions were the same for each?



```r
p.cat <- prop.table(table(Cat))
p.cat
```

```
## Cat
##  Absent Present 
##    0.61    0.39 
```




So we would expect 39\% of the cores to have ragworm regardless of the substrate. 


### Expected counts

We can express this as the expected counts under the null model of no association between substrate and ragworm presence.

We can calculate the expected values ``by hand'' by first extracting each of the numbers we need. Do not be concerned about the details of the R code at this time, but do try to follow the logic.

The overall proportion of observations with ragworm absent is....



```r
p.abs <- p.cat[1]
p.abs
```

```
## Absent 
##   0.61 
```




The overall proportion of observations with ragworm present is....



```r
p.pres <- p.cat[2]
p.pres
```

```
## Present 
##    0.39 
```




The count of the observations classified as mud is



```r
mud <- table(Substrate)[1]
mud
```

```
## Mud 
##  50 
```





And as sand



```r
sand <- table(Substrate)[2]
sand
```

```
## Sand 
##   60 
```




So we can obtain the expected values if the proportion of presences and absences are the same regardless of the substrate by multiplying these together.



```r
options(digits = 3)
```






```r
p.abs * mud
```

```
## Absent 
##   30.5 
```

```r
p.abs * sand
```

```
## Absent 
##   36.5 
```

```r
p.pres * mud
```

```
## Present 
##    19.5 
```

```r
p.pres * sand
```

```
## Present 
##    23.5 
```




The quick way to obtain the expected counts in R is to use matrix multiplication. Check that the result coincides with the expected counts calculated ``by hand''.



```r
expected <- prop.table(table(Cat)) %*% t(table(Substrate))
expected
```

```
##          Substrate
## Cat        Mud Sand
##   Absent  30.5 36.5
##   Present 19.5 23.5
```





### Calculating Chi squared

Now that we have got the observed counts and the expected counts we can calculate the value of Chi squared

$X^{2}=\sum\frac{(obs-exp)^{2}}{exp}$



```r
observed - expected
```

```
##          Substrate
## Cat         Mud  Sand
##   Absent  -7.45  7.45
##   Present  7.45 -7.45
```

```r
ChiSquare <- sum((observed - expected)^2/expected)
ChiSquare
```

```
## [1] 8.56
```




As usual, finding the significance value involves comparing the value to the distribution under the null hypothesis.



```r
pchisq(ChiSquare, df = 1, lower = F)
```

```
## [1] 0.00344
```





### Running the test in R

Again, there was no need to do all this ``by hand''. Now that you understand how it all works you can get R to run the test for you in one go. You will still usually want to look at the contingency table and the proportions before running the test. 

When running the test on a 2x2 table R will make a technical continuity correction by default. In order to compare the result we obtained through running the calculation step by step we can switch this off. 



```r
chisq.test(Cat, Substrate, correct = F)
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  Cat and Substrate 
## X-squared = 8.56, df = 1, p-value = 0.003441
## 
```




The corrected result is found by default.



```r
chisq.test(Cat, Substrate)
```

```
## 
## 	Pearson's Chi-squared test with Yates' continuity correction
## 
## data:  Cat and Substrate 
## X-squared = 7.45, df = 1, p-value = 0.00635
## 
```





### Mosaic plots

One way of visualising a contingency table is to use a mosaic plot. This is rather like a two way barchart. The area of each section in the plot correspond to the size of the cell count in the contingency table.




```r
mosaicplot(observed)
```

![plot of chunk unnamed-chunk-21](figure/unnamed-chunk-21.png) 


Comparing the mosaic plot for the observed data with the expected may help to clarify.



```r
mosaicplot(expected)
```

![plot of chunk unnamed-chunk-22](figure/unnamed-chunk-22.png) 




```r
options(digits = 6)
```





### Running a chi-squared test in SPSS


*  The easiest way to run a chi-squared test in SPSS is using the crosstabs option that is under the descriptives menu.

![alt text](primerfigs/chisq.png)

*  You arrange the variables in rows and columns and then click the ``Statistics'' and ``Cells'' buttons. Choose the Chi-square statistic from the statistics menu.

![alt text](primerfigs/chisq1.png)

*  You can ask for a lot of information to be placed in each cell using the Cells menu. It is usually better to restrict the amount of output for clarity.
\end{itemize}
![alt text](primerfigs/chisq2.png)

*  The output shows the same information that was obtained using the table and prop.table options in R. However all the output is placed together in one large table.
\end{itemize}
![alt text](primerfigs/chisq3.png)

*  If you asked for one, SPSS will provide a barchart that can help in the interpretation of the table. 

![alt text](primerfigs/chisq4.png)

