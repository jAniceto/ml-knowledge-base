# Scaling

With few exceptions, Machine Learning algorithms don't perform well when the input numerical attributes have very different scales. Note that scaling the target values is generally not required.

As with all the transformations, it is important to **fit the scalers to the training data only**, not to the full dataset (including the test set). Only then can you use them to transform the training set and the test set (and new data).

There are two common ways to get all attributes to have the same scale: min-max scaling and standardization.


## Is feature scaling always necessary?

No, scaling is not necessary for Random Forests and Decision Trees. The nature of Random Forests and Decision Trees is such that convergence and numerical precision issues, which can sometimes trip up the algorithms used in logistic and linear regression, as well as neural networks, aren't so important. Because of this, you don't need to transform variables to a common scale like you might with a neural network.

Note that, for classification tasks, the output of the Random Forest is the class selected by most trees. For regression tasks, the mean or average prediction of the individual trees is returned. Therefore, data normalization won't affect the output for Random Forest classifiers while it will affect the output for Random Forest regressors.


## Normalization

Min-max scaling (many people call this normalization) is quite simple: values are shifted and rescaled so that they end up ranging from 0 to 1.


## Standardization

Standardization first it subtracts the mean value (so standardized values always have a zero mean), and then it divides by the standard deviation so that the resulting distribution has unit variance.

```python
# Create the scaler object with a range of 0-1
scaler = MinMaxScaler(feature_range=(0, 1))

# Fit on the training data
scaler.fit(x_train)

# Transform both the training and testing data
x_train = scaler.transform(x_train)
x_test = scaler.transform(x_test)
```

## Scaling data before or after train/test split

In the interest of preventing information about the distribution of the test set leaking into your model, **you should fit the scaler on your training data only**, then standardise both training and test sets with that scaler. By fitting the scaler on the full dataset prior to splitting, information about the test set is used to transform the training set, which in turn is passed downstream.

As an example, knowing the distribution of the whole dataset might influence how you detect and process outliers, as well as how you parameterise your model. Although the data itself is not exposed, information about the distribution of the data is. As a result, your test set performance is not a true estimate of performance on unseen data.

You first need to split the data into training and test set (validation set might also be required).

Don't forget that testing data points represent real-world data. Feature normalization (or data standardization) of the explanatory (or predictor) variables is a technique used to center and normalise the data by subtracting the mean and dividing by the variance. If you take the mean and variance of the whole dataset you'll be introducing future information into the training explanatory variables (i.e. the mean and variance).

Therefore, you should perform feature normalisation over the training data. Then perform normalisation on testing instances as well, but this time using the mean and variance of training explanatory variables. In this way, we can test and evaluate whether our model can generalize well to new, unseen data points.

