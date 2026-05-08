# Gradient Boosting

Boosting algorithms play a crucial role in dealing with bias variance trade-off. Unlike bagging algorithms, which only controls for high variance in a model, boosting controls both the aspects (bias and variance), and is considered to be more effective.

Boosting is a sequential technique which works on the principle of ensemble. It combines a set of weak learners and delivers improved prediction accuracy. At any instant $t$, the model outcomes are weighed based on the outcomes of previous instant $t-1$. The outcomes predicted correctly are given a lower weight and the ones miss-classified are weighted higher. This technique is followed for a classification problem while a similar technique is used for regression.

## Function to train a model with GB and hyperparameter search

The following function does the following:

1) Defines a list of hyperparameters to try, i.e. defines an hyperparameter grid.

2) Runs a grid search or a random search depending on the given input.

3) Return an object representing the best model found.


```python
def train_gradient_boosting(x_train, y_train, search_type):
    """Train Gradient Boosting model with hyperparameter tuning.
    
    ARGUMENTS:
        x_train : train set variables
        y_train : train set targets
        search_type : random or grid
        
    RETURNS:
        best model    
    """
    # Loss function to be optimized
    loss = ['squared_error', 'absolute_error', 'huber']
    
    # Learning rate
    learning_rate = [0.1, 0.05, 0.01, 0.5]

    # Number of trees used in the boosting process
    n_estimators = [100, 50, 200, 500, 1000]

    # The fraction of samples to be used for fitting the individual base learners
    subsample = [1.0, 0.8]

    # Maximum depth of each tree
    max_depth = [3, 2, 5, 10] 

    # Minimum number of samples per leaf
    min_samples_leaf = [1, 2, 4, 8]

    # Minimum number of samples to split a node
    min_samples_split = [2, 4, 10, 20, 50]  

    # Maximum number of features to consider for making splits
    max_features = [None, 0.2, 0.3, 0.4, 'sqrt', 'log2']
    # max_features = ['auto', 8, 10, 12, 14, 16]

    # Define the grid of hyperparameters to search
    hyperparameter_grid = {'loss': loss,
                           'learning_rate': learning_rate,
                           'subsample': subsample,
                           'n_estimators': n_estimators,
                           'max_depth': max_depth,
                           'min_samples_leaf': min_samples_leaf,
                           'min_samples_split': min_samples_split,
                           'max_features': max_features}

    # Create the model to use for hyperparameter tuning
    model = GradientBoostingRegressor(random_state=42)

    if search_type == 'random':
        # Set up the random search with 4-fold cross validation
        search_cv = RandomizedSearchCV(estimator=model,
                                       param_distributions=hyperparameter_grid,
                                       cv=4, 
                                       n_iter=100, # 2000
                                       scoring='neg_mean_absolute_error',
                                       n_jobs=-1, 
                                       verbose=2, 
                                       return_train_score=True,
                                       random_state=42)
    
    elif search_type == 'grid':
        # Set up the grid search with 4-fold cross validation
        search_cv = GridSearchCV(estimator=model,
                                 param_grid=hyperparameter_grid,
                                 cv=4, 
                                 scoring='neg_mean_absolute_error',
                                 n_jobs=-1, 
                                 verbose=2, 
                                 return_train_score=True)
    else: 
        print('search_type must be random OR grid')
        return
    
    # Fit to the training data
    search_cv.fit(x_train, y_train)
    
    # Print best parameters
    print('Best parameters:')
    print(search_cv.best_params_)

    return search_cv.best_estimator_
```