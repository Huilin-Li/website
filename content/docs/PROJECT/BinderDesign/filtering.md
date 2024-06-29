---
title: backbones filtering
weight: 4
---
In [design binders via RFdiffusion and ProteinMPNN-FastRelax]({{<ref "./design.md" >}}), at the beginning of [sequence design and binder assessment script `mpnn_af.slm`]({{<ref "./design.md#step3">}}), we could perform a fitering step to remove useless backnones.

In my opinion, this is a very heloful step, because it greatly increase the speed and efficiency of desigining reasonable binders. For example, in my `target.pdb`, I only expect designed binders could bind with the outside part of the `target.pdb` (see fig.1).
<center>{{<figure src="../bioIMG/CB.png" caption="fig.1 `target.pdb` and atoms where binders should bind with">}}</center>


<center>{{<figure src="../bioIMG/vis_bbs.png" caption="fig.2 distribution of all designed backbone centers">}}</center>