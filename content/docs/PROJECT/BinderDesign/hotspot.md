---
title: ppi.hotspot_res argument
weight: 2
---
# `ppi.hotspot_res` argument
I spent a great of time on `ppi.hotspot_res` argument. 

In this blog, I want to make notes on how I design the binder of the target protein using [RFdiffusion](https://github.com/RosettaCommons/RFdiffusion), [ProteinMPNN-FastRelax](https://github.com/nrbennet/dl_binder_design/tree/main) and so on.



```python
def point_to_plane_dist(p, p0, p1, p2):
    """
    The distance from point p to the plane determined by three points (p0, p1, p2)
    :param p: point p
    :param p0, p1, p2: three points determine one plane
    :return: the distance between point to plane
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

At the same time, we can get positions of AAs who are located in the transmembrane parts from *Subcellular Location* section [here](https://www.uniprot.org/uniprotkb/A2VEY9/entry#subcellular_location).
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

## Step3. Visualize truncation.pdb with only 𝐶𝛽 ATOM
Here, we take 𝐶𝛽 ATOM as the represent of each AA. 
```python
truncation = PandasPdb().read_pdb("truncation.pdb")
cols = ["atom_name", "residue_name", "residue_number", "x_coord", "y_coord", "z_coord"]
df = truncation.df["ATOM"][cols]
df_CB = df[df["atom_name"]=="CB"]
# visualization
fig = go.Figure(data=[go.Scatter3d(x=df_CB["x_coord"],
                                   y=df_CB["y_coord"],
                                   z=df_CB["z_coord"],
                                   name="",
                                   hoverinfo="text",
                                   hovertext=df_CB["residue_number"],
                                   mode="markers",
                                   marker=dict(color="gray", size=5))])
fig.update_layout(
    width=1100,
    height=1100,
    margin=dict(l=0, r=0, b=0, t=0),
    scene=dict(xaxis_title='x coord', yaxis_title='y coord', zaxis_title='z coord'))
fig.show()
```

{{<load-plotly >}} {{<plotly json="/fig.json" height="300px" >}}

