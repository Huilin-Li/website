---
title: TP Binder Design
weight: 1
---
# Binder Design
In this blog, I want to make notes on how I design the binder of the target protein using [RFdiffusion](https://github.com/RosettaCommons/RFdiffusion), [ProteinMPNN-FastRelax](https://github.com/nrbennet/dl_binder_design/tree/main) and so on.

<center>{{<figure src="../bioIMG/AF-A2VEY9-F1.png" width="400" caption="AF-A2VEY9-F1, Alphafold." >}}</center>


In this case, I take the [A2VEY9](https://www.uniprot.org/uniprotkb/A2VEY9/entry#structure) as the example. Its 3D view is [here](https://alphafold.ebi.ac.uk/entry/A2VEY9)

## Step0: Labaries and Helpful tools
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
{{< hint warning >}}
I also programmed some helpful tools for computations. We could leave them here, and when these tools are used, we could come back and check them.
{{< /hint >}}
```python
def residue_remove(pdbfile, remove_ids, outputName):
    """
    Remove residues based on positions.
    Assumption: pdbfile only has A chain
    """
    residue_to_remove = []

    pdb_io = PDB.PDBIO()
    pdb_parser = PDB.PDBParser()
    structure = pdb_parser.get_structure(" ", pdbfile)

    model = structure[0]
    chain = model["A"]

    for residue in chain:
        id = residue.id
        if id[1] in remove_ids: 
            residue_to_remove.append(residue.id)

    for residue in residue_to_remove:
        chain.detach_child(residue)
    pdb_io.set_structure(structure)
    pdb_io.save(outputName)
    return 
```

```python
def update_resnum(pdbfile):
    """
    🌟 CHANGE RESIDUE NUMBER from discrete position IDs to continuous position IDs
    Assumption: pdbfile only has A chain
    """
    pdb_io = PDB.PDBIO()
    pdb_parser = PDB.PDBParser()
    structure = pdb_parser.get_structure(" ", pdbfile)

    model = structure[0]
    chain = model["A"]
    print(len([i for i in chain.get_residues()]))
    new_resnums = list(range(1, 1+len([i for i in chain.get_residues()])))

    for i, residue in enumerate(chain.get_residues()):
        res_id = list(residue.id)
        res_id[1] = new_resnums[i]
        residue.id = tuple(res_id)

    pdb_io.set_structure(structure)
    pdb_io.save(pdbfile.split('pdb')[0] + '_update_resnum.pdb')
    return

```

```python
# distance calculation
def dist_to_plane(p, p0, p1, p2):
    """
    Distance from point p to the plane determined by three points (p0, p1, p2)
    """
    u = p1 - p0
    v = p2 - p0
    # vector normal to plane
    n = np.cross(u, v)
    n /= np.linalg.norm(n)
    p_ = p - p0
    dist_to_plane = np.dot(p_, n)
    return dist_to_plane
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
For example, I want to **remove AAs with very low pLDDT and AAs who are located in the cell membrane**. By selecting the most big dark green area, we exclude AAs with very low pLDDT score from highlighted areas. We collect positions of AAs with very low pLDDT are (1-13, 336-693).

<center>{{<figure src="../bioIMG/fig1.PNG" width="600" caption="https://alphafold.ebi.ac.uk/entry/A2VEY9" >}}</center>

At the same time, we can collecr positions of AAs who are located in the transmembrane parts from *Subcellular Location* section [here](https://www.uniprot.org/uniprotkb/A2VEY9/entry#subcellular_location).
<center>{{<figure src="../bioIMG/fig2.PNG" width="800" caption="https://www.uniprot.org/uniprotkb/A2VEY9/entry#subcellular_location" >}}</center>

```python
# truncating ids
truncating_ids = set(list(range(1,14))+list(range(336,694))+list(range(43,64))+list(range(72,93))+list(range(191,212))+list(range(233,254))+list(range(347,368)))
len(truncating_ids) #=455

residue_remove(pdbfile="AF-A2VEY9-F1-model_v4.pdb", 
               remove_ids=truncating_ids, 
               outputName="truncation.pdb")
```
Let's check this truncated protein in [PyMol](https://pymol.org/)! We can see that the original protein becomes two much smaller parts, but these two parts are more robust to analysis in next steps.
<center>{{<figure src="../bioIMG/fig3.PNG" width="800" caption="Original protein (red) and Truncated protein(green)" >}}</center>

## Step2. Visualize truncation.pdb with only 𝐶𝛽 ATOM
