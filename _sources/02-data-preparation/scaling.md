# Scaling

With few exceptions, Machine Learning algorithms don't perform well when the input numerical attributes have very different scales. Note that scaling the target values is generally not required.

As with all the transformations, it is important to **fit the scalers to the training data only**, not to the full dataset (including the test set). Only then can you use them to transform the training set and the test set (and new data).

There are two common ways to get all attributes to have the same scale: min-max scaling and standardization.


## Is feature scaling always necessary?

No, scaling is not necessary for Random Forests and Decision Trees. The nature of Random Forests and Decision Trees is such that convergence and numerical precision issues, which can sometimes trip up the algorithms used in logistic and linear regression, as well as neural networks, aren't so important. Because of this, you don't need to transform variables to a common scale like you might with a neural network.

Note that, for classification tasks, the output of the Random Forest is the class selected by most trees. For regression tasks, the mean or average prediction of the individual trees is returned. Therefore, data normalization won't affect the output for Random Forest classifiers while it will affect the output for Random Forest regressors.


## Min-max scaling

Min-max scaling (also called normalization) is quite simple: values are shifted and rescaled so that they end up in the range $[0, 1]$ (or $[-1, 1]$).

$$
x_\text{scaled} = \frac{x - x_\text{min}}{x_\text{max} - x_\text{min}}
$$

where $x$ is the original value, $x_\text{min}$ is the minimum value of the feature, $x_\text{max}$ is the maximum value of the feature, and $x_\text{scaled}$ is the scaled value.

Advantages:
- Ensures that all features have same scale.
- It is simple and easy to interpret.

Disadvantages:
- It is sensitive to outliers and the extreme values can distort scaling may cause makes most data points cluster near 0 or 1.
- Fixed range limits flexibility for datasets with changing scales.

Typical workflow using `scikit-learn`:

```python
# Create the scaler object with a range of 0-1
scaler = MinMaxScaler(feature_range=(0, 1))

# Fit on the training data
scaler.fit(x_train)

# Transform both the training and testing data
x_train = scaler.transform(x_train)
x_test = scaler.transform(x_test)
```


## Standardization

Standardization follows Standard Normal Distribution. It transforms data so that the mean becomes 0 and the standard deviation becomes 1. This centers the data around zero and standardizes variability. It's ideal for algorithms like SVM, logistic regression or neural networks that assume data is normally distributed.

$$
x_\text{scaled} = \frac{x - \mu}{\sigma}
$$

where $\mu$ is the mean value of the feature and $\sigma$ is the standard deviation.

Advantages:
- Handles features with different units effectively.
- Reduces impact of outliers without completely removing them.

Disadvantages:
- It is sensitive to outliers and extreme values can skew mean and standard deviation which leads to poor scaling.
- It is not ideal for non-normal distributions.


## Robust scaling

Robust scaling reduces the impact of outliers by scaling data using median and interquartile range (IQR) which makes it fit to extreme values. It is useful when data contains many outliers and we need to maintain relative distances between non-outlier data points or when working with algorithms which are sensitive to extreme values.

$$
x_\text{scaled} = \frac{x - \text{Median}(x)}{Q_3 - Q_1}
$$

where $\text{Median}(x)$ is the $Q_2$ ​or the median quartile (50th percentile), $Q_1$​ is the first quartile (25th percentile), and $Q_3$ is the third quartile (75th percentile).

Advantages:
- Resistant to outliers.
- Maintains the structure of data and it is better than MinMaxScaler in the presence of extreme values.

Disadvantages:
- May not perform well when data is highly skewed.
- Less interpretable compared to MinMaxScaler.



## Scaling data before or after train/test split

In the interest of preventing information about the distribution of the test set leaking into your model, **you should fit the scaler on your training data only**, then standardise both training and test sets with that scaler. By fitting the scaler on the full dataset prior to splitting, information about the test set is used to transform the training set, which in turn is passed downstream.

As an example, knowing the distribution of the whole dataset might influence how you detect and process outliers, as well as how you parameterise your model. Although the data itself is not exposed, information about the distribution of the data is. As a result, your test set performance is not a true estimate of performance on unseen data.

You first need to split the data into training and test set (validation set might also be required).

Don't forget that testing data points represent real-world data. Feature normalization (or data standardization) of the explanatory (or predictor) variables is a technique used to center and normalise the data by subtracting the mean and dividing by the variance. If you take the mean and variance of the whole dataset you'll be introducing future information into the training explanatory variables (i.e. the mean and variance).

Therefore, you should perform feature normalisation over the training data. Then perform normalisation on testing instances as well, but this time using the mean and variance of training explanatory variables. In this way, we can test and evaluate whether our model can generalize well to new, unseen data points.

