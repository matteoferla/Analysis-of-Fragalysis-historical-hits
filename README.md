# Analysis of historical fragment hits in Fragalysis
> :construction: This is work that was in progress and requires more, but I am doing other things :construction:

## Rationale

In my current strategies I sort virtual hits via multiple properties and cluster by interactions.
For sorting "risk" is solely how much of the template hit is retained.
But in expansions some things work better than others.

![pipeline-02.png](images/pipeline-02.png)

Interactions are enthalpic, but entropy is also important due to:

**Rigidification** (entropic penalty): sidechains and ligand become more rigid.
The loss of 1 degree of rotational freedom is on the one kcal/mol. This is visible via the B-factors.

**Induced fit** (entropic penalty): the protein changes shape to accommodate the ligand.
In terms of sidechains, if it is not in the correct orientation then the k_on rate will be lower.

**Desolvation** (entropic gain): freeing waters from the binding site, 
means more particles in solution, which is entropically favourable (cf. chelate effect).

> For more see this 
[OPIG blog post](https://www.blopig.com/blog/2023/11/demystifying-the-thermodynamics-of-ligand-binding/)

## Method

Fragalysis historical data downloaded. PLIP annotated.

* Download, see [download](notebooks/mass_download.ipynb)
* Analyses see [bfactor](notebooks/bfactor.ipynb)
* Analyses 2 see [bfactor](notebooks/bfactor-extra.ipynb)

## Results

> Jupyter notebooks are in the [notebooks](notebooks) folder.

Data downloaded, normalized and PLIP annotated.

### Initial screen filtering
The initial libraries were identified using the dataset 
from [repo:Fragment-hit-follow-up-chemistry](https://github.com/matteoferla/Fragment-hit-follow-up-chemistry)
```python
from rdkit import Chem, rd
import pandas as pd

libraries = pd.read_csv('https://github.com/matteoferla/Fragment-hit-follow-up-chemistry/raw/main/combined-XChem-libraries.csv', index_col=0)
reduced_libraries  =  libraries.drop_duplicates(['SMILES', 'library']) \
                     .groupby('SMILES').agg({'library': ','.join, 
                                             'Id':set }) \
                     .reset_index()
reduced_libraries['Id'] = reduced_libraries['Id'].apply(','.join)
```

TODO Add matching text

### B-factors
Caveat: normalizing the B-factors by Z-score normalisation is very common, but they are not gaussian distributed.

![ZBfactor-hist.png](images/ZBfactor-hist.png)

Does normalised B-factor correlate with number of hits found? 0.05 Spearman correlation is poor.
This includes residues that form no interactions with ligands.

![ZBfactor-vs-hits.png](images/ZBfactor-vs-hits.png)

TODO etc etc 

### SASA

TODO Add results after surface acessible solvent area correction

### Water

TODO Add results from water displacement

## Future work

Identification of the target pocket is crucial.