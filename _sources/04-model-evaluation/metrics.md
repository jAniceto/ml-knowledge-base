# Performance metrics

Here the symbol $\hat{y}$ represents the predicted value, $y$ represents the actual value, and $n$ is the number of data points. The following are common regression performance metrics. 


## Mean Absolute Error (MAE)

$$\text{MAE} = \frac{1}{n} \sum_{i=1}^n \left| y_i - \hat{y}_i \right|$$

Advantages:

- Simple to interpret.

- Less affected by outliers (all individual errors are weighted equally in the average).


Disadvantages: 

- Since it uses the modulus, it does not take into account the direction of the error.


## Mean Squared Error (MSE)

$$\text{MSE} = \frac{1}{n} \sum_{i=1}^n \left( y_i - \hat{y}_i \right)^2$$

Advantages:

- Penalizes larger errors more heavily, making it more sensitive to outliers. Optimization algorithms benefit from this penalization, allowing to identify the global minima. 


Disadvantages: 

- Harder to interpret, since if is not in the same units of the original data.


## Root Mean Squared Error (RMSE)

$$\text{RMSE} = \sqrt{ \frac{1}{n} \sum_{i=1}^n \left( y_i - \hat{y}_i \right)^2 }$$

Advantages:

- Same units as the output. More straightforward to interpret.

- Sensitive to outliers.


Disadvantages: 

- Sensitive to outliers.


### Mean Absolute Error (MAE) *vs* Root Mean Squared Error (RMSE)

Which should you use to optimize a model? 

MAE less affected by outliers. RMSE more affected by outliers. So, if the cost associated with a large error is big, RMSE should be used. If the cost of the error does not increase significantly with the value of the error, then MAE can be the better option.


## R-squared (R2)

$$R^2 = 1 - \frac{SSE}{SST}$$

The closer to 1, the better.

![r-squared](../../_static/r-squared.png)

Advantages:

- Intuitive quantification of the quality of the fit.

- Don't need to compare with other models to see if it's good or bad. 


Disadvantages:

- It can be artificially close to 0 or close to 1, for good and bad models, respectively. For example, a nonlinear model can have high $R^2$ with linear regression. The same function with the same MSE error, but with a different X range, can result in lower $R^2$~


## References

- [Evaluation of empirical models for predicting monthly mean horizontal diffuse solar radiation](https://doi.org/10.1016/j.rser.2015.11.058)

- [Advantages of the mean absolute error (MAE) over the root mean square error (RMSE) in assessing average model performance ](http://dx.doi.org/10.3354/cr030079)

- [Lecture 10: F-Tests, R2, and Other Distractions](https://www.stat.cmu.edu/~cshalizi/mreg/15/lectures/10/lecture-10.pdf)