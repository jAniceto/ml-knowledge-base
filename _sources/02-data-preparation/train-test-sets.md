# Train, validation, and test sets

Data should be divided into three data sets:

- training, 

- validation, and

- testing.


## Training set

The training set is used to fit a certain algorithm to find the model parameters, which are internal values that allow a model to make predictions. 

## Validation set

The validation set is used to evaluate the choice of the algorithm and respective hyperparameters. The hyperparameters are external values to the model, related to the training process, which can be chosen and tuned. 

In order to reduce the bias to the validation set, *n*-fold cross-validation should be employed, withnrecommended to be 5 or 10. In ten-fold cross-validation, for example, a model is evaluated 10-times against ten independent samples corresponding to 10 % of the dataset after it is trained 10 different times with the remaining 90 % of the validation data.

## Testing set

The test set should be held out from the feature selection, training, optimization, and validation stages, being stored for a final independent test of the most accurate model according to cross-validation. The amount of data reserved for the test set is usually between 10 % and 30 % of the whole dataset, depending on the amount of data available and the nature of the problem.

If the performance of the model toward the test set is not satisfactory and the model needs to be further refined.


## Conducting the split in `sklearn`

The following code can be used to split a dataframe containg all data into training and testing sets. In this example, the column `D12` represents the target variable.

```python
# Separate the variables and targets
x = df.drop(columns=['D12'])
y = pd.DataFrame(df['D12'])

# Split into training and testing set
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=TEST_FRACTION, random_state=0)

print('Training set x:', x_train.shape)
print('Testing set x:', x_test.shape)
print('Training set y:', y_train.shape)
print('Testing set y:', y_test.shape)


# Training set x: (873, 167)
# Testing set x: (292, 167)
# Training set y: (873, 1)
# Testing set y: (292, 1)
```