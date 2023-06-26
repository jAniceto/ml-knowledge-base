# Similarity Search

We can use the molecular fingerprints to calculate the similarity between a list of molecules. 

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

## References

- [What is the Tanimoto Similarity?](https://www.pinecone.io/learn/roughly-explained/tanimoto-similarity/)