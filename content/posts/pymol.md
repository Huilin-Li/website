+++
title = "PyMol commands"
description = ""
tags = ["PyMol",]
categories = ["PyMol",]
weight = 1
+++ 
 
 - select residues based on residue number
```t
select nterm, resi 42+74+43+56+39
```
- mutate one amino acid
```t
cmd.wizard("mutagenesis")
cmd.fetch("target.pdb")
cmd.get_wizard().set_mode("LEU")
cmd.get_wizard().do_select("chain A and resid 22")
cmd.get_wizard().apply()
cmd.save("mutated_target.pdb")
```
