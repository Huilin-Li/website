---
title: backbones filtering
weight: 4
---
In [design binders via RFdiffusion and ProteinMPNN-FastRelax]({{<ref "./design.md" >}}), at the beginning of [sequence design and binder assessment script `mpnn_af.slm`]({{<ref "./design.md#step3">}}), we could perform a fitering step to remove useless backnones.

In my opinion, this is a very heloful step, because it greatly increase the speed and efficiency of desigining reasonable binders. For example, my `target.pdb` is like:
<center>{{<figure src="../bioIMG/target1.png" width="400" caption="`target.pdb` (only showing C_{/beta}) atom">}}</center>

I firstly finding two centers:
