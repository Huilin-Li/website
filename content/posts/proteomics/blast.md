+++
title = "sequence alignment via BLASTp with using makeblastdb for custom database"
bookToc=true
+++




```
cd root/OMGstudy/
tree
.
├── query.fasta
├── OMGDB_blast
├── OMGDB_fasta
    └── OMGDB.fasta


makeblastdb -in ./OMGDB_fasta/OMGDB.fasta -input_type fasta -dbtype prot -out ./OMGDB_blast/OMGDB.blast
tree
.
├── query.fasta
├── OMGDB_blast
│   ├── OMGDB.blast.00.phr
│   ├── OMGDB.blast.00.pin
│   ├── OMGDB.blast.00.psq
│   ├── OMGDB.blast.01.phr
│   ├── OMGDB.blast.01.pin
... ...
│   ├── OMGDB.blast.30.psq
│   ├── OMGDB.blast.31.phr
│   ├── OMGDB.blast.31.pin
... ...
│   ├── OMGDB.blast.43.phr
│   ├── OMGDB.blast.43.pin
│   ├── OMGDB.blast.43.psq
│   ├── OMGDB.blast.pal
│   ├── OMGDB.blast.pdb
│   ├── OMGDB.blast.pot
│   ├── OMGDB.blast.ptf
│   └── OMGDB.blast.pto
├── OMGDB_fasta
    └── OMGDB.fasta


blastp -query query.fasta -db /root/OMGstudy/OMGDB_blast/OMGDB.blast > query_blastp_omgdb.out
tree
.
├── query_blastp_omgdb.out
├── query.fasta
├── OMGDB_blast
│   ├── OMGDB.blast.00.phr
│   ├── OMGDB.blast.00.pin
│   ├── OMGDB.blast.00.psq
│   ├── OMGDB.blast.01.phr
│   ├── OMGDB.blast.01.pin
... ...
│   ├── OMGDB.blast.30.psq
│   ├── OMGDB.blast.31.phr
│   ├── OMGDB.blast.31.pin
... ...
│   ├── OMGDB.blast.43.phr
│   ├── OMGDB.blast.43.pin
│   ├── OMGDB.blast.43.psq
│   ├── OMGDB.blast.pal
│   ├── OMGDB.blast.pdb
│   ├── OMGDB.blast.pot
│   ├── OMGDB.blast.ptf
│   └── OMGDB.blast.pto
├── OMGDB_fasta
    └── OMGDB.fasta
```
