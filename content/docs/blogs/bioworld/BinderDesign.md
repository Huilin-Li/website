---
title: TP Binder Design
weight: 1
---
# Binder Design
In this blog, I want to make notes on how I design the binder of the target protein using [RFdiffusion](https://github.com/RosettaCommons/RFdiffusion), [ProteinMPNN-FastRelax](https://github.com/nrbennet/dl_binder_design/tree/main) and so on.

<center>{{<figure src="../bioIMG/AF-A2VEY9-F1.png" width="400" caption="AF-A2VEY9-F1, Alphafold." >}}</center>


In this case, I take the [A2VEY9](https://www.uniprot.org/uniprotkb/A2VEY9/entry#structure) as the example. Its 3D view is [here](https://alphafold.ebi.ac.uk/entry/A2VEY9)

## Step0: Labaries
```python
from biopandas.pdb import PandasPdb
from itertools import chain
from Bio import PDB
import numpy as np
import pandas as pd
import plotly.express as px
import plotly.graph_objects as go
from sklearn.cluster import KMeans
```


## Step1: Load the target protein.
The pdb file can be downloaded [here](https://alphafold.ebi.ac.uk/entry/A2VEY9).
```python
target_pdb = PandasPdb().read_pdb('AF-A2VEY9-F1-model_v4.pdb')
# read structure information into pd.DataFrame
target_df = target_pdb.df['ATOM']
```
<center>{{<figure src="../bioIMG/df1.PNG" >}}</center>

## Step2: Truncate target protein
For example, I want to **remove AAs with very low pLDDT and AAs who are located in the cell membrane**. By selecting the most big dark green area, we exclude AAs with very low pLDDT score from highlighted areas. We collect positions of AAs with very low pLDDT are (1, 2, 336-693).

<center>{{<figure src="../bioIMG/fig1.PNG" width="600" caption="https://alphafold.ebi.ac.uk/entry/A2VEY9" >}}</center>

At the same time, we can collecr positions of AAs who are located in the transmembrane parts from *Subcellular Location* section [here](https://www.uniprot.org/uniprotkb/A2VEY9/entry#subcellular_location).
<center>{{<figure src="../bioIMG/fig2.PNG" width="800" caption="https://www.uniprot.org/uniprotkb/A2VEY9/entry#subcellular_location" >}}</center>

```python
# truncating ids
truncating_ids = set([1,2]+list(range(336,694))+list(range(43,64))+list(range(72,93))+list(range(191,212))+list(range(233,254))+list(range(347,368)))
len(truncating_ids) #=444
```
