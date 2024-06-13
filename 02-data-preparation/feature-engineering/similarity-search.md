# Measure molecular similarity

There are a variety of ways to compute [molecular similarity](https://en.wikipedia.org/wiki/Chemical_similarity) but the most common approach is to generate molecular fingerprints (e.g., ECFP4) and use the Tanimoto coefficient to determine similarity.

The ECFP4 / ECFP6 / Morgan fingerprints do a pretty good job of encoding atom environments.


## Computing similarity from ECFP4

Here is an example of using ECFP4 fingerprint to compute the Tanimoto Similarity (the default metric of `DataStructs.FingerprintSimilarity`).

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

## Another, more complete, approach

```python
from rdkit import Chem
from rdkit import DataStructs
from rdkit.Chem.Fingerprints import FingerprintMols
import pandas as pd

# load csv's
df = pd.read_csv('first.csv')

# proof and make a list of SMILES
df_smiles = df_1['smiles']
c_smiles = []
for ds in df_smiles:
    try:
        cs = Chem.CanonSmiles(ds)
        c_smiles.append(cs)
    except:
        print('Invalid SMILES:', ds)
print()

# make a list of mols
ms = [Chem.MolFromSmiles(x) for x in c_smiles]

# make a list of fingerprints (fp)
fps = [FingerprintMols.FingerprintMol(x) for x in ms]

# the list for the dataframe
qu, ta, sim = [], [], []

# compare all fp pairwise without duplicates
for n in range(len(fps)-1): # -1 so the last fp will not be used
    s = DataStructs.BulkTanimotoSimilarity(fps[n], fps[n+1:]) # +1 compare with the next to the last fp
    print(c_smiles[n], c_smiles[n+1:]) # witch mol is compared with what group
    # collect the SMILES and values
    for m in range(len(s)):
        qu.append(c_smiles[n])
        ta.append(c_smiles[n+1:][m])
        sim.append(s[m])
print()

# build the dataframe and sort it
d = {'query':qu, 'target':ta, 'Similarity':sim}
df_final = pd.DataFrame(data=d)
df_final = df_final.sort_values('Similarity', ascending=False)
print(df_final)

# save as csv
df_final.to_csv('third.csv', index=False, sep=',')
```


## References

- [What is the Tanimoto Similarity?](https://www.pinecone.io/learn/roughly-explained/tanimoto-similarity/)
- [Why is Tanimoto index an appropriate choice for fingerprint-based similarity calculations?](https://doi.org/10.1186/s13321-015-0069-3)
- [Similarity Coefficients for Binary Chemoinformatics Data: Overview and Extended Comparison Using Simulated and Real Data Sets](https://doi.org/10.1021/ci300261r)
- [RDKit Manual](https://www.rdkit.org/docs/GettingStartedInPython.html#topological-fingerprints)
