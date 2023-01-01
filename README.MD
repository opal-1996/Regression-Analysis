# Linear & Nonlinear Regression

Regression analysis is a set of statistical methods used for the estimation of relationships between a dependent variable and one or more independent variables. It can be utilized to assess the strength of the relationship between variables and for modeling the future relationship between them.

Regression analysis includes several variants, such as `Ordinary Least Square (OLS)/Simple Linear Regression`, `Multiple Regression`, `Generalized Linear Regression`, and `Nonlinear Regression`. Depend on the dataset we have and the problems we need to solve, we use specific regression methods under different kinds of scenarios. 

## Linear Regression
### Simple Linear Regression / OLS

Simple linear regression is a model that assesses the relationship between one dependent variable and one independent variable. The model is expressed using the following equation:

$$
\begin{aligned}
y = a + bx +\epsilon
\end{aligned}
$$

Where:

* y - dependent variable
* x - independent variable
* a - intercept
* b - slope
* $\epsilon$ - residual

Key assumptions:

* The linear regression model is "linear in parameters"
* Random sampling of observations
* $E(\epsilon | X) = 0$ 
* Homoscedasticity ($Var(\epsilon|X) = \sigma^2$) and no autocorrelation ($Cov(\epsilon_i\epsilon_j|X) = 0$ for $i \neq j$)
* Error terms are normally distributed

Loss function:

$$
\begin{aligned}
L = \sum_{i=1}^{N}(y_i - \hat{y_i})^2 = \sum_{i=1}^{N}(y_i - (a + bx_i + \epsilon))^2
\end{aligned}
$$

Interpret of OLS results:

In python, we can use [`statsmodel`](https://www.statsmodels.org/stable/generated/statsmodels.regression.linear_model.OLS.html#statsmodels.regression.linear_model.OLS) package for OLS modeling. It takes care of the optimization of loss function and all the math behind the algorithm, all we need to feed is input features and target feature. The OLS function would fit a model and estimate the parameters. A typical OLS results table looks like this:

![image](../Regression-Analysis/Linear%20Regression%20-%20Capital%20Asset%20Pricing%20Model/images/OLS.png)
[*Image source.*](https://www.geeksforgeeks.org/interpreting-the-results-of-linear-regression-using-ols-summary/)

* Constant term($\hat{a}$): the intercept of the regression line, tells the average value of all omitted variables and noise present in the model. From the figure above, the intercept is -3.2002. 
* Coefficient term($\hat{b}$): tells the change in y for a unit change in x. For example, if x rises by 1 unit, then y rises by 0.7529 units. 
* Standard error of parameters(std err): shows the sampling variability of these parameters.
* t-statistics: we assume that error term follows the normal distribution and because of this the parameters also have normal distributions with variance calculated in column `std err`. 
* t-statistics are calculated by assuming following hypothesis:

$$
\begin{aligned}
H_0: b = 0 \\
H_a: b \neq 0 \\
t = \frac{\hat{b} - 0}{std(\hat{b})}
\end{aligned}
$$

* p-values: the probability of obtaining the t statistics at least as contradictory to $H_0$ as calculated from assuming that the null hypothesis is true. From the results above, we can see that p-value for both parameters is 0, so we can reject the null hypothesis at almost every significance level.
* R-squared value: $R^2$ is the coefficient of determination that tells us how much percentage variation of independent variable can be explained by independent variable x. Here, 66.9% variation in y can be explained by $x_1$. The maximum value of $R^2$ is 1.
* F-statistics: tells the goodness of fit of a regression. 

$$
\begin{aligned}
F = \frac{R^2/(k-1)}{(1-R^2)/(n-k)}
\end{aligned}
$$

* AIC & BIC are statistical tools used to estimate the best model from a set of models.

Pros of OLS:

* Simple to implement
* Less complexity
* May lead to overfitting but can be avoided using dimensionality reduction, regularization, and cross-validation techniques

Cons of OLS:

* Outliers affect the algorithm badly
* Over-simplifies real-world problems
  
Business applications:

* [Beta](https://corporatefinanceinstitute.com/resources/valuation/what-is-beta-guide/) Calculation (volatility of returns relative to the overall market) for a stock
* Revenue forecasting based on the number of running ads

### Multiple Linear Regression

Multiple linear regression analysis is similar to the OLS, with the exception taht multiple variables are used in the model. The mathematical representation of multiple linear regression is:

$$
\begin{aligned}
y = a + b_1x_1 + b_2x_2 + b_3x_3 + ... +  b_nx_n + \epsilon
\end{aligned}
$$

Where:

* y - dependent variable
* $x_i$ - independent variables
* a - intercept
* $b_i$ - slopes
* $\epsilon$ - residual

Multiple linear regression follows the same conditions as the simple linear model. However, since there are several independent variables here, we have another mandatory condition for this model:

* Non-collinearity: independent variables are uncorrelated

Polynomial Regression:

OLS only works when the relationship been the data is linear.
When the relationship between data is non-linear, Polynomial regression comes to the rescue. 

Polynomial regression is a form of linear regression where due to the non-linear relationship between dependent and independent variables we add some polynomial terms to linear regression to convert it into Polynomial regression. The equation looks like this:

$$
\begin{aligned}
y = a + b_1x_1 + b_2x_1^2 + b_3x_1^3 + b_nx_1^n + \epsilon
\end{aligned}
$$

The degree of order which to use is a hyperparameter, and we should choose it wisely. Using a high degree of polynomial tends to overfit the data and for smaller values of degree, the model tends to underfit.

Caveat: collinearity issue

### Generalized Linear Regression

Generalized linear regression is an advanced statistical modeling technique formulated by John Nelder and Robert Wedderburn in 1972. It is an umbrella term that encompasses many other models, which allows the response variable y to have an error distribution other than a normal distribution. The models include linear regression, logistic regression, and poission regression.

Three components of any GLMs:

* Random Component - specifies the probability distribution of the target variable. E.g., normal distribution for y in OLS, or binomial distribution for y in the binary logistic regression model.
* Systematic Component - specifies the explanatory variables ($x_i$) in the model, more specifically, their linear combination. E.g., $\beta_0$ + $\beta_1x_1$ + $\beta_2x_2$.
* Link Function $g(u)$ - specifies the link between the random and systematic components. It indicates how the expected value of the target relates to the linear combination of explanatory variables. E.g., $log(\frac{p}{1-p}) = \beta_0 + \beta_1x_1 + \beta_2x_2 + ... + \beta_nx_n$ for logistic regression.

Assumptions:

* $y_i$ are independently distributed
* Target variable does not have to be normally distributed, but it typically assumes a distribution from an exponential family (e.g., binomial, poission, multinomial, normal)
* GLMs don't assume linear relationship between the target variable and the explanatory variables, but they do assume a linear relationship between the transformed expected target in terms of the link function and the explanatory variables
* Explanatory variables can be nonlinear transformations of some original variables
* Errors need to be independent but not normally distributed
* Parameters estimation uses maximum likelihood estimation (MLE) rather than OLS.

Why GLMs?

* The relationship between x and y is not linear
* Variance of errors in y (commonly called as homoscedasticity) is not constant and varies with x
* Target variables are not continuous but categorical / discrete

Example: Logistic Regression

Logistic regression, also known as logit model, is used for classification and predictive analysis. It estimates the probability of an event occuring, such as voted or didn't vote, based on a given dataset of independent variables. 

In logistic regression, a logit tranformation is applied on the odds. The logistic function is represented by the following formulas:

$$
\begin{aligned}
ln(\frac{p_i}{1-p_i}) = \beta_0 + \beta_1x_1 + \beta_2x_2 + \beta_3x_3 + ... + \beta_nx_n
\end{aligned}
$$

Where:

* $x_i$ - independent variables
* $\beta_i$ - parameters
* $p_i$ - $P(y_i = 1)$

Loss function: cross-entropy / log loss

$$
\begin{aligned}
L = -\frac{1}{N}\sum_{i=1}^{N}y_i log(p(y_i)) + (1-y_i)log(1-p(y_i))
\end{aligned}
$$

Real-world applications:

* Credit scoring
* Spam detection
* Fraud detection

###  Ridge Regression & Lasso Regression

When it comes to training models, there are two major problems one can encounter: `overfitting` and `underfitting`. 

For overfitting problems, regularization technique is applied. For example, `ridge regression` and `lasso regression`.

Lasso Regression / L1 Regression:

Lasso is short for Least Absolute Shrinkage and Selection Operator. It adds penalty term to the loss function. This term is the absolute sum of the coefficients. As the values of coefficients go away from 0, the loss increases and thus pushes the values of coefficients back to 0. 

$$
\begin{aligned}
L_{lasso} = ||Y - \beta X||^2 + \lambda ||\beta||
\end{aligned}
$$

Limitations of lasso regression:

* Lasso sometimes struggles with some types of data. If the number of predictors is greater than the number of observations, lasso will pick as most n predictors as non-zero, even if all predictors are relevant.
* If there are two or more highly collinear variables then lasso regression select one of them randomly which is not good for the interpretation.

Ridge Regression / L2 Regression:

In ridge regression, we add a penalty term which is equal to the square of the magnitude of the coefficient. Parameter $\lambda$ is given to control the balance between variance and bias.

$$
\begin{aligned}
L_{ridge} = ||Y - \beta X||^2 + \lambda ||\beta||^2
\end{aligned}
$$

Limitation of ridge regression:

* Ridge regression decreases the complexity of a model but does not reduce the number of variables since it never leads a coefficient to 0 (only minimizes it). Hence it is not good for feature reduction.

###  Nonlinear Regression

Nonlinear regression is a mathematical model that fits an equation to certain data using a generated line. As is the case with a simple linear regression that uses a straight-line equation, nonlinear regression shows association using a curve, making it nonlinear in the parameters.

A simple nonlinear regression model is expressed as follows:

$$
\begin{aligned}
y = f(X, \beta) + \epsilon
\end{aligned}
$$

Where:

* X - a vector of predictors
* $\beta$ - a vector of parameters
* f - the known regression function
* $\epsilon$ - error term

Examples:

* Fourier: $y = \theta_1 * cos (X + \theta_4) + \theta_2 * cos(2*X + \theta_4) + \theta_3$
