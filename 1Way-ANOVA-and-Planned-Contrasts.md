One Way ANOVA & Planned Contrasts
================

#### Load in your packages

``` r
library(pacman)
pacman::p_load(psych, car, ggplot2, foreign, haven, nortest, psych)
```

#### Load in your data

``` r
ClassE_G_3Siegelstudy <- read.csv("ClassE.G.3Siegelstudy.csv")
```

``` r
str(ClassE_G_3Siegelstudy)
```

    ## 'data.frame':    40 obs. of  3 variables:
    ##  $ ID   : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ Group: int  1 1 1 1 1 1 1 1 2 2 ...
    ##  $ Time : int  3 5 1 8 1 1 4 9 2 12 ...

#### Check out your data

``` r
str(ClassE_G_3Siegelstudy)
```

    ## 'data.frame':    40 obs. of  3 variables:
    ##  $ ID   : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ Group: int  1 1 1 1 1 1 1 1 2 2 ...
    ##  $ Time : int  3 5 1 8 1 1 4 9 2 12 ...

``` r
ClassE_G_3Siegelstudy$Group <- factor(ClassE_G_3Siegelstudy$Group)
```

## Doing the ANOVA

``` r
ClassE_G_3Siegelstudy.anova <- aov(Time ~ Group, data = ClassE_G_3Siegelstudy) #The formula for doing an anova is aov(x ~ y, data = ***)
summary(ClassE_G_3Siegelstudy.anova)
```

    ##             Df Sum Sq Mean Sq F value   Pr(>F)    
    ## Group        4   3498   874.4   27.32 2.44e-10 ***
    ## Residuals   35   1120    32.0                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

## Contrasts

#### Contrast between two groups

``` r
t.test(formula = Time ~ Group,
                       data = ClassE_G_3Siegelstudy,
                       subset = Group %in% c(1, 4)) #Between Groups 1 and 4
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  Time by Group
    ## t = -7.9547, df = 10.253, p-value = 1.063e-05
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -25.5834 -14.4166
    ## sample estimates:
    ## mean in group 1 mean in group 4 
    ##               4              24

#### Contrast Matrix

``` r
#Set up a matrix of the contrasts - last two numbers (5,5) define the dimensions of the matrix
contrasts(ClassE_G_3Siegelstudy$Group)<-matrix(c(0,1,-1,0,0,0,0,0,1,-1,1, -.5, -.5, 0, 0,1/3,1/3,1/3,-.5,-.5,0, .5,.5,-.5,-.5), 5,5)

anova.w.contrasts<-aov(Time~Group, data = ClassE_G_3Siegelstudy, contrasts = contrasts(ClassE_G_3Siegelstudy$Group))
```

    ## Warning in model.matrix.default(mt, mf, contrasts): non-list contrasts argument
    ## ignored

``` r
summary(anova.w.contrasts,split=list(Group=list("2 vs 3"=1,"4 vs 5"=2, "1 vs 2 & 3" = 3,"1,2,3 vs 4&5"=4, "Avg 2 & 3 vs Avg 4 & 5" = 5)))
```

    ##                                 Df Sum Sq Mean Sq F value   Pr(>F)    
    ## Group                            4   3498     874  27.325 2.44e-10 ***
    ##   Group: 2 vs 3                  1      4       4   0.125   0.7258    
    ##   Group: 4 vs 5                  1    100     100   3.125   0.0858 .  
    ##   Group: 1 vs 2 & 3              1    225     225   7.042   0.0119 *  
    ##   Group: 1,2,3 vs 4&5            1   3168    3168  99.008 9.66e-12 ***
    ##   Group: Avg 2 & 3 vs Avg 4 & 5  1                                    
    ## Residuals                       35   1120      32                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

#### Pairwise T Test with Bonferroni adjustment

``` r
pairwise.t.test(ClassE_G_3Siegelstudy$Time, 
                ClassE_G_3Siegelstudy$Group, 
                p.adjust = "bonferroni")
```

    ## 
    ##  Pairwise comparisons using t tests with pooled SD 
    ## 
    ## data:  ClassE_G_3Siegelstudy$Time and ClassE_G_3Siegelstudy$Group 
    ## 
    ##   1       2       3       4      
    ## 2 0.41051 -       -       -      
    ## 3 0.18319 1.00000 -       -      
    ## 4 3.1e-07 0.00019 0.00054 -      
    ## 5 1.9e-09 8.9e-07 2.6e-06 0.85818
    ## 
    ## P value adjustment method: bonferroni
