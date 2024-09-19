+++
title = "read data"
bookToc=true
+++ 

### read `id` and `seq` from `.fasta` 
```python
from Bio import SeqIO

with open('fa.fasta') as fasta_file:  
    identifiers, seq = [], []
    for seq_record in SeqIO.parse(fasta_file, 'fasta'): 
        identifiers.append(seq_record.id)
        seq.append("".join(seq_record.seq))

fa_df = pd.DataFrame()
fa_df["seq"] = seq
fa_df["id"] = identifiers
```
### pandas read
```python
pd.read_csv('result.csv', header=None, index_col=0)
# index_col=False


```


