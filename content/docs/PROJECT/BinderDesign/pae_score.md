---
title: pae_interaction score
weight: 3
---
As [RFdiffusion](https://github.com/RosettaCommons/RFdiffusion) states, we expect a binder whose `pae_interaction` score is lower than 10. However, we should also have a look at other scores, such as pLDDT, rmsd.
```python
check_n = 6
folder_nam = ["folder"+str(i) for i in range(check_n)]
score_paths = ["../MyBinderWork/ThreeThoudand_mpnn_af/"+nam+"/score.sc" for nam in folder_nam]

ALL_DF = []
for p in score_paths:
    df = pd.read_csv(p, delim_whitespace=True)
    ALL_DF.append(df)

SCORE_df = pd.concat(ALL_DF)[["binder_aligned_rmsd", "pae_binder", "pae_interaction", "plddt_binder", "description"]]


```


<center>{{<figure src="../bioIMG/pae.PNG" width="600" caption="An example of one of my experiments">}}</center>
