# RDKit

## Set up RDKit

Installing RDKit with `conda`:

```
$ conda -c rdkit rdkit
```

Using in a Jupyter Notebook:

```python
import rdkit
from rdkit import Chem
from rdkit.Chem import AllChem

from rdkit.Chem.Draw import IPythonConsole
```


## Basic usage

Get a RDKit molecule object from SMILES. From the RDKit molecule object we can draw structures, compute fingerprints/properties, etc.

```python
smiles = 'COC(=O)c1c[nH]c2cc(OC(C)C)c(OC(C)C)cc2c1=O'
mol = Chem.MolFromSmiles(smiles)
print(mol)

# <rdkit.Chem.rdchem.Mol object at 0x000001F84A4CEE90>
```

Reading a list of SMILES:

```python
smiles = [
    'N#CC(OC1OC(COC2OC(CO)C(O)C(O)C2O)C(O)C(O)C1O)c1ccccc1',
    'c1ccc2c(c1)ccc1c2ccc2c3ccccc3ccc21',
    'C=C(C)C1Cc2c(ccc3c2OC2COc4cc(OC)c(OC)cc4C2C3=O)O1',
    'ClC(Cl)=C(c1ccc(Cl)cc1)c1ccc(Cl)cc1'
]

mols = [Chem.MolFromSmiles(smi) for smi in smiles]
```

Draw molecules into grid:

```python
from rdkit.Chem import Draw

Draw.MolsToGridImage(mols, molsPerRow=2, subImgSize=(200, 200))
```

Using `PandaTools` to allow molecule objects in dataframes:

```python
import pandas as pd
from rdkit.Chem import PandasTools

url = 'https://raw.githubusercontent.com/XinhaoLi74/molds/master/clean_data/ESOL.csv'

df = pd.read_csv(url)

PandasTools.AddMoleculeColumnToFrame(df, smilesCol='smiles')
```

This adds a column to the dataframe containing a `rdchem.Mol` object.

To draw the stuctures in a grid:

```python
PandasTools.FrameToGridImage(df.head(8), legendsCol='logSolubility', molsPerRow=4)
```

To add new columns of properites use Pandas `map` method.

```python
df['n_Atoms'] = df['ROMol'].map(lambda x: x.GetNumAtoms())
```


## Computing descriptors/fingerprints

RDKit has a variety of built-in functionality for generating molecular fingerprints/descriptors.


```python
from descriptastorus.descriptors.DescriptorGenerator import MakeGenerator
#https://github.com/bp-kelley/descriptastorus

generator = MakeGenerator(("RDKit2D",))

rdkit2d = [generator.process(x)[1:] for x in df['SMILES']]

rdkit2d_name = []
for name, numpy_type in generator.GetColumns():
    rdkit2d_name.append(name)

rdkit2d_df = pd.DataFrame(rdkit2d, index=df.index, columns=rdkit2d_name[1:])

train_rdkit2d_df.shape
# (8221, 200)
```


## References

- [RDKit Cookbook, RDKit Documentation](https://www.rdkit.org/docs/Cookbook.html)

- [RDKit Fingerprints, RDKit Documentation](https://www.rdkit.org/docs/RDKit_Book.html#additional-information-about-the-fingerprints)

- [RDKit Cheatsheet](https://xinhaoli74.github.io/posts/2020/04/RDKit-Cheatsheet/)

- [Hierarchical-QSAR-Modeling Notebook](https://github.com/XinhaoLi74/Hierarchical-QSAR-Modeling/blob/master/notebooks/descriptors.ipynb)
