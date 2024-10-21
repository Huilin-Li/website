+++
title = "commonly used Shell script and Command line, when working on large dataset in HPC"
bookToc=true
+++

```shell
ls -l | wc -l

wget -i links.txt -o wget.log

tar -tzf file.tar.gz | wc -l # count files in tar.gz without unzip # it actually counts lines, and it will cause counting \n as a line

tar -zcvf  myfolder.tar.gz myfolder

cp ./folder/*.txt ./newfolder

gunzip *.gz # for f in *.gz; do gunzip "$f"; done

cat *.fa > all_data.fa # concatenate multiple fasta files into one fasta file in command line

source ~/miniconda3/etc/profile.d/conda.sh
conda activate SE3nv
```


```shell
PDBdb_dir_has=()
for pdb_fp in $PDBdb_dir*pdb; do
    pdb=$(basename $pdb_fp .pdb)
    PDBdb_dir_has+=($pdb)
done
```

```shell
A=()
lines=`cat $AFDB_pLDDT_70_log`
for line in $lines; do
    if [[ $line  == *".pdb" ]]; then
        entry=$(echo "$line" | cut -d "." -f 1)
        A+=($entry)
    fi
done
```

