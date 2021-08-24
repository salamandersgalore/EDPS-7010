Lab 5 - 1 Way Between ANOVA
================
Sam Dutton

### Lab 5 - 1 Way Between Subjects ANOVA

``` r
library(pacman)
pacman::p_load(psych, car, ggplot2, foreign, haven, nortest, psych,tidyverse,Rmisc)
```

``` r
lab.five <- read.spss("Lab5_Eysenck_1way.sav", to.data.frame = T,stringsAsFactors = TRUE)
```

#### **Pre-lab questions**:

##### How many factors? *1*

##### How many levels? *5*

##### What kind of design? *1 x 5 ANOVA*

#### **Part 1**

##### **Step 1**: Write the null and alternative hypotheses.

###### Null: There will be no difference in mean words recalled between groups

###### Alternative: There will be a difference in mean words recalled between groups.

##### **Step 2**: Set the criteria for a decision

###### Set the alpha at *p*&lt;.05

##### **Step 3**:

###### running descriptives in this section as well with a bar graph

``` r
str(lab.five)
```

    ## 'data.frame':    50 obs. of  2 variables:
    ##  $ Condition: num  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ Recall   : num  9 8 6 8 10 4 6 5 7 7 ...
    ##  - attr(*, "codepage")= int 65001

``` r
lab.five$Condition<-factor(lab.five$Condition)
re.mean <-mean(lab.five[,2])

ggplot(lab.five, aes(as.factor(Condition), Recall)) + 
  stat_summary(fun.y = mean, geom = "bar", na.rm = TRUE) 
```

    ## Warning: `fun.y` is deprecated. Use `fun` instead.

![](Lab-5---1Way-btwn-ANOVA_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
five.anova <- aov(Recall ~ Condition, data = lab.five)
summary(five.anova)
```

    ##             Df Sum Sq Mean Sq F value   Pr(>F)    
    ## Condition    4  351.5   87.88   9.085 1.82e-05 ***
    ## Residuals   45  435.3    9.67                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

##### **Step 4**: Make a decision. Report results in APA.

###### A one-way ANOVA was conducted to examine if word recall differed by types of processing. A significant effect of word processing type was found, *F*(4, 45)= 9.09, *p* &lt; .05.

### **Part 2**

###### Write the null and alternative hypothesis for testing the *assumption of normality*.

###### Null: The sample will be normally distributed.

###### Alternative: The sample will not be normal; it is not normally distributed.

###### *Determine if the assumption of homogeneity is met.*

###### running a k-s test

``` r
ks.test(lab.five$Recall, pnorm, mean(lab.five$Recall), sd(lab.five$Recall))
```

    ## Warning in ks.test(lab.five$Recall, pnorm, mean(lab.five$Recall),
    ## sd(lab.five$Recall)): ties should not be present for the Kolmogorov-Smirnov test

    ## 
    ##  One-sample Kolmogorov-Smirnov test
    ## 
    ## data:  lab.five$Recall
    ## D = 0.14727, p-value = 0.2283
    ## alternative hypothesis: two-sided

###### **b. Is the assumption of homogeneity met?** *Yes, it is.*

###### **c. report in APA format**

The recall scores did not deviate significantly from normal, D(49)=
0.15, p = 0.23.

### **Part 3**

##### *Write the null and alternative hypothesis for testing the assumption of homogeneity of variance.*

###### Null: There will be homogeneity of variance between groups. \#\#\#\#\#\#\# Alternative: There will not be homogeneity of variance between groups.

##### *b. determine if the assumption of homogeneity is met*

###### Levene’s test below.

``` r
levene <- leveneTest(lab.five$Recall, lab.five$Condition, center = mean)
levene
```

    ## Levene's Test for Homogeneity of Variance (center = mean)
    ##       Df F value  Pr(>F)  
    ## group  4  2.5287 0.05356 .
    ##       45                  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

###### Yes, the assumption of homogeneity is met. The null hypothesis is not rejected.

##### *c. Report the results in APA format*

###### The variances for the 5 levels of word processing were not significantly different, *F*(4, 45)=2.53, *p*&gt;.05.
