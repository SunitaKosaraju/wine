# Wine Lovers Unite!
## Summary 
In this project, I aim to beat wine experts in predicting the quality of redwine from a given region in Portugal. I coded this project in R.  Here, I reflect on the use of linear regression, KNN classification and compare my results against a group of Portuguese academics who did similar analysis using neural networks.

##Introduction

The wine data set includes variables for the following features of wine :fixed.acidity, volatile.acidity,citric.acid, residual.sugar, chlorides, free.sulfur.dioxide, total.sulfur.dioxide,density, pH	sulphates, and alcohol

## Approaches:
At its heart, this is a classification problem.  The idea is to predict the class of a given wine based on its chemical properties.  I tried 2 approaches for this problem.  First linear regression, then knn.  I used 10 fold cross validation using the Weka tool for both models in order to check the accuracy of my predictions.  

*Note- I used linear regression (with rounding) for experimental purposes.  It's not the most elegant/ appropriate  method for a classification problem of this type. 

## Exploration 

To start with, I examined the data.  First, I found that there is no missing data.  

I saw that the quality variable had the following distribution: 


table(wine$quality)
3   4   5   6   7   8 
10  53 681 638 199  18 


This table shows that the most common quality rank is 5.  If we thus set this as the baseline, the model is accurate 42.5% of the time.  Thus, any model that I use, must have an accuracy rate better than 42.5% threshold.


## Part 1) Linear Regression
I tried linear regression.  The reason is because I wanted to see which variables affect the quality of wine (and how much they affect the quality).  I understood that the quality variable is a discrete one while linear regression is usually more appropriate for continuous outcome variables, so in my code, I rounded outcome results so that they would match the discrete distribution in the training set so that I could check the accuracy of the model against the training set.  

My Task, Experience and Performance variables are set as follows: 

•	Task: To predict the quality of wine.  Quality of wine is organized into 8 possible classes.
•	Experience : Using linear regression.
•	Performances :  After trying various permutations of the data I performed a step-wise linear regression to find the best model using AIC.  

This was: model1<- lm(quality ~ alcohol + volatile.acidity + sulphates + total.sulfur.dioxide + chlorides + pH + free.sulfur.dioxide, data = wine)

I observed the R-square, RSME and accuracy of the model.  This results in an R-square of 0.3567 and a standard error of 0.6477.  This concerns me as the R-square is very low and the standard error is very high!  This makes me conclude that perhaps linear regression is NOT the best algorithm for this dataset. I run a Pred(25) function to check the error distribution of the model and note that 95% of the data has less than 25% of error from the expected value.  This seems to conflict with the high standard error.    Finally, I check the accuracy of the model using a confusion matrix.  I find that after rounding the outcome variables, the accuracy is 60%.  This is close to the academic researchers accuracy rate of 63%.  

Given the low R-square, and the discrepancy in the error analysis, I decided to use another algorithm (classification) and compare the results by looking at the errors and accuracy of the two algorithms.  

## Part 2)  KNN Classification

I tried a Knn approach using cross validation.  I tried this approach by converting quality to a factor and then using the rWeka and rJava tools to calculate Knn.
•	Task: To predict the quality of wine.  Quality of wine is organized into 8 possible classes.
•	Experience : Knn using 10 fold cross validation
•	Performances : The K value of 1 results in 65% accuracy.  I will use this model for predicting wine quality rather than using the linear regression.  The recall is also 65%.

## Part 3) Comparing my work against Portuguese academics

 Other researchers have built predictive models using this data.  For example,  researchers from the University of Minho in Portugal were able to predict the correct class of wine with an accuracy of 62.5% (where T=0.5).  
 In comparison, my model predicted 65% accuracy.  (See their paper at http://repositorium.sdum.uminho.pt/bitstream/1822/10029/1/wine5.pdf)

Before fitting the models, the researchers first standardized the data to a zero mean and one standard deviation.  The research team used  the Broyden–Fletcher–Goldfarb–Shanno (BFGS) algorithm to train the Neural Networks.  This algorithm is useful for solving unconstrained nonlinear optimization problems.  The team also used Support Vector Machines  provided by LIBSVM (kernlab package).  By contrast, I attempted to use linear regression and settled on the Knn classification method.
