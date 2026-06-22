# Correlations

Analysing the correlation between variables and target responses help identify which inputs carry meaningful information about the target and which are redundant or irrelevant. Measures like the Pearson correlation coefficient reveal linear relationships, while more general tools such as Distance correlation can uncover nonlinear dependencies. By analysing these relationships, you can reduce multicollinearity, improve model interpretability, and focus on variables that genuinely contribute to predictive performance, leading to simpler and more robust models.


## Linear correlations

The Pearson correlation coefficient (often denoted $r$) is a statistical measure that quantifies the strength and direction of a linear relationship between two continuous variables. 

The coefficient is mathematically defined as the covariance of two variables divided by the product of their standard deviations:

$$
r =  \frac{ \sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y}) }{\sqrt{\sum_{i=1}^{n}(x_i-\bar{x})^2}\sqrt{\sum_{i=1}^{n}(y_i-\bar{y})^2}}
$$

Values close to $+1$ indicate a strong positive linear relationship (both variables increase together), while values near $–1$ indicate a strong negative relationship. A value around $0$ suggests no linear correlation.

The Pearson correlation coefficient assumes that both variables are continuous, approximately normally distributed, and exhibit a linear relationship with homoscedasticity (equal variance). It is sensitive to outliers and may misrepresent non-linear relationships. In such cases, non-parametric alternatives like Spearman rank correlation coefficient or Kendall's tau coefficient are preferred.


## Nonlinear correlations

The standard Pearson correlation coefficient won't help if the correlation between variables in nonlinear. A perfect quadratic relationship (like $y=x^2$) can even give a Pearson correlation near zero. To detect nonlinear correlations, other methods must be used, such as:

- Variable transformation;
- Rank-based correlations;
- Distance-based correlations.

### Variable transformation 

If you suspect a specific nonlinear form (like quadratic, $y=x^2$), transform your data and then apply Pearson correlation. Computing the Pearson correlation between $y$ and $x$ will show low values since the correlation is not linear but computing the Pearson correlation between $y$ and $x^2$ will show a high Pearson correlation.

### Rank-based correlations

The Spearman's rank correlation coefficient detects monotonic relationships (increasing or decreasing, not necessarily linear) between variables. It works for many nonlinear cases, but fails for non-monotonic ones (like pure quadratic $x^2$).

Spearman's coefficient quantifies how well the relationship between two variables can be described by a monotonic function (one that never reverses direction). Each variable's values are first converted into ranks, differences between paired ranks are squared and summed, and the result is scaled by sample size.

Values of $+1$ or $-1$ indicate perfect monotonic association (ranks increase or decrease together exactly) while $0$ indicates no monotonic relationship. Because it relies on ranks rather than raw scores, Spearman's coefficient is robust to outliers and applicable to ordinal or skewed data. 

### Distance-based correlations

Distance correlation is a statistical measure of dependence between random variables or vectors. It extends classical correlation by detecting both linear and nonlinear associations between variables. Unlike Pearson's correlation, it equals zero only if the variables are statistically independent.

To compute distance correlation we can use a dedicated library like [`dcor`](https://github.com/vnmabus/dcor).

```python
import numpy as np
import dcor

# Example data (nonlinear relationship)
x = np.linspace(-2, 2, 100)
y = x**2  # quadratic relationship

# Compute distance correlation
dcor_value = dcor.distance_correlation(x, y)
```

A `dcor_value` of $0$ means variables are independent (no correlation). A `dcor_value` of $> 0$ means some dependence is present (linear or nonlinear). A `dcor_value` close to $1$ means a strong relationship exists.

Alternative calculations:

```python
dcor.distance_covariance(x, y)
dcor.distance_correlation_sqr(x, y)
```

You can test if the dependence is statistically significant:

```python
result = dcor.independence.distance_covariance_test(x, y)
```

A small $p$-value (e.g., $< 0.05$) means you should reject independence.


## Correlations between lagged variables (delays)

When variables are affected by a time delay, standard correlation (computed at the same time index) can completely miss the relationship. Instead, you need to evaluate correlation as a function of lag.

A cross-correlation function will measure how well one signal matches another when shifted in time. The idea is:

1) We shift $x$ by different lags $k$;
2) We compute correlation between $x(t−k)$ and $y(t)$.

The lag with the highest correlation tells you if a relationship exists and how long the delay is.

Instead of computing only $corr(x_t, y_t)$, we compute $corr(x_{t-k}, y_t)$ for many values of $k$. If the higher correlation shows at, for instance, $k=5$ then changes in $x$ affect $y$ only 5 samples later. Knowing the interval between samples (for instance, 1 min) we know the time delay (in this case: 5 samples $\times$ 1 min $=$ 5 min delay).

```python
import numpy as np
import matplotlib.pyplot as plt

# Example data (y depends on x with delay 5)
x = np.random.randn(100)
y = np.roll(x, 5) + 0.1*np.random.randn(100)  # y depends on x with delay 5

# Compute cross-correlation
corr = np.correlate(y - y.mean(), x - x.mean(), mode='full')
lags = np.arange(-len(x)+1, len(x))

# Normalize
corr = corr / (np.std(x) * np.std(y) * len(x))

# Find best lag
best_lag = lags[np.argmax(corr)]
print("Best lag:", best_lag)

plt.plot(lags, corr)
plt.xlabel("Lag")
plt.ylabel("Correlation")
plt.show()
``` 

An here is an example `sklearn` pipeline for automatic lag selection using regularization. It:
 - generates lagged features;
 - handles missing values;
 - scales data;
 - uses Lasso regression to automatically select the most relevant lags;
 - works with time-aware validation.


 ```python
import numpy as np
import pandas as pd

from sklearn.base import BaseEstimator, TransformerMixin
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LassoCV
from sklearn.model_selection import TimeSeriesSplit

# ---------------------------
# Custom lag feature generator
# ---------------------------
class LagFeatureTransformer(BaseEstimator, TransformerMixin):
    def __init__(self, lags):
        self.lags = lags

    def fit(self, X, y=None):
        return self

    def transform(self, X):
        X_lagged = pd.DataFrame(index=X.index)

        for col in X.columns:
            for lag in self.lags:
                X_lagged[f"{col}_lag_{lag}"] = X[col].shift(lag)

        return X_lagged


# ---------------------------
# Example data
# ---------------------------
np.random.seed(0)
n = 200

df = pd.DataFrame({
    "x1": np.random.randn(n),
    "x2": np.random.randn(n)
})

# target depends on delayed x1
df["y"] = df["x1"].shift(3) + 0.1*np.random.randn(n)

df = df.dropna()

X = df[["x1", "x2"]]
y = df["y"]

# ---------------------------
# Define lag range
# ---------------------------
lags = range(1, 11)  # try lags 1 to 10

# ---------------------------
# Pipeline
# ---------------------------
pipeline = Pipeline([
    ("lags", LagFeatureTransformer(lags=lags)),
    ("dropna", FunctionTransformer(lambda X: X.dropna(), validate=False)),
    ("scaler", StandardScaler()),
    ("model", LassoCV(cv=TimeSeriesSplit(n_splits=5)))
])

# Align target after lagging
X_lagged = pipeline.named_steps["lags"].transform(X)
valid_idx = X_lagged.dropna().index

X_final = X_lagged.loc[valid_idx]
y_final = y.loc[valid_idx]

# Fit model
pipeline.named_steps["model"].fit(
    StandardScaler().fit_transform(X_final),
    y_final
)

# ---------------------------
# Inspect selected lags
# ---------------------------
feature_names = X_final.columns
coeffs = pipeline.named_steps["model"].coef_

selected = [
    (name, coef)
    for name, coef in zip(feature_names, coeffs)
    if abs(coef) > 1e-6
]

print("Selected lagged features:")
for name, coef in selected:
    print(f"{name}: {coef:.3f}")
``` 


Same example but using but using a nonlinear model (tree-based), which can capture complex delayed relationships without needing explicit transformations. We use Gradient Boosting via `GradientBoostingRegressor`.

```python
import numpy as np
import pandas as pd

from sklearn.base import BaseEstimator, TransformerMixin
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import GradientBoostingRegressor
from sklearn.model_selection import TimeSeriesSplit

# ---------------------------
# Lag feature transformer
# ---------------------------
class LagFeatureTransformer(BaseEstimator, TransformerMixin):
    def __init__(self, lags):
        self.lags = lags

    def fit(self, X, y=None):
        return self

    def transform(self, X):
        X_lagged = pd.DataFrame(index=X.index)

        for col in X.columns:
            for lag in self.lags:
                X_lagged[f"{col}_lag_{lag}"] = X[col].shift(lag)

        return X_lagged


# ---------------------------
# Example data
# ---------------------------
np.random.seed(0)
n = 300

df = pd.DataFrame({
    "x1": np.random.randn(n),
    "x2": np.random.randn(n)
})

# Nonlinear delayed relationship
df["y"] = (df["x1"].shift(3)**2 + 
           0.5 * np.sin(df["x2"].shift(5)) +
           0.1*np.random.randn(n))

df = df.dropna()

X = df[["x1", "x2"]]
y = df["y"]

# ---------------------------
# Lag setup
# ---------------------------
lags = range(1, 11)

lagger = LagFeatureTransformer(lags=lags)

# Create lagged data
X_lagged = lagger.transform(X)
valid_idx = X_lagged.dropna().index

X_final = X_lagged.loc[valid_idx]
y_final = y.loc[valid_idx]

# ---------------------------
# Model (nonlinear)
# ---------------------------
model = GradientBoostingRegressor(
    n_estimators=200,
    max_depth=3,
    learning_rate=0.05,
    random_state=0
)

# ---------------------------
# Time-series CV
# ---------------------------
tscv = TimeSeriesSplit(n_splits=5)

for train_idx, test_idx in tscv.split(X_final):
    X_train, X_test = X_final.iloc[train_idx], X_final.iloc[test_idx]
    y_train, y_test = y_final.iloc[train_idx], y_final.iloc[test_idx]
    
    model.fit(X_train, y_train)

# ---------------------------
# Feature importance = lag selection
# ---------------------------
importances = model.feature_importances_
feature_names = X_final.columns

selected = sorted(
    [(name, imp) for name, imp in zip(feature_names, importances)],
    key=lambda x: x[1],
    reverse=True
)

print("Most important lagged features:")
for name, imp in selected[:10]:
    print(f"{name}: {imp:.4f}")
```