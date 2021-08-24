T Tests Intro
================

``` r
# Load in your packages
library(stats)
library(psych)
library(readr)

#read in your data
PDIdf <- read.csv("ClassE.G.1.PDIscore.csv")
```

``` r
t.test(PDIdf$PDIscore, mu = 100)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  PDIdf$PDIscore
    ## t = 2.4529, df = 55, p-value = 0.01737
    ## alternative hypothesis: true mean is not equal to 100
    ## 95 percent confidence interval:
    ##  100.7549 107.4951
    ## sample estimates:
    ## mean of x 
    ##   104.125

``` r
describeBy(PDIdf$PDIscore, mu = 100)
```

    ## Warning in describeBy(PDIdf$PDIscore, mu = 100): no grouping variable requested

    ##    vars  n   mean    sd median trimmed   mad min max range skew kurtosis   se
    ## X1    1 56 104.12 12.58    102  103.74 14.83  83 127    44 0.27    -1.21 1.68

## Independent Samples T-Test

``` r
library(stats)
library(psych)
library(car)
```

    ## Loading required package: carData

    ## 
    ## Attaching package: 'car'

    ## The following object is masked from 'package:psych':
    ## 
    ##     logit

``` r
library(readr)

homophobia <- read.csv("ClassE.G.2.Ch.7-5_Homophobia.csv")
```

#### Take a look at the dataset

``` r
str(homophobia)
```

    ## 'data.frame':    64 obs. of  3 variables:
    ##  $ ID     : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ Arousal: num  39.1 38 14.9 20.7 19.5 32.2 11 20.7 26.4 35.7 ...
    ##  $ Group  : int  1 1 1 1 1 1 1 1 1 1 ...

#### Descriptives by Group

``` r
homophobia$Group <- as.factor(homophobia$Group)
describeBy(homophobia$Arousal, homophobia$Group)
```

    ## 
    ##  Descriptive statistics by group 
    ## group: 1
    ##    vars  n mean   sd median trimmed   mad min  max range skew kurtosis   se
    ## X1    1 35   24 12.2   20.7   23.38 13.79 5.3 54.1  48.8 0.47    -0.69 2.06
    ## ------------------------------------------------------------ 
    ## group: 2
    ##    vars  n mean   sd median trimmed   mad   min  max range  skew kurtosis   se
    ## X1    1 29 16.5 11.8     18   17.05 10.23 -15.5 35.8  51.3 -0.64      0.1 2.19

#### Plot it using ggplot2

``` r
library(ggplot2)
```

    ## 
    ## Attaching package: 'ggplot2'

    ## The following objects are masked from 'package:psych':
    ## 
    ##     %+%, alpha

``` r
ggplot(homophobia, aes(as.factor(Group), Arousal)) +
  stat_summary(fun = mean, geom = "bar") + 
  xlab("Group")
```

![](T-Tests_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

#### Test Assumption of Homogeneity

``` r
leveneTest(homophobia$Arousal ~ homophobia$Group, center = mean)
```

    ## Levene's Test for Homogeneity of Variance (center = mean)
    ##       Df F value Pr(>F)
    ## group  1  0.3911  0.534
    ##       62

#### Do the t test

``` r
t.test(homophobia$Arousal ~ homophobia$Group, var.equal=TRUE)
```

    ## 
    ##  Two Sample t-test
    ## 
    ## data:  homophobia$Arousal by homophobia$Group
    ## t = 2.4837, df = 62, p-value = 0.01572
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##   1.46298 13.53012
    ## sample estimates:
    ## mean in group 1 mean in group 2 
    ##        24.00000        16.50345

## Paired Samples T-Test

``` r
library(foreign)
library(psych)
library(stats)
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ tibble  3.0.6     ✓ dplyr   1.0.4
    ## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
    ## ✓ purrr   0.3.4     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x ggplot2::%+%()   masks psych::%+%()
    ## x ggplot2::alpha() masks psych::alpha()
    ## x dplyr::filter()  masks stats::filter()
    ## x dplyr::lag()     masks stats::lag()
    ## x dplyr::recode()  masks car::recode()
    ## x purrr::some()    masks car::some()

``` r
library(dplyr)

TTEG <- read.spss("ClassE.G.3.Anorexia.Tab7-3.sav", to.data.frame=T)
```

    ## re-encoding from CP1252

#### Manipulate your data if you need to

``` r
anorexia <- TTEG #can rename objects like this
anorexia <- select(anorexia, "Before", "After") #keeps selected columns, creates new data frame object
```

#### Check out your data

``` r
str(anorexia)
```

    ## 'data.frame':    17 obs. of  2 variables:
    ##  $ Before: num  83.8 83.3 86 82.5 86.7 79.6 76.9 94.2 73.4 80.5 ...
    ##  $ After : num  95.2 94.3 91.5 91.9 100.3 ...

``` r
describeBy(anorexia$Before)
```

    ## Warning in describeBy(anorexia$Before): no grouping variable requested

    ##    vars  n  mean   sd median trimmed  mad  min  max range skew kurtosis   se
    ## X1    1 17 83.23 5.02   83.3   83.15 4.15 73.4 94.2  20.8 0.15    -0.29 1.22

``` r
describeBy(anorexia$After)
```

    ## Warning in describeBy(anorexia$After): no grouping variable requested

    ##    vars  n  mean   sd median trimmed  mad  min   max range  skew kurtosis   se
    ## X1    1 17 90.49 8.49   92.6   90.77 3.85 75.2 101.6  26.4 -0.74    -0.93 2.06

#### Visualize

``` r
#you don't have to use ggplot2 to make plots, base R also has tools for data visualization
anorexia_mean <- sapply(anorexia, mean)
barplot(anorexia_mean,
        col = "orange",
        main = "Barplot",
        ylab = "Mean")
```

![](T-Tests_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

#### Do the T Test

``` r
t.test(anorexia$Before, anorexia$After, paired = TRUE)
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  anorexia$Before and anorexia$After
    ## t = -4.1802, df = 16, p-value = 0.0007072
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -10.948840  -3.580571
    ## sample estimates:
    ## mean of the differences 
    ##               -7.264706

## T Test Wrap Up

``` r
library(foreign)
library(tidyverse)
library(car)
library(ggplot2)
library(foreign)
library(haven)
PDI <- read_sav("ClassE.G.1.PDIscore.sav")
```

#### One sample statistics

``` r
describeBy(PDI$PDIscore)
```

    ## Warning in describeBy(PDI$PDIscore): no grouping variable requested

    ##    vars  n   mean    sd median trimmed   mad min max range skew kurtosis   se
    ## X1    1 56 104.12 12.58    102  103.74 14.83  83 127    44 0.27    -1.21 1.68

#### One Sample T Tests

``` r
t.test(PDI$PDIscore, mu=100)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  PDI$PDIscore
    ## t = 2.4529, df = 55, p-value = 0.01737
    ## alternative hypothesis: true mean is not equal to 100
    ## 95 percent confidence interval:
    ##  100.7549 107.4951
    ## sample estimates:
    ## mean of x 
    ##   104.125

## Two Sample T Tests

``` r
Homophobia <- read_sav("ClassE.G.2.Ch.7-5_Homophobia.sav")
```

#### Descriptives by Group

``` r
describeBy(Homophobia$Arousal, group = Homophobia$Group)
```

    ## 
    ##  Descriptive statistics by group 
    ## group: 1
    ##    vars  n mean   sd median trimmed   mad min  max range skew kurtosis   se
    ## X1    1 35   24 12.2   20.7   23.38 13.79 5.3 54.1  48.8 0.47    -0.69 2.06
    ## ------------------------------------------------------------ 
    ## group: 2
    ##    vars  n mean   sd median trimmed   mad   min  max range  skew kurtosis   se
    ## X1    1 29 16.5 11.8     18   17.05 10.23 -15.5 35.8  51.3 -0.64      0.1 2.19

#### Levene Test for Homogeneity of Variance

``` r
t.test(Homophobia$Arousal ~ Homophobia$Group, var.equal = TRUE)
```

    ## 
    ##  Two Sample t-test
    ## 
    ## data:  Homophobia$Arousal by Homophobia$Group
    ## t = 2.4837, df = 62, p-value = 0.01572
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##   1.46298 13.53012
    ## sample estimates:
    ## mean in group 1 mean in group 2 
    ##        24.00000        16.50345

#### Another Example - Paired Sample

``` r
Anorexia <- read_sav("ClassE.G.3.Anorexia.Tab7-3.sav")
```

``` r
describeBy(Anorexia)
```

    ## Warning in describeBy(Anorexia): no grouping variable requested

    ##        vars  n  mean   sd median trimmed  mad  min   max range  skew kurtosis
    ## ID        1 17  9.00 5.05    9.0    9.00 5.93  1.0  17.0  16.0  0.00    -1.41
    ## Before    2 17 83.23 5.02   83.3   83.15 4.15 73.4  94.2  20.8  0.15    -0.29
    ## After     3 17 90.49 8.49   92.6   90.77 3.85 75.2 101.6  26.4 -0.74    -0.93
    ##          se
    ## ID     1.22
    ## Before 1.22
    ## After  2.06

#### Do the Paired Samples T Test

``` r
t.test(Anorexia$Before, Anorexia$After, paired = TRUE)
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  Anorexia$Before and Anorexia$After
    ## t = -4.1802, df = 16, p-value = 0.0007072
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -10.948840  -3.580571
    ## sample estimates:
    ## mean of the differences 
    ##               -7.264706
