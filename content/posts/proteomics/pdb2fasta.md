+++
title = "pdb 2 fasta"
bookToc=true
+++ 



```python
from Bio import SeqIO

def pdb2fasta(pdb_file, fasta_file, HEADER=None):
    with open(pdb_file, 'r') as pdb_file:
        for record in SeqIO.parse(pdb_file, 'pdb-atom'):
            if '?' in record.id:
                header = HEADER
            else:
                header = record.id
            with open(fasta_file, 'w') as fasta_file:
                fasta_file.write('>' + header+'\n')
                fasta_file.write(str(record.seq))
    return record.seq
```