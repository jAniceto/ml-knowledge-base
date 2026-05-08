# Data validation

## Standardizing SMILES

MolVS is a molecule validation and standardization package for Python. To install it:

```
$ conda config --add channels conda-forge
$ conda install molvs
```

Use it to standardize a SMILES string:

```python
from molvs import standardize_smiles

standardize_smiles('C[n+]1c([N-](C))cccc1')
# 'CN=c1ccccn1C'
```


## References

- [MolVS: Molecule Validation and Standardization](https://molvs.readthedocs.io/en/latest/)