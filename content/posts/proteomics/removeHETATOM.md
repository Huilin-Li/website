+++
title = "remove HETATOM from pdb format"
bookToc=true
+++ 

```python
from Bio import PDB

pdb = PDB.PDBParser().get_structure("2gq1", "FBP/2gq1.pdb")

class ResSelect(PDB.Select):
    def accept_residue(self, res):
        if res.id[0] != " ":  #>= start_res and res.id[1] <= end_res and res.parent.id == chain_id:
            return False
        else:
            return True

pdb_io = PDB.PDBIO()
pdb_io.set_structure(pdb)
pdb_io.save("FBP/2gq1_no_HETATOM.pdb", ResSelect())
```