+++
title = "SeqIO.parse()"
bookToc=true
+++ 

```python
from Bio import SeqIO

with open("fa.fasta") as fa:
    for record in SeqIO.parse(fa, "fasta"):
        ID = record.id
        seq = record.seq
        description = recrod.description 

```