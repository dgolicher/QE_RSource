# Class 3: Descriptive statistics


## Introduction}

In this class you will start using R and SPSS for simple statistical analysis for the first time. Hopefully you will find that the material is mainly a refresher with regard to the key concepts. However it is worth working through the analyses, and in particular the exercises, in order to develop some practical skills with R and SPSS. The online tests on MyBU are based on these exercises and the data sets provided for them. So working through the exercises will enable you to complete the tests quickly.


What you must know before next term
 
*  How to find a median
*  How to calculate a mean
*  How to find quantiles
*  How to calculate a standard deviation
 
What you will look at in greater depth next term
 
 
*  How to interpret the distribution of data with respect to the normal distribution
*  Why and when to transform data
*  How to interpret descriptive statistics in the light of ecological processes
*  How to produce effective figures to communicate data properties
 

### Set reading

Crawley, M (2005) Statistics: An introduction using R: Chapters 3 to 5

Dytham (2011) Choosing and using statistics: A biologists guide Chapter 5


### Analysing real data

Quantitative ecology involves taking measurements on ecological variables and using the resulting data to answer interesting scientific questions. In order to achieve this goal you will normally use some form of inferential statistics. This could involve simple hypothesis testing or more complex model building. However you always need to describe the data you have in order to understand their properties and communicate these properties to others. Descriptive statistics involves summarising data and showing the properties of the data with appropriate figures.

Because we want to adopt a sequential approach here we will use R commands in order to look at the analysis step by step.

Let's get some data into R and start analysing it. The data is from a survey of the benthic fauna of deep sea ridges. http://www.ridge2000.org/science/data/index.php. 

Open the R console.

Typing or cutting and pasting the following line into the console and pressing enter will read in the data.

 


```r
d <- read.csv("http://tinyurl.com/QEcol2013/Mussels.csv")
```




Nothing will appear to happen. However the data will be loaded into memory, providing that you are connected to the internet of course. You will not see the data, but R should return you to the > prompt. The data in fact been loaded into an R data frame called simply ``d''. As we saw in the last class a data frame is rather like a single sheet in a spreadsheet. Alternatively you can save the data file (or open it in directly a spreadsheet in order to confirm the structure) by clicking on this link 
http://tinyurl.com/QEcol2013/Mussels.csv
 
If you have stored a data file on disk, you can read it into R from there (rather than pulling it from the web) by setting the working directory to the folder where you saved the file using the ``Change dir'' option under the file menu at the top of the console.

 
![alt text](primerfigs/change.png)
 

and then write.



```r
d <- read.csv("Mussels.csv")
```




In order to get a summary of the data that you have loaded you can write.



```r
str(d)
```

```
## 'data.frame':	113 obs. of  3 variables:
##  $ Lshell  : num  122.1 100.1 100.7 102.3 94.9 ...
##  $ BTVolume: int  39 21 23 22 20 22 21 18 21 15 ...
##  $ Site    : Factor w/ 6 levels "Site_1","Site_2",..: 6 6 6 6 6 6 6 6 6 6 ...
```




If you have typed the commands correctly, this is what the output will look like on your console.

 
![alt text](primerfigs/Rcon.png)
 

Typing fix(d) will open the spreadsheet like data editor.



```r
fix(d)
```




 
![alt text](primerfigs/Rcon2.png)
 

You can then edit the data values if you wish to. The new values will be saved when you close the window. Be a bit careful with this. You cannot type any commands until you have finished editing the data, and if you do change the data you will get different results to those shown in the text. If things appear to have ``frozen'' the problem is simply that you have not yet closed the editing window (it may have gone to the back). Make sure that you have clicked in the console before typing in order to make it the focal window. You can always reload the data by running the first line and starting again without opening the editor.

The variable ``Lshell'' represents the length in mm of mussel shells taken from deep water vents. These particular individuals are of the genus Bathymodiolus (family Mytilidae; subfamily Bathymodiolinae). The analysis could equally well be applied to   shells from a local rocky shore. 

Note that we refer to the numbers as making up a ``variable''. This is because, unsurprisingly, they vary. It is the variability in lengths that statistical methods deal with. If all adult mussels were exactly the same length there would be no need to apply descriptive statistics. 

In order to work with individual variables you attach the data frame.



```r
attach(d)
```





### Visualising data

The first issue with variable data involves spotting patterns and summarising the data. One way to start may be to place the values in order.



```r
sort(Lshell)
```

```
##   [1]  61.9  69.3  73.8  76.6  77.5  79.3  81.7  83.9  84.1  85.6  88.1
##  [12]  89.2  89.6  89.8  90.8  91.1  91.5  92.1  93.5  93.9  94.3  94.3
##  [23]  94.7  94.9  94.9  95.8  96.4  96.8  97.0  97.0  97.4  99.5  99.5
##  [34]  99.5  99.6  99.8 100.1 100.2 100.7 101.1 101.1 101.8 102.2 102.3
##  [45] 102.3 102.4 102.4 102.6 102.8 103.3 104.1 104.2 104.9 105.5 106.0
##  [56] 106.4 106.9 107.2 108.2 108.6 109.2 110.0 110.1 110.7 111.5 111.9
##  [67] 112.6 113.4 113.5 113.8 113.8 113.9 114.2 115.2 115.5 115.8 116.3
##  [78] 116.7 116.8 116.9 117.4 118.3 118.4 118.6 118.7 118.9 118.9 119.5
##  [89] 120.4 120.5 120.7 120.9 121.1 121.1 121.5 122.1 122.8 123.2 124.2
## [100] 124.2 124.3 124.7 125.6 125.6 125.9 127.7 128.0 128.4 129.9 130.6
## [111] 131.7 132.2 132.6
```




However the pattern in the data is still difficult to spot. The data values can be written as a stem and leaf plot.



```r
stem(Lshell)
```

```
## 
##   The decimal point is 1 digit(s) to the right of the |
## 
##    6 | 2
##    6 | 9
##    7 | 4
##    7 | 789
##    8 | 244
##    8 | 689
##    9 | 0011224444
##    9 | 555667777
##   10 | 000000011122222233344
##   10 | 566677899
##   11 | 001223344444
##   11 | 56667777889999
##   12 | 00111112233444
##   12 | 5666888
##   13 | 01223
## 
```




This can be a useful way of providing the raw data values in an appendix.

A common way of looking at the distribution of the data is as a histogram.
 


```r
hist(Lshell)
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8.png) 



Or a boxplot.



```r
boxplot(Lshell)
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 


The default commands produce the very minimalist Figures 1 and 2. These would need additional options in order to be used in a publication, but the simple default commands are fine for looking at the data quickly in order to understand them. We will look at the use of boxplots and histograms as diagnostic tools and ways to improve the appearance of output from R in more detail later in the course. 


## Measures of central tendency


#### The median

Measures of central tendency are useful summaries of a variable that should already be familiar. They are often referred to as ``average'' values. There are two main measures we can use. The mean and the median. 

The median is the value in the middle of the sorted data. Once the data are placed in order, a reasonable way to find the centre would be to locate the middle ranked value.

In the case of an odd number of observations finding the middle is obvious. Say, the numbers are 3,5,**6**,7,9,: The median is 6 

There is a slight problem in the case of an even number of observations. To see this think about four numbers, say 3,**5,7**,9 .The middle number isn't either the second (5), nor the third (7). We take the mean of the two.
 
*  If n is odd:The median is the number with the rank (n+1)/2
*  If n is even we take the mean of the numbers with ranks n/2 and n/2 +1
 


```r
median(Lshell)
```

```
## [1] 106.9
```





#### The mean}

Finding the value of the mean in R is easy. Just type mean.



```r
mean(Lshell)
```

```
## [1] 106.8
```




The equation for calculating a mean is written. 

**$\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_{i}$** 

In words, this says ``Add up all the numbers and divide by the number of entries''

To find n we ask R for the length of the vector.



```r
n <- length(Lshell)
n
```

```
## [1] 113
```





Now we can get the total using sum.



```r
Total <- sum(Lshell)
Total
```

```
## [1] 12072
```




So the mean is the total over n.



```r
Mean <- Total/n
Mean
```

```
## [1] 106.8
```





If you have followed this calculation you should be able to use the same technique to break down some more complex equations.


### Measures of variability


#### Range

The range is simply the distance between the maximum and minimum values.



```r
min(Lshell)
```

```
## [1] 61.9
```

```r
max(Lshell)
```

```
## [1] 132.6
```

```r
max(Lshell) - min(Lshell)
```

```
## [1] 70.65
```





#### Quantiles

The median is a specific example of a quantile. Quantiles are points taken at regular intervals from the cumulative distribution function (CDF) of a random variable. In other words they divide ordered data into q equal-sized data subsets.

The 2-quantile is called the median

If the data are split into 4-quantiles they are called qua tiles (Notice that quartiles is spelt with an **R** rather than an **N**)

If the data are split into 10-quantiles they are called deciles 

Another general term is percentiles.

These are calculated in the same way depending on where the breaks fall. To get the qua tiles in R write



```r
summary(Lshell)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    61.9    97.0   107.0   107.0   119.0   133.0 
```




At this point you should look back to the boxplot produced earlier. How do these numbers relate to the elements shown? The boxplot uses quartiles. How?

*The line in the centre of a boxplot shows the median. The box extends to the quartiles, so that it includes 50\% of the data. The whiskers extend to the farthest data point that is not considered to be an outlier. Points beyond the whiskers may be outliers (we will see more about this later).*

More generally you can specify the specific quantiles you want to find.



```r
quantile(Lshell, c(2.5, 5, 95, 97.5)/100)
```

```
##   2.5%     5%    95%  97.5% 
##  76.04  80.74 128.16 130.82 
```




Try finding some quantiles yourself by looking at the sorted data.


#### The standard deviation

The standard deviation is very commonly used to summarise variability. In words, we calculate the standard deviation by subtracting the mean from each value, squaring the result and then adding up these values in order to find the sum of squares. We then divide by n-1 and take the square root.

There is a slight complication regarding the standard deviation. When we collect our data we usually only obtain a sample from a larger population of observations we could have made. The procedures that fall under the heading of statistical inference attempt to estimate the properties of this larger (or maybe infinite) population from the sample we have. There is therefore a difference between the population standard deviation, that we rarely can ever know, and the sample standard deviation that we can calculate directly. The population standard deviation is given the symbol $\sigma$ and the sample deviation the symbol s.

The formula for the sample standard deviation s that is an``unbiased estimator'' of $\sigma$ is

**$\sigma=sqrt{\frac{1}{n-1}}\sum_{i=1}^{n}(\tilde{x}-x_{i})$$^{2}$**

The steps in calculating s are as follows. First the sum of the squared deviations from the mean. Notice that if we did not square the deviations they would sum to zero.

Notice that in R the <- operator assigns the result of a calculation to a variable. This can be useful if you wish to perform more calculations on the result. Writing the name of the variable prints out its value.



```r
sumsquare <- sum((Lshell - Mean)^2)
sumsquare
```

```
## [1] 24678
```




Now find the mean square.



```r
meansquare <- sumsquare/(n - 1)
meansquare
```

```
## [1] 220.3
```




And finally the root mean square.



```r
rootmeansquare <- sqrt(meansquare)
rootmeansquare
```

```
## [1] 14.84
```




The mean square is also known as the variance and the root mean square is the standard deviation. We can check that they are the same.



```r
meansquare
```

```
## [1] 220.3
```

```r
var(Lshell)
```

```
## [1] 220.3
```






```r
rootmeansquare
```

```
## [1] 14.84
```

```r
sd(Lshell)
```

```
## [1] 14.84
```




Chapter four of Crawley looks at all this in more depth. 


#### The normal distribution

One of the main motivations for calculating the standard deviation is that if the observations form a symmetrical normal distribution we can predict where the bulk of the observations will lie if we know the mean and the standard deviation.

A normal distribution is shown in Figure 3.

![plot of chunk unnamed-chunk-23](figure/unnamed-chunk-23.png) 



If we shade in the area that extends out from the mean to a distance of one standard deviation we will have shaded in 68\% of the area under the curve (Figure 4). So if (and this is often a big if) we are justified in assuming that the data were obtained from an approximately normal distribution we have a useful summary of the population in the form of the mean and the standard deviation.


![plot of chunk unnamed-chunk-24](figure/unnamed-chunk-24.png) 



![plot of chunk unnamed-chunk-25](figure/unnamed-chunk-25.png) 


You should read more details regarding the properties of the normal distribution in Crawley.


### Checking data properties with figures

Both boxplots and histograms are very useful tools for checking whether the data values in a variable are approximately normally distributed. A normal distribution results in a more or less symmetrical histogram if the sample is large. If the sample is small the histogram is unlikely to be completely symmetrical as a result of sampling effects, even if the sample is taken from a population with an approximately normal distribution. Always be very careful when interpreting small samples.

The whiskers of a boxplot extend (more or less) to the extreme that would be expected under a normal distribution. If there are many outliers beyond this point it is a good indication that the normal distribution may not be a good fit to the data, especially if all the outliers fall on one side. For example, lets see what some intentionally skewed data look like as a histogram and boxplot.



```r
x <- rlnorm(100, mean = 2, sd = 1.2)
hist(x)
```

![plot of chunk unnamed-chunk-26](figure/unnamed-chunk-261.png) 

```r
boxplot(x)
```

![plot of chunk unnamed-chunk-26](figure/unnamed-chunk-262.png) 



A few outliers are to be expected in all data sets. They may be genuine ``errors'' or they may turn out to be the most interesting elements of the data. However in this case there is clear visual evidence of skew.

![plot of chunk unnamed-chunk-27](figure/unnamed-chunk-27.png) 



![plot of chunk unnamed-chunk-28](figure/unnamed-chunk-28.png) 



![plot of chunk unnamed-chunk-29](figure/unnamed-chunk-29.png) 



Histograms and boxplots are good starting points for getting a feel for how the variability in data is distributed. There are other more sophisticated techniques for looking at data distributions and checking modelling assumptions that we will look at in detail next term.


## Descriptives in SPSS
 
*  You first need to import the data into SPSS using the ``Read text data'' option under the file menu. Follow most of the default steps but set the file delimiter to comma and also make sure that you set the option to read in the first line as column headers.
 
![alt text](primerfigs/spss1.png)
 
After importing the data you can save them in the SPSS *.sav format. R can read this files as well after loading the package ``foreign''. We will look at data manipulation and data sharing between software next term.
*  There are two views of the data in SPSS. The variable view shows the properties of the variables. Notice that the variable ``Site'' should be a ``Nominal'' measure in SPSS jargon. This is because although the sites are numbered, the numbers are names. You cannot add and subtract them. Such variables are known as factors in R.
 
![alt text](primerfigs/spss2.png)
 
*  The data view shows the numbers for each variable.
 
![alt text](primerfigs/spss3.png)
 
*  Now, to obtain descriptives you can use the descriptives option under the analyses menu.
 
![alt text](primerfigs/spss4.png)
 
*  SPSS provides a formatted table in the output window.
 
![alt text](primerfigs/spss5.png)
 
*  Try producing histograms and boxplots using the chart builder. 
 
![alt text](primerfigs/spss6.png)
 
*  You simply need to drag and drop the variable(s) into place and choose some options. The results are sent to the output window.
 
![alt text](primerfigs/spss7.png)
 
*  Notice that if you double click on the chart you can edit it.
 
![alt text](primerfigs/chart.png)
 
*  Quantiles and percentiles can be obtained along with some other descriptive stats using the frequencies menu. There are often several different ways of obtaining the same result in SPSS.
 
![alt text](primerfigs/spss8.png)


### Reading

Chapters 3 to 5 of Crawley. Again you do not need to read the more advanced elements in the text. Concentrate on explanations of histograms, boxplots and variance in order to reinforce this text.


# Exercises **IMPORTANT**

Results from the exercises may be asked about in questions on the online tests.

## Rainfall in the UK

June 2012 was reported to have been the wettest on record in Southern England, although other parts of the country were slightly less exceptional.

Aggregated climate statistics are available from the met office 
http://www.metoffice.gov.uk/climate/uk/datasets
I have downloaded and formatted the compiled rainfall data for the whole of England. You can read it into R by pasting in the line below.



```r
rain <- read.csv("http://tinyurl.com/QEcol2013/Rainfall.csv")
str(rain)
```

```
## 'data.frame':	103 obs. of  18 variables:
##  $ Year: int  1910 1911 1912 1913 1914 1915 1916 1917 1918 1919 ...
##  $ JAN : num  82.9 38.4 99.8 110.5 43 ...
##  $ FEB : num  98.5 57.6 57.3 32.1 76.7 ...
##  $ MAR : num  26.8 55.8 108.6 98.1 105.3 ...
##  $ APR : num  63.5 44 7.8 89.4 30.5 33.3 42.1 52.1 55.9 59.5 ...
##  $ MAY : num  64.2 36.6 55.8 61.9 42.7 61.2 64.4 56.3 53.3 25.2 ...
##  $ JUN : num  64.6 73 116.2 34.6 55.3 ...
##  $ JUL : num  79.8 13.2 90.7 32.5 89.4 ...
##  $ AUG : num  100.8 50.3 170.5 39.4 55.7 ...
##  $ SEP : num  15.7 56.2 49.5 59.3 43.6 ...
##  $ OCT : num  86.7 83.2 90.5 95.6 61.6 ...
##  $ NOV : num  109.9 104 69.1 82.8 102.9 ...
##  $ DEC : num  124.6 160.1 102 49.2 179 ...
##  $ WIN : num  NA 221 317 244 169 ...
##  $ SPR : num  154 136 172 249 178 ...
##  $ SUM : num  245 137 377 106 200 ...
##  $ AUT : num  212 244 209 238 208 ...
##  $ ANN : num  918 773 1018 785 886 ...
```




If you attach the data frame you can refer to the variable for the month of June by typing JUN. 



```r
attach(rain)
str(JUN)
```

```
##  num [1:103] 64.6 73 116.2 34.6 55.3 ...
```




Produce a boxplot, a histogram and data summary for the variable. How do you interpret the fact that the recorded aggregate rainfall for June 2012 was 142.6 mm in the context of the variability between years?

What about the months of July and August in 2012? How do these values compare with the mean and median?

Do the histograms appear symmetrical? 

Are the data approximately normally distributed? 

How do outliers appear on the boxplots?

If you want to use SPSS you can download the file and then import it as shown last week. Check your browser settings if you open the text instead of downloading it or try using 

 
## Declared earnings in the UK

The inland revenue publish figures on declared earnings for income tax. A sample of 1000 numbers for this data for 2009 is included in the file ``Earnings.csv''.



```r
earn <- read.csv("http://tinyurl.com/QEcol2013/Earnings.csv")
attach(earn)
str(Earnings)
```

```
##  num [1:10000] 6.98 7.06 7.45 6.8 7.33 ...
```




Look at these data using boxplots and histograms. Describe the distribution. What does this imply? Calculate the mean and median. Are they similar. Why not?
 
