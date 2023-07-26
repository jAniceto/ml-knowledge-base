# Feature engineering

Features are independent variables that are used as inputs for the model. In chemistry or material science, they are also referred as descriptors. It can be any relevant characteristic of the molecule or material, such as:

- measured experimental properties

- tabulated properties

- calculated properties obtained from numerous methods, such as, molecular dynamics and cheminformatics.

In most numerical problem cases, finding the features with higher predictive is usually more important than the choice of algorithm.


## Molecular descriptors

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


## Further reading

- [Computing Molecular Descriptors](https://drzinph.com/computing-molecular-descriptors-intro/)


## References

- Mauri, A., Consonni, V., Todeschini, R. (2017). Molecular Descriptors. In: Leszczynski, J., Kaczmarek-Kedziera, A., Puzyn, T., G. Papadopoulos, M., Reis, H., K. Shukla, M. (eds) Handbook of Computational Chemistry. Springer, Cham. https://doi.org/10.1007/978-3-319-27282-5_51


## Contents

```{toctree}
rdkit
maccs
morgan
mordred
similarity-search
```
