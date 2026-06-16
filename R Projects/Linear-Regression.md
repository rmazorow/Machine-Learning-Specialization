Linear Regression with R
================
Rocky Mazorow
2026-06-16

Linear Regression in R - Full Project for Beginners by Alejandro AO

Project Goal: Predict how much a customer will spend per year.

Video: <https://www.youtube.com/watch?v=Rdq2oEWg3KU> Code:
<https://github.com/alejandro-ao/ecommerce-project-r> Dataset:
<https://www.kaggle.com/datasets/kolawale/focusing-on-mobile-app-or-website/data>
Import data

# Import data

    ## 'data.frame':    500 obs. of  8 variables:
    ##  $ Email               : chr  "mstephenson@fernandez.com" "hduke@hotmail.com" "pallen@yahoo.com" "riverarebecca@gmail.com" ...
    ##  $ Address             : chr  "835 Frank Tunnel\nWrightmouth, MI 82180-9605" "4547 Archer Common\nDiazchester, CA 06566-8576" "24645 Valerie Unions Suite 582\nCobbborough, DC 99414-7564" "1414 David Throughway\nPort Jason, OH 22070-1220" ...
    ##  $ Avatar              : chr  "Violet" "DarkGreen" "Bisque" "SaddleBrown" ...
    ##  $ Avg..Session.Length : num  34.5 31.9 33 34.3 33.3 ...
    ##  $ Time.on.App         : num  12.7 11.1 11.3 13.7 12.8 ...
    ##  $ Time.on.Website     : num  39.6 37.3 37.1 36.7 37.5 ...
    ##  $ Length.of.Membership: num  4.08 2.66 4.1 3.12 4.45 ...
    ##  $ Yearly.Amount.Spent : num  588 392 488 582 599 ...

    ##        Email          Address          Avatar    Avg..Session.Length
    ##  Length   :500   Length   :500   Length   :500   Min.   :29.53      
    ##  N.unique :500   N.unique :500   N.unique :138   1st Qu.:32.34      
    ##  N.blank  :  0   N.blank  :  0   N.blank  :  0   Median :33.08      
    ##  Min.nchar: 13   Min.nchar: 23   Min.nchar:  3   Mean   :33.05      
    ##  Max.nchar: 39   Max.nchar: 67   Max.nchar: 20   3rd Qu.:33.71      
    ##                                                  Max.   :36.14      
    ##   Time.on.App     Time.on.Website Length.of.Membership Yearly.Amount.Spent
    ##  Min.   : 8.508   Min.   :33.91   Min.   :0.2699       Min.   :256.7      
    ##  1st Qu.:11.388   1st Qu.:36.35   1st Qu.:2.9304       1st Qu.:445.0      
    ##  Median :11.983   Median :37.07   Median :3.5340       Median :498.9      
    ##  Mean   :12.052   Mean   :37.06   Mean   :3.5335       Mean   :499.3      
    ##  3rd Qu.:12.754   3rd Qu.:37.72   3rd Qu.:4.1265       3rd Qu.:549.3      
    ##  Max.   :15.127   Max.   :40.01   Max.   :6.9227       Max.   :765.5

# Exploratory data analysis

Use ggplot geom_point to individually see what relationships exists with
Yearly Amount Spent. There does not seem to be a correlation with Time
on Website; however, there is a very faint positive correlation with
Session Length.

![](Linear-Regression_files/figure-gfm/plot_indiv-1.png)<!-- -->![](Linear-Regression_files/figure-gfm/plot_indiv-2.png)<!-- -->
Alternatively, we can use pair plots to view the relationships across
all the data. Looking only at Yearly Amount Spent, there is a strong
positive relationship with Length of Membership.

![](Linear-Regression_files/figure-gfm/plot_pair-1.png)<!-- -->

## Exploring the selected variable(s)

Based on the pairplot, let’s evaluate Length of Membership. First, let’s
check if the variable normally distributed.

![](Linear-Regression_files/figure-gfm/plot_hist-1.png)<!-- -->![](Linear-Regression_files/figure-gfm/plot_hist-2.png)<!-- -->

# Simple linear regression model

For a simple linear regression model, you have a single variable
influencing your response variable.

$$ \hat{y} = \hat{\beta_0} + \hat{\beta_1}x$$ \## Fitting a linear model

    ## 
    ## Call:
    ## lm(formula = Yearly.Amount.Spent ~ Length.of.Membership)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -125.975  -29.032   -0.494   33.033  147.777 
    ## 
    ## Coefficients:
    ##                      Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)           272.400      7.675   35.49   <2e-16 ***
    ## Length.of.Membership   64.219      2.090   30.72   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 46.66 on 498 degrees of freedom
    ## Multiple R-squared:  0.6546, Adjusted R-squared:  0.6539 
    ## F-statistic: 943.9 on 1 and 498 DF,  p-value: < 2.2e-16

![](Linear-Regression_files/figure-gfm/plot_lm-1.png)<!-- -->

## Residuals analysis

Linear models assume that the residuals of the predictions are normally
distributed. If they are not normally distributed, the model may be
biased.

![](Linear-Regression_files/figure-gfm/plot_res-1.png)<!-- -->![](Linear-Regression_files/figure-gfm/plot_res-2.png)<!-- -->

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  residuals(lm.fit)
    ## W = 0.99756, p-value = 0.6837

## Training and evaluation of model

Our previous model used all our data for training. We will know split
the data into a training set (70%) and test set (30%) to check the
validity of our model.

    ## 
    ## Call:
    ## lm(formula = Yearly.Amount.Spent ~ Length.of.Membership, data = train)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -130.942  -29.058    1.151   35.164  140.117 
    ## 
    ## Coefficients:
    ##                      Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)           267.351      9.359   28.57   <2e-16 ***
    ## Length.of.Membership   66.582      2.531   26.30   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 48.13 on 348 degrees of freedom
    ## Multiple R-squared:  0.6654, Adjusted R-squared:  0.6644 
    ## F-statistic:   692 on 1 and 348 DF,  p-value: < 2.2e-16

To quantitatively evaluate our model, we can look at some values related
to the model errors: Mean Absolute Percentage Error (MAPE), $R^2$, and
Root Mean Squared Error (RMSE) . The unit of these values matches the
unit of your target variable y, in this case \$.

    ##        MAPE          R2        RMSE 
    ##  0.07546202  0.66537222 43.69925455

# Multivariable linear regression model

More often than not, you have more than a single variable influencing
your response variable. The model will return one coefficient per
variable to represent the variable’s weight (estimate) for the model. In
this case, your model may look more like:

$$ \hat{y} = \hat{\beta_0} + \hat{\beta_1}x_1 + \hat{\beta_2}x_2 + \hat{\beta_3}x_3 + \hat{\beta_4}x_4$$

    ## 
    ## Call:
    ## lm(formula = Yearly.Amount.Spent ~ Length.of.Membership + Avg..Session.Length + 
    ##     Time.on.App + Time.on.Website)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -30.4059  -6.2191  -0.1364   6.6048  30.3085 
    ## 
    ## Coefficients:
    ##                        Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)          -1051.5943    22.9925 -45.736   <2e-16 ***
    ## Length.of.Membership    61.5773     0.4483 137.346   <2e-16 ***
    ## Avg..Session.Length     25.7343     0.4510  57.057   <2e-16 ***
    ## Time.on.App             38.7092     0.4510  85.828   <2e-16 ***
    ## Time.on.Website          0.4367     0.4441   0.983    0.326    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 9.973 on 495 degrees of freedom
    ## Multiple R-squared:  0.9843, Adjusted R-squared:  0.9842 
    ## F-statistic:  7766 on 4 and 495 DF,  p-value: < 2.2e-16

You should still perform a residual analysis as explained above.

We see that Time on Website is not a significant model (p \> 0.05) and
can be removed from the model.

## Training and evaluation of model

Our previous model used all our data for training. We will know split
the data into a training set (70%) and test set (30%) to check the
validity of our model.

    ## 
    ## Call:
    ## lm(formula = Yearly.Amount.Spent ~ Length.of.Membership + Avg..Session.Length + 
    ##     Time.on.App + Time.on.Website, data = train)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -31.045  -6.609  -0.142   6.464  29.640 
    ## 
    ## Coefficients:
    ##                        Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)          -1063.6628    27.9515  -38.05   <2e-16 ***
    ## Length.of.Membership    61.6186     0.5533  111.38   <2e-16 ***
    ## Avg..Session.Length     26.0353     0.5547   46.94   <2e-16 ***
    ## Time.on.App             38.8576     0.5547   70.05   <2e-16 ***
    ## Time.on.Website          0.4528     0.5662    0.80    0.424    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 10.42 on 345 degrees of freedom
    ## Multiple R-squared:  0.9844, Adjusted R-squared:  0.9843 
    ## F-statistic:  5457 on 4 and 345 DF,  p-value: < 2.2e-16

To quantitatively evaluate our model, we can look at some values related
to the model errors: Mean Absolute Percentage Error (MAPE), $R^2$, and
Root Mean Squared Error (RMSE) . The unit of these values matches the
unit of your target variable y, in this case \$.

    ##       MAPE         R2       RMSE 
    ## 0.01506234 0.98443972 8.90585297
