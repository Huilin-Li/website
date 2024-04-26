---
title: Truncation
weight: 1
---
# `target.pdb` preprocessing

Assuming we have a target protein `target.pdb`, and we want to create a binder to `target.pdb`. The first step is preprocessing `target.pdb`. Generally spearking, there are two steps we need to think of.

## Truncating `target.pdb` 
Truncating target protein helps a lot in reducing the stress of computing. For example, we could **remove AAs with very low pLDDT or AAs who are far away from the place we want binders to bind with**. 

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
    chain = model["A"] # assuming pdbfile only has A chain

    for residue in chain:
        id = residue.id
        if id[1] in remove_ids: 
            residue_to_remove.append(residue.id)

    for residue in residue_to_remove:
        chain.detach_child(residue)
    pdb_io.set_structure(structure)
    pdb_io.save(outputName)
    return 

# truncating ids
truncating_ids = [1,2,3]

# remove some AAs
residue_remove(pdbfile="target.pdb", remove_ids=truncating_ids, outputName="truncation.pdb")
```
This step can also be achieved in **PyMol**

{{< hint danger >}}
**How much we can remove from our target protein?**  
At the very beginning, I was thinking removing all useless AAs. For example, I was creating a binder to a transmembrane protein, and I only left the part of protein which is located at the outside of the whole protein. Unfornutately, this part of protein I aim to create binders to bind with is too "thin" to lead design binders correctly.

If the "target" protein you feed into the RFdiffusion model is too small, `ppi.hotspot_res` argument becomes ineffective. Let's imagine the "target" protein we feed into the RFdiffusion model is a paper, and `ppi.hotspot_res` is located somewhere in this paper. Since this "target" protein (the paper) is too thin, RFdiffusion model feels confused about which side (above or below the paper ) the `ppi.hotspot_res` points to.

My suggestion would be to try some different trunctations and pick one that can approcimately balance the computation and correct leading of bidner design.
{{< /hint >}}


## Reset AAs' residues numbers in `truncation.pdb`
Normally, after removing some AAs, the residues number in `truncation.pdb` becomes discontinuous, and we want to concatenate them again and update their residue number with continuous numbers.

```python
def update_resnum(pdbfile):
    """
    🌟 CHANGE RESIDUE NUMBER
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
    pdb_io.save(pdbfile.split('.pdb')[0] + '_update_resnum.pdb')
    return
```