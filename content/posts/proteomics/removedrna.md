+++
title = "remove DNA/RNA and O from pdb format"
bookToc=true
+++ 

```python
from Bio import PDB


class rm_dna_rna_O(PDB.Select):
    def accept_residue(self, res):
        if (len(res.get_resname()) <3) or (res.id[0] != " "):
            return False
        else:
            return True



if __name__=="__main__":
    for pdbfile_path in glob.glob("/path/PDBDB/*.pdb"):
        print(pdbfile_path, end=" ")
        name = pdbfile_path.split("/")[-1]
        pdb = PDB.PDBParser().get_structure(name, pdbfile_path)
        pdb_io = PDB.PDBIO()
        pdb_io.set_structure(pdb)
        pdb_io.save("/path/PDBDB_no_drna/"+name, rm_dna_rna_O())
        print('-- Done') 
```
        








