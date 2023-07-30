# MACCS fingerprints

A chemical fingerprint is a list of binary values (0 or 1) which characterize a molecule. There are several ways to create the list. Here we describe the widely used MACCS (Molecular ACCess System) keys.

The MACCS keys are a set of questions about a chemical structure, for instance:

- Are there fewer than 3 oxygens?
- Is there a S-S bond?
- Is there a ring of size 4?
- Is at least one F, Cl, Br, or I present?

The result of this is a list of binary values – either true (1) or false (0). This list of values for a given chemical structure is called the MACCS key fingerprint for that structure.

Here's an example. If the molecule is `C1CCC1` then the answers to those questions are:

- 0 oxygens < 3 oxygens → True
- no S-S bond → False
- there is a ring of size 4 → True
- there are no halogens → False

The answers are frequently written as a list of bits (also called a bitstring). The bitstring for this molecule is `1010`. 

There are 166 public keys (fragment definitions) of MACCS in RDKit implementation. Essentially, it is a binary fingerprint (zeros and ones) that answer 166 fragment related questions. 


## Computing MACCS keys

In the following we assume you have a dataframe (`df`) with a column containning the molecule `rdchem.Mol` object (`df['ROMol']`).

```python
import pandas as pd
from rdkit.Chem import MACCSkeys

maccs = [MACCSkeys.GenMACCSKeys(x) for x in df['ROMol']]

maccs_lists = [list(l) for l in maccs]

maccs_name = [f'MACCS_{i}' for i in range(167)]

maccs_df = pd.DataFrame(maccs_lists, index=df.index, columns=maccs_name)

maccs_df.shape
# (8221, 167)
```

## References 

- [RDKit MACCS implementation](https://github.com/rdkit/rdkit/blob/master/rdkit/Chem/MACCSkeys.py)