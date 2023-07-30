# Definitions

## Features

In machine learning, features are also referred to as variables, predictors, attributes, inputs, or independent variables. All these terms refer to the input variables or attributes that are used to train a machine learning model and make predictions. They are the measurable characteristics or properties of the data that can be used to predict the output or target variable. The choice of features is a critical step in the machine learning process, as the quality and relevance of the features can greatly impact the accuracy and performance of the model.


## Target

The target variable is the variable that the model is trying to predict. It is also known as the dependent variable, response variable, outcome variable, output variable or label. This later term ("label") is often used in classification tasks, where the target variable is a categorical variable that is used to label or classify instances of data. The target variable is the variable that is affected by changes in the input variables, or features. 

For example, in a model that predicts house prices, the target variable would be the house price, and the features would be various attributes of the house such as its size, number of bedrooms, location, etc. The model would take in these features as inputs and output a predicted house price.


## Hyperparameters

Hyperparameters are parameters that are not learned directly from the training data, but must be set by the user before the learning process begins. These parameters define the characteristics of a machine learning model and affect its learning process.

Hyperparameters are usually set through trial and error, intuition, or by using techniques like grid search or random search. The optimal values of hyperparameters may vary depending on the specific problem, dataset, and model architecture. Finding the right hyperparameters often requires experimentation and fine-tuning to achieve the best performance.

## Dense *vs* sparse matrices

The terms "dense matrix" and "sparse matrix" are used to describe matrices based on the distribution of non-zero elements within the matrix.

A dense matrix is a matrix in which a significant portion of its elements are non-zero. In other words, it has a relatively high density of non-zero entries. 

A sparse matrix is a matrix in which the majority of its elements are zero. Sparse matrices arise in various applications, such as graph representation, scientific computing, and large-scale data analysis, where the data is often sparse. Storing and manipulating sparse matrices efficiently can save memory and computational resources.

The main difference between dense and sparse matrices lies in the way they are stored and the computational algorithms used to operate on them. Dense matrices are typically stored as a contiguous block of memory, allowing for efficient random access to elements. On the other hand, sparse matrices employ specialized data structures (e.g., compressed sparse row or column formats) to store only the non-zero elements and their corresponding indices, which reduces memory requirements and enables efficient operations on sparse data.

When choosing between dense and sparse matrices, it depends on the characteristics of the problem and the nature of the data. Dense matrices are suitable when the matrix is small or moderately sized, and the majority of elements are non-zero. Sparse matrices are useful when dealing with large-scale problems or datasets where most of the elements are zero, and computational and memory efficiency are crucial considerations.