# Hyperparameters - Gradient Boosting

Gradient Boosting models have three types of hyperparameters :

1.  Tree-specific parameters which affect each individual tree in the
    model;

2.  Boosting parameters which affect the boosting operation in the
    model;

3.  Other parameters for overall functioning.

## Tree-specific hyperparameters

### Minimum samples for splitting

-   In scikit-learn: `min_samples_split`.

-   Defines the minimum number of samples (or observations) which are
    required in a node to be considered for splitting.

-   Used to control over-fitting. Higher values prevent a model from
    learning relations which might be highly specific to the particular
    sample selected for a tree.

-   Too high values can lead to under-fitting hence, it should be tuned
    using CV.

### Minimum samples per leaf

-   In scikit-learn: `min_samples_leaf`.

-   Defines the minimum samples (or observations) required in a terminal
    node or leaf.

-   Used to control over-fitting similar to `min_samples_split`.

-   Generally lower values should be chosen for imbalanced class
    problems because the regions in which the minority class will be in
    majority will be very small.

### Minimum samples per leaf as a fraction of total samples

-   In scikit-learn: `min_weight_fraction_leaf`.

-   Similar to `min_samples_leaf` but defined as a fraction of the total
    number of observations instead of an integer.

-   Only one of `min_samples_leaf` or `min_weight_fraction_leaf` should
    be defined.

### Maximum depth of a tree

-   In scikit-learn: `max_depth`.

-   The maximum depth of a tree.

-   Used to control over-fitting as higher depth will allow model to
    learn relations very specific to a particular sample.

-   Should be tuned using CV.

### Maximum number of leaves in a tree

-   In scikit-learn: `max_leaf_nodes`.

-   The maximum number of terminal nodes or leaves in a tree.

-   Can be defined in place of `max_depth`. Since binary trees are
    created, a depth of *n* would produce a maximum of 2<sup>*n*</sup>
    leaves.

-   If this is defined, the model will ignore `max_depth`.

### Maximum number of features

-   In scikit-learn: `max_features`.

-   The number of features to consider while searching for a best split.
    These will be randomly selected.

-   As a thumb-rule, square root of the total number of features works
    great but we should check upto 30-40 % of the total number of
    features.

-   Higher values can lead to over-fitting but depends on case to case.

## Boosting hyperparameters

### Learning rate

-   In scikit-learn: `learning_rate`.

-   This determines the impact of each tree on the final outcome (step
    2.4). GBM works by starting with an initial estimate which is
    updated using the output of each tree. The learning parameter
    controls the magnitude of this change in the estimates.

-   Lower values are generally preferred as they make the model robust
    to the specific characteristics of tree and thus allowing it to
    generalize well.

-   Lower values would require higher number of trees to model all the
    relations and will be computationally expensive.

### Number of sequential trees

-   In scikit-learn: `n_estimators`.

-   The number of sequential trees to be modeled.

-   Though GB is fairly robust at higher number of trees but it can
    still overfit at a point. Hence, this should be tuned using CV for a
    particular learning rate.

### Fraction of observations to be selected for each tree

-   In scikit-learn: `subsample`.

-   The fraction of observations to be selected for each tree. Selection
    is done by random sampling.

-   Values slightly less than 1 make the model robust by reducing the
    variance.

-   Typical values  0.8 generally work fine but can be fine-tuned
    further.

## Other hyperparameters

### Loss function

-   In scikit-learn: `loss`.

-   It refers to the loss function to be minimized in each split.

-   It can have various values for classification and regression case.
    Generally the default values work fine. Other values should be
    chosen only if you understand their impact on the model.