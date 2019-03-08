
# Interpreting Significance and P-values 

## Introduction

In this lesson, you'll learn more about some of the ideas mentioned in earlier lessons around interpreting significance of results. You'll learn that associating a confidence level to our model's output is be very important during decision making, and an essential part of data science.

## Objectives

You will be able to:

* Interpret p-values and regression coefficients
* Describe model suitability in terms of obtained significance results

## Let's get started
The ideas of hypothesis testing can be applied to regression, as well as statistical inference. For now, You'll learn about how it works in regression, and we'll spend time on formal hypothesis testing in a variety of contexts later on.

When performing hypothesis tests, a set of hypotheses are developed including a so-called **null hypothesis** and an **alternative hypothesis**. 
In regression, you will generally try to reject the null hypothesis, because that means that there is some relationship between the dependent and independent variable and your model at hand makes sense. Rejecting or not rejecting a null hypothesis always goes hand in hand with a so-called **confidence level**, often defined by the greek letter alpha, $\alpha$.

## Hypothesis Testing in Regression 

During regression, you try to measure the model parameters (coefficients). The null and alternative hypotheses are also get set up in those terms. Think about the simple regression model you created using the advertising dataset. For a simple dataset like this, you can set up the hypotheses as follows:

> **Null Hypothesis ($H_0$)**: There is no relationship between amount spent on TV advertisement and sales figures

> **Alternative Hypothesis ($H_a$):** There is "some" relation between amount spent on TV advertisement and sales figures

If we reject the null hypothesis, it means that we believe that there is some sort of relationship between TV advertisement and sales.
If we fail to reject the null hypothesis, it means that we believe that  there is no _significant_ relationship between TV advertisement and sales, and that our slope parameter $m$ in $y= mx+c$ is not _significantly_ different from zero. 

You see that we used the word _significant_ and _significantly_. What does this mean, and when is something _significant_?

## P-value as a level of statistical significance

When performing statistical analyses, rejecting or not rejecting a null hypothesis always goes along with an associated **significance level** or **p-value**.

> p-value represents a **probability of observing your results (or something more extreme) given that the null hypothesis is true**
 
Applied to a regression model, p-values associated with coefficients estimates indicates the probability of observing the associated coefficient given that the null-hypothesis is true. As a result, very small p-values indicate that coefficients are **statistically significant**. A very commonly used cut-off value for the p-value is 0.05. If your p-value is smaller than 0.05, you would say:

> The parameter is statistically significant at $\alpha$ level 0.05. 

Just like for statistical significance, rejecting the null-hypothesis at an alpha level of 0.05 is the equivalent for having a 95% confidence interval around the coefficient that does not include zero. In short

> The p-value represents the probability that the coefficient is actually zero. 

Let's import the code from the previous lesson and let's have a look at the p-value.


```python
import pandas as pd
import matplotlib.pyplot as plt
import statsmodels.api as sm
import statsmodels.formula.api as smf
import scipy.stats as stats
plt.style.use('fivethirtyeight')

data = pd.read_csv('Advertising.csv', index_col=0)
f = 'sales~TV'
f2 = 'sales~radio'
model = smf.ols(formula=f, data=data).fit()
```

Now, we can check for the p-value associated with our coefficient for TV:


```python
model.pvalues
```




    Intercept    1.406300e-35
    TV           1.467390e-42
    dtype: float64



You'll notice that you also get a p-value for the intercept. Except if the intercept is actually 0, you should get a pretty low p-value here anytime. The tiny p-value for TV indicates that our coefficient for TV is definitely significant, and we can reject the null-hypothesis. Next, there are two final things from the entire model summary we'd like to highlight:


```python
model.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>sales</td>      <th>  R-squared:         </th> <td>   0.612</td>
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th> <td>   0.610</td>
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th> <td>   312.1</td>
</tr>
<tr>
  <th>Date:</th>             <td>Fri, 08 Mar 2019</td> <th>  Prob (F-statistic):</th> <td>1.47e-42</td>
</tr>
<tr>
  <th>Time:</th>                 <td>15:13:22</td>     <th>  Log-Likelihood:    </th> <td> -519.05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td>   200</td>      <th>  AIC:               </th> <td>   1042.</td>
</tr>
<tr>
  <th>Df Residuals:</th>          <td>   198</td>      <th>  BIC:               </th> <td>   1049.</td>
</tr>
<tr>
  <th>Df Model:</th>              <td>     1</td>      <th>                     </th>     <td> </td>   
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>     <td> </td>   
</tr>
</table>
<table class="simpletable">
<tr>
      <td></td>         <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>Intercept</th> <td>    7.0326</td> <td>    0.458</td> <td>   15.360</td> <td> 0.000</td> <td>    6.130</td> <td>    7.935</td>
</tr>
<tr>
  <th>TV</th>        <td>    0.0475</td> <td>    0.003</td> <td>   17.668</td> <td> 0.000</td> <td>    0.042</td> <td>    0.053</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td> 0.531</td> <th>  Durbin-Watson:     </th> <td>   1.935</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th> <td> 0.767</td> <th>  Jarque-Bera (JB):  </th> <td>   0.669</td>
</tr>
<tr>
  <th>Skew:</th>          <td>-0.089</td> <th>  Prob(JB):          </th> <td>   0.716</td>
</tr>
<tr>
  <th>Kurtosis:</th>      <td> 2.779</td> <th>  Cond. No.          </th> <td>    338.</td>
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.



1. Have a look at the probability for the F-statistic in the upper right plane. This is a p-value for the **overall model fit**. You can see how this probability value is the exact same as the p-value for TV. You'll see that this is always the case if there is only one independent variable. Here, TV drives the entire model, so the model p-value is the same as the TV coefficient p-value.

2. In the plane with the coefficient estimates, p-values are in the column `P > |t|` and rounded to 3 digits (hence 0.000 for Intercept and TV). You can find the confidence intervals in the two last columns: The confidence interval for intercept is [6,13, 7.935] meaning that there is a 95% chance that the actual coefficient value is in that range. For TV, there is a 95% chance that the actual coefficient value is in the interval [0.042, 0.053]. Note that 0 is in none of these intervals as expected given the very low p-value.

Note that the alpha level of 0.05 is just a convention. You'll come across alpha levels of 0.1 and 0,001. Note that the confidence intervals change too as alpha levels change (to 90% and 99% confidence intervals, yet the standard output of statsmodels is a 95% confidence interval). 

Try running above and check for p-values for `radio` and see how would interpret the outcome. 

## Additional Resources

We encourage you to visit following links to see more examples and explanations around hypothesis testing and p values in regression!

[Hypothesis Test for Regression Slope](https://stattrek.com/regression/slope-test.aspx)

[Regression Continued](http://www.stat.ucla.edu/~cochran/stat10/winter/lectures/lect18.html)

## Summary 

In this lesson, you learned how to apply the ideas of hypothesis testing in regression analysis to associate significance and confidence level with your model. You used this with your previous regression model to check the association. In the next lab, you'll combine all the ideas around simple linear regression with OLS on a slightly more complex dataset with a more features. 
