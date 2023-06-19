# Molecular descriptors

A molecular descriptor is defined as the "final result of a logical and mathematical procedure, which transforms chemical information encoded within a symbolic representation of a molecule into a useful number or the result of some standardized experiment".

Molecular descriptors can be divided into two main categories: 

- experimental measurements, which are usually  physico-chemical properties such as refractivity, dipole moment, polarizability, etc.; 

- theoretical molecular descriptors, which are derived from a symbolic representation of the molecule and can be further classified according to the different types of molecular representation.

The main types of theoretical molecular descriptors are: 

- 0D-descriptors (i.e. constitutional descriptors, count descriptors);

- 1D-descriptors (i.e. list of structural fragments, fingerprints);

- 2D-descriptors (i.e. graph invariants);

- 3D-descriptors (such as, for example, 3D-MoRSE descriptors, WHIM descriptors, GETAWAY descriptors, quantum-chemical descriptors, size, steric, surface and volume descriptors);

- 4D-descriptors (such as those derived from GRID or CoMFA methods, Volsurf). 


Software for molecular descriptors calculation:

- RDKit;

- Mordred - based on RDKit, up to 1826 descripotrs, open-source, [Github](https://github.com/mordred-descriptor/mordred);

- PaDEL-descriptor - based on Chemistry Development Kit (CDK), up to 1875 descripotors, open-source; [Paper]( https://doi.org/10.1002/jcc.21707)

- PIKAChU - written in Python, [Paper](https://doi.org/10.1186/s13321-022-00616-5), [Github](https://github.com/BTheDragonMaster/pikachu)


## RDKit

### Set up RDKit

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


### Basic usage

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


### Computing descriptors/fingerprints

RDKit has a variety of built-in functionality for generating molecular fingerprints/descriptors.


#### Morgan Fingerprint (ECFPx)

To compute Morgan Fingerprint (ECFPx) use `AllChem.GetMorganFingerprintAsBitVect`. ECFP6 fingerprint for each molecule has 1024 bits. In the following `df['ROMol']` is the column containning the molecule `rdchem.Mol` object.

```python
from rdkit.Chem import AllChem

radius=3
nBits=1024

ECFP6 = [AllChem.GetMorganFingerprintAsBitVect(x,radius=radius, nBits=nBits) for x in df['ROMol']]

ecfp6_lists = [list(l) for l in ECFP6]

ecfp6_name = [f'ECFP_{i}' for i in range(nBits)]

ecfp6_df = pd.DataFrame(ecfp6_lists, index=df.index, columns=ecfp6_name)

ecfp6_df.shape
# (8221, 1024)
```

The `radius` is usually set 2 for similarity search and 3 for machine learning. For the number of bits (`nBits`) the default is 2048. 1024 is also widely used.


#### MACCS keys

```python
from rdkit.Chem import MACCSkeys

maccs = [MACCSkeys.GenMACCSKeys(x) for x in df['ROMol']]

maccs_lists = [list(l) for l in maccs]

maccs_name = [f'MACCS_{i}' for i in range(167)]

maccs_df = pd.DataFrame(maccs_lists, index=df.index, columns=maccs_name)

maccs_df.shape
# (8221, 167)
```

#### Other RDKit descriptors

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

### Similarity Search

We can use the molecular fingerprints to calculate the similarity between a list of molecules. Here is an example of using ECFP4 fingerprint to compute the Tanimoto Similarity (the default metric of `DataStructs.FingerprintSimilarity`).

```python
from rdkit import DataStructs

ref_smiles = 'COC(=O)c1c[nH]c2cc(OC(C)C)c(OC(C)C)cc2c1=O'
ref_mol = Chem.MolFromSmiles(ref_smiles)
ref_ECFP4_fps = AllChem.GetMorganFingerprintAsBitVect(ref_mol,2)

bulk_ECFP4_fps = [AllChem.GetMorganFingerprintAsBitVect(x,2) for x in df['ROMol']]

similarity_efcp4 = [DataStructs.FingerprintSimilarity(ref_ECFP4_fps,x) for x in bulk_ECFP4_fps]
```

We can also add the `similarity_efcp4` to the dataframe and visualize the structure and similarity.

```python
df['Tanimoto_Similarity (ECFP4)'] = similarity_efcp4
PandasTools.FrameToGridImage(df, legendsCol="Tanimoto_Similarity (ECFP4)", molsPerRow=4)

```

## Mordred

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


## Further reading

- [Computing Molecular Descriptors](https://drzinph.com/computing-molecular-descriptors-intro/)


## References

- Mauri, A., Consonni, V., Todeschini, R. (2017). Molecular Descriptors. In: Leszczynski, J., Kaczmarek-Kedziera, A., Puzyn, T., G. Papadopoulos, M., Reis, H., K. Shukla, M. (eds) Handbook of Computational Chemistry. Springer, Cham. https://doi.org/10.1007/978-3-319-27282-5_51

- [RDKit Cookbook, RDKit Documentation](https://www.rdkit.org/docs/Cookbook.html)

- [RDKit Fingerprints, RDKit Documentation](https://www.rdkit.org/docs/RDKit_Book.html#additional-information-about-the-fingerprints)

- [RDKit Cheatsheet](https://xinhaoli74.github.io/posts/2020/04/RDKit-Cheatsheet/)

- [Hierarchical-QSAR-Modeling Notebook](https://github.com/XinhaoLi74/Hierarchical-QSAR-Modeling/blob/master/notebooks/descriptors.ipynb)
