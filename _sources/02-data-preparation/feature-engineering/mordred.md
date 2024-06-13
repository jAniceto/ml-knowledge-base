# Mordred

```python
from mordred import Calculator, descriptors

mordred_calc = Calculator(descriptors, ignore_3D=True)  # can't do 3D without sdf or mol file

mordred = mordred_calc.pandas([mol for mol in df['ROMol']])

mordred.shape
# (8221, 1613)

# Remove non numerical features.
mordred = mordred.select_dtypes(include=['float64', 'int64', 'float'])

mordred.shape
# (8221, 1071)
```