# SHAP (SHapley Additive exPlanations)

SHAP (SHapley Additive exPlanations) is a method to explain the output of any machine learning model in a way that is understandable to humans. It is based on [Shapley values](https://en.wikipedia.org/wiki/Shapley_value), a concept from cooperative game theory that allocates payouts to players depending on their contribution to the total payout. In the context of machine learning, **SHAP values measure the impact of each feature on the prediction of a model**, considering the interaction with other features.

SHAP values have several desirable properties:

- Local Accuracy: The explanation for a single prediction (local explanation) faithfully represents the model's output for that specific input;

- Missingness: If a feature's value is missing for a particular prediction, its attributed SHAP value is zero, reflecting no impact on the model's output due to the missing feature;

- Consistency: If changing a model to rely more on a particular feature increases the prediction's accuracy, the SHAP value for that feature should not decrease;

- Additivity: The sum of the SHAP values for all features plus a base value (the model's output for the average input) equals the prediction for a specific input instance.

## How SHAP works

SHAP explains the prediction of an instance by computing the contribution of each feature to the prediction. It does this by comparing what the model predicts with and without the feature. However, since features can interact in complex ways, SHAP considers all possible combinations of features to accurately measure each feature's contribution. This process involves retraining the model on all subsets of features, which can be computationally expensive, especially for models with a large number of features. To address this, various approximations and optimizations have been developed, such as TreeSHAP for tree-based models. 

## Applications of SHAP

- Model interpretation: SHAP helps in understanding which features are most important for a model's predictions. This can aid in model debugging, validation, and improving model transparency;
    
- Feature selection: By identifying the most influential features, SHAP can guide feature selection, potentially improving model simplicity and performance;
    
- Fairness and bias detection: SHAP values can help detect bias by revealing whether and how certain sensitive features influence model predictions.

## Implementation

SHAP is implemented in Python with the [`shap` package](https://shap.readthedocs.io/en/latest/index.html).

Below is an example of how to use SHAP with a GradientBoostingRegressor model from scikit-learn. Here we calculate SHAP values for the test set, and visualize the contributions of each feature to the model's predictions.

```python
import shap
from sklearn.ensemble import GradientBoostingRegressor

# x_train, y_train are the features (X) and target variable (Y) in the dataset
model = GradientBoostingRegressor(random_state=0)
model.fit(x_train, y_train)

# Create a SHAP TreeExplainer for the gradient boosting model
explainer = shap.TreeExplainer(model)

# Calculate SHAP values for the test set
shap_values = explainer.shap_values(x_test)

shap.summary_plot(shap_values, x_test, feature_names=boston.feature_names)

# Visualize the SHAP values for the first prediction
shap.initjs()  # Initialize JavaScript visualization in Jupyter notebooks or similar environments
shap.force_plot(explainer.expected_value[1], shap_values[1][0], x_test[0])
```


## References

- [SHAP package documentation](https://shap.readthedocs.io/en/latest/index.html)