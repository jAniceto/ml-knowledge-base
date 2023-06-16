# Feature selection

Consider the following case. The [RDkit](https://www.rdkit.org/) software can generate descriptors based on the molecular structure of compounds. It generates descriptors from the simplified molecular-input line-entry system (SMILES, a method of describing the chemical structure by string).

RDkit generates 200 descriptors. However, some of them are not effective (e.g., all values are the same for all compounds, the correlation coefficient with the target is almost zero, etc.). Therefore, some pretreatments can be conducted : 
- exclude features of standard deviation for all data points = 0 (all data points have the same value in the descriptor).
- exclude features whose correlation coefficient is below a defined threshold (e.g., < 0.1).
- select features using [Boruta](https://towardsdatascience.com/boruta-explained-the-way-i-wish-someone-explained-it-to-me-4489d70e154a). Boruta is a software that can select features. It iteratively removes the features that are proved by a statistical test to be less relevant than random probes. 


## Recursive feature elimination

- [Article](https://machinelearningmastery.com/rfe-feature-selection-in-python/)

## Boruta method (`BorutaPy`)

- [Github](https://github.com/scikit-learn-contrib/boruta_py)
- [Paper](https://www.jstatsoft.org/article/view/v036i11)
- [Article](https://danielhomola.com/feature%20selection/phd/borutapy-an-all-relevant-feature-selection-method/)
