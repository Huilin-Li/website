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

```


```shell
for f in *.gz;
do 
    if [[ $(file --mime-type -b "$f") == "application/gzip" ]]; 
    then 
        gunzip "$f" 
    else 
        echo "$f is not a valid gzip" 
    fi
done
```

