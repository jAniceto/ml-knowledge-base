
# Parametric tests or models 

In statistics and machine learning, the terms "parametric" and "non-parametric" are used to describe different types of models or approaches based on the assumptions they make about the underlying data distributions or the structure of the model itself. 


## Parametric models

Parametric models assume that the data follows a specific probability distribution, which can be fully described by a finite set of parameters. The form of the function that relates the input variables to the output variable is specified in advance, except for some unknown parameters that the model will try to learn from the data.

Characteristics:

- Fixed number of parameters: Regardless of the dataset size, the number of parameters remains constant. This makes the models generally more straightforward and computationally less intensive.

- Assumptions: These models make strong assumptions about the form of the data distribution (e.g., assuming data is normally distributed). If the assumptions are correct, parametric models can be very efficient and effective.

- Examples: Linear regression, logistic regression, and Gaussian Naive Bayes.


## Non-parametric models

Non-parametric models do not assume that the data fits any specific distribution and are not defined by a fixed set of parameters. The number of parameters can grow as the size of the data increases. These models are more flexible in fitting the data but can require more data to produce accurate models and can be computationally intensive.

Characteristics:

- Flexibility: They can adapt to any form of the data distribution, making them suitable for complex datasets where the underlying distribution is unknown or difficult to specify.

- Data-driven: The model structure is determined from the data. As more data becomes available, the model can capture more complexities.

- Potential for overfitting: Due to their flexibility and complexity, non-parametric models are more prone to overfitting, especially when the dataset is small or noisy.

- Examples: K-nearest neighbors (KNN), decision trees, and kernel density estimation.


## Choosing Between Parametric and Non-Parametric Models

When to use parametric models: If you have prior knowledge that suggests the data follows a certain distribution, or you're working with a small dataset, or computational efficiency is a priority, a parametric approach might be preferable.

When to use non-parametric models: If the data's distribution is unknown or complex, if you have a large amount of data that can support the model's complexity, or if you want to avoid making strong assumptions about the form of the data distribution, a non-parametric approach might be more appropriate.

In practice, both types of models have their advantages and disadvantages, and the choice often involves a trade-off between simplicity and flexibility, interpretability and accuracy, or theoretical assumptions and empirical performance.