# Model evaluation

- The **bias** of an estimator is its average error for different training sets. 
- The **variance** of an estimator indicates how sensitive it is to varying training sets. 
- **Noise** is a property of the data.

Bias and variance are inherent properties of estimators and we usually have to select learning algorithms and hyperparameters so that both bias and variance are as low as possible (see [Bias-variance dilemma](https://en.wikipedia.org/wiki/Bias%E2%80%93variance_tradeoff)). 

Another way to reduce the variance of a model is to use more training data. However, you should only collect more training data if the true function is too complex to be approximated by an estimator with a lower variance.

## Model metrics

Three statistics are used in Ordinary Least Squares (OLS) regression to evaluate model fit: R-squared, the overall F-test, and the Root Mean Square Error (RMSE). All three are based on two sums of squares: Sum of Squares Total (SST) and Sum of Squares Error (SSE). SST measures how far the data are from the mean, and SSE measures how far the data are from the model’s predicted values. Different combinations of these two values provide different information about how the regression model compares to the mean model.


### R-squared and Adjusted R-squared

The difference between SST and SSE is the improvement in prediction from the regression model, compared to the mean model. Dividing that difference by SST gives R-squared. It is the proportional improvement in prediction from the regression model, compared to the mean model. It indicates the goodness of fit of the model.

R-squared has the useful property that its scale is intuitive: it ranges from zero to one, with zero indicating that the proposed model does not improve prediction over the mean model, and one indicating perfect prediction. Improvement in the regression model results in proportional increases in R-squared.

One pitfall of R-squared is that it can only increase as predictors are added to the regression model. This increase is artificial when predictors are not actually improving the model’s fit. To remedy this, a related statistic, Adjusted R-squared, incorporates the model’s degrees of freedom. Adjusted R-squared will decrease as predictors are added if the increase in model fit does not make up for the loss of degrees of freedom. Likewise, it will increase as predictors are added if the increase in model fit is worthwhile. Adjusted R-squared should always be used with models with more than one predictor variable. It is interpreted as the proportion of total variance that is explained by the model.

There are situations in which a high R-squared is not necessary or relevant. When the interest is in the relationship between variables, not in prediction, the R-square is less important. An example is a study on how religiosity affects health outcomes. A good result is a reliable relationship between religiosity and health. No one would expect that religion explains a high percentage of the variation in health, as health is affected by many other factors. Even if the model accounts for other variables known to affect health, such as income and age, an R-squared in the range of 0.10 to 0.15 is reasonable.

### The F-test

The F-test evaluates the null hypothesis that all regression coefficients are equal to zero versus the alternative that at least one is not. An equivalent null hypothesis is that R-squared equals zero. A significant F-test indicates that the observed R-squared is reliable and is not a spurious result of oddities in the data set. Thus the F-test determines whether the proposed relationship between the response variable and the set of predictors is statistically reliable and can be useful when the research objective is either prediction or explanation.

### RMSE

The RMSE is the square root of the variance of the residuals. It indicates the absolute fit of the model to the data–how close the observed data points are to the model’s predicted values. Whereas R-squared is a relative measure of fit, RMSE is an absolute measure of fit. As the square root of a variance, RMSE can be interpreted as the standard deviation of the unexplained variance, and has the useful property of being in the same units as the response variable. Lower values of RMSE indicate better fit. RMSE is a good measure of how accurately the model predicts the response, and it is the most important criterion for fit if the main purpose of the model is prediction.

### Q-squared

Q-squared (Q2) is the R-squared value that you get from applying the QSAR model to the test set instead of the training set. Since the model is not directly calibrated to fit the test set, Q-squared may or may not increase as you add more PLS factors. But if it's a good model (i.e., it embodies the SAR and you've picked a reasonable number of PLS factors), Q-squared will be comparable in value to R-squared.

## sklearn metrics for regression problems

| Scoring                       | Function                          |
|-----------------------------	|----------------------------------	|
| explained_variance          	| metrics.explained_variance_score 	|
| max_error                   	| metrics.max_error                	|
| neg_mean_absolute_error     	| metrics.mean_absolute_error      	|
| neg_mean_squared_error      	| metrics.mean_squared_error       	|
| neg_root_mean_squared_error 	| metrics.mean_squared_error       	|
| neg_mean_squared_log_error  	| metrics.mean_squared_log_error   	|
| neg_median_absolute_error   	| metrics.median_absolute_error    	|
| r2                          	| metrics.r2_score                 	|
| neg_mean_poisson_deviance   	| metrics.mean_poisson_deviance    	|
| neg_mean_gamma_deviance     	| metrics.mean_gamma_deviance      	|


## References

- [Cross-validation: evaluating estimator performance](https://scikit-learn.org/stable/modules/cross_validation.html)
- [Regression metrics](https://scikit-learn.org/stable/modules/model_evaluation.html#regression-metrics)
- [Tips for parameter search](https://scikit-learn.org/stable/modules/grid_search.html#tips-for-parameter-search)
- [Article - Learning Curves Explained](https://vitalflux.com/learning-curves-explained-python-sklearn-example/)
- [Article - Validation Curve Explained - Plot the influence of a single hyperparameter](https://towardsdatascience.com/validation-curve-explained-plot-the-influence-of-a-single-hyperparameter-1ac4864deaf8)

