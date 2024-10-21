+++
title = "pdb 2 pdbs of chains"
bookToc=true
+++

```python
from Bio import PDB



if __name__=="__main__":
    for pdbfile_path in glob.glob("/path/DB/pure_PDB/*.pdb"):
        name = pdbfile_path.split("\\")[-1].split(".")[0]
        print(name, end=" ")
        pdb = PDB.PDBParser().get_structure(name, pdbfile_path)
        pdb_io = PDB.PDBIO()
        pdb_chains = pdb.get_chains()
        for chain in pdb_chains:
            pdb_io.set_structure(chain)
            pdb_io.save("/path/DB/pure_PDB_DB_by_Chain/" + pdb.get_id() + "_" + chain.get_id() + ".pdb")
        print('-- Done') 
        



```