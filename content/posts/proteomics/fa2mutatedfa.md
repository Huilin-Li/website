+++
title = "fasta 2 mutated fasta (python code)"
bookToc=true
+++


```python
import copy

mutations = ["K114Q", "R196I", "Y20F", "K8Q"]
fa_header = open("./xxxx.fasta", "r").read().split("\n")[0]
fa_seq = open("./xxxx.fasta", "r").read().split("\n")[1]


new_seq = copy.copy(fa_seq)
for mutation in mutations:
    orig = mutation[0]
    idx = int(mutation[1:-1])
    mut = mutation[-1]
    seq_list = list(new_seq)
    if seq_list[idx-1] == orig:
        seq_list[idx-1] = mut
    else:
        print("Check!")
    new_seq = ''.join(seq_list)


with open('mutatedfa.fa', 'w') as f:
    f.write(fa_header)
    f.write("\n")
    f.write(new_seq)

```