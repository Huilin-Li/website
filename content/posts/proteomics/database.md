+++
title = "databases: PDB, Swiss-Prot, OMG_Prot50"
bookToc=true
+++
- [PDB database](#pdb-database)
- [Swiss-Prot database (AI-predicted structure)](#swiss-prot-database)
- [OMG_Prot50 (Open MetaGenomic )]()


## PDB database
### download
1. https://files.rcsb.org/pub/pdb/data/structures/all/pdb/

{{< figure src="../img/pdb_db.png" width="50" alt=" ">}}

2. save this website as *PDB - FTP Archive over HTTP.html*
3. extract *pdbXXXX.ent.gz* list
```python
from bs4 import BeautifulSoup
# how many ent.gz file
with open('PDB - FTP Archive over HTTP.html', 'r') as file:
    html_content = file.read()
    
soup = BeautifulSoup(html_content, 'lxml')
text = soup.get_text('\n', '\n\n')
lines = text.split('\n')

PDB_id_list = []

for line in lines:
    if 'ent.gz' in line:
        PDB_id_list.append(line.split(".")[0][4:] #.split("pdb")[1])
# PDB_id_list[:5]
# ['100d', '101d', '101m', '102d', '102l']
```
{{< hint danger >}}
Is's not okay to extract *id* by `.split("pdb")`. Because *pdb* might be also the part of the *id* in some special cases, for example, *pdb1pdb.ent.gz*.
{{< /hint >}}

4. parallel downloads
```python
# split into 44 entry_i.txt file
length = 5000
count = len(df)//length
for i in range(43):
    subdf = df.iloc[i*5000:i*5000+5000]
    sublist =subdf["id"].tolist()
    string = ",".join(sublist)
    text = open('groups/entry_'+ str(i)+'.txt', 'w')
    text.write(string)
    text.close()

final_sublist = df.iloc[43*5000:43*5000+5000]["id"].tolist()
finalstring = ",".join(final_sublist)
finaltext = open('groups/entry_'+ str(43)+'.txt', 'w')
finaltext.write(finalstring)
finaltext.close()
```
 Obatin the batch-download script **batch_download.sh** from [Batch Downloads with Shell Script](https://www.rcsb.org/docs/programmatic-access/batch-downloads-with-shell-script)

```shell
#!/bin/bash
cd /(your path)

for line in $(cat ./../groups/entry_list.txt); do
   ./../batch_download.sh -f ./../groups/${line} -p &
done

# while IFS= read -r line; do
#    ./../batch_download.sh -f ./../groups/${line} -p &
# done < <(grep "" ./../groups/entry_list.txt)
```
where *entry_list.txt* has *entry_i.txt* line by line.
```shell
$ bash parallel_download.sh > download.log 
```
{{< hint danger >}}
*Failed download* is common when Shell script downloads large files simutaneously. Therefore, it is important to check whether the script has downloaded the complete and correct *ent.gz* files. For example, by `gunzip *gz`, the wrong *.gz* files will output `unzip error`. And then, this wrong *.gz* file should be removed and we need to download it again.

{{< /hint >}}


### desciption
At this moment, I download 218,546 PDB entries from PDB database. 








## Swiss-Prot database
### download
[UniProt](https://www.uniprot.org/uniprotkb?query=*&facets=reviewed%3Atrue) provides the reviewed Swiss-Prot database.
{{< figure src="../img/uniprot.png" title="Fig.1 the Entry from UniProt">}}


With the help of [unipressed](https://github.com/multimeric/Unipressed), I can quickly analyze the reviewed Swiss-Prot database. There are 22,140 enries whose 3D structure information are not stored. In the remaining 549,724 entries who have 3D structures, 33,725 entries have 3D structure from both [Alphafold Protein Structure Database](https://alphafold.ebi.ac.uk/download#swissprot-section) and [PDB database](https://www.rcsb.org/docs/programmatic-access/file-download-services); 2,194 entries only have 3D structure from [PDB database](https://www.rcsb.org/docs/programmatic-access/file-download-services); 513,805 entries only have 3D structure from [Alphafold Protein Structure Database](https://alphafold.ebi.ac.uk/download#swissprot-section).

I compared the dataset that is already compressed in [Alphafold Protein Structure Database](https://alphafold.ebi.ac.uk/download#swissprot-section) (Fig.2) and the 513,805 entries who only have [Alphafold Protein Structure Database](https://alphafold.ebi.ac.uk/download#swissprot-section) 3D structure as recorded in [UniProt](https://www.uniprot.org/uniprotkb?query=*&facets=reviewed%3Atrue) (Fig.1). 4,274 entries' structures in (Fig.1) have not been stored in (Fig.2). 

{{< hint warning >}}
To minimize duplications between the UniProt Swiss-Prot database and the PDB database, I focused on downloading the structures of 513,805 entries from UniProt (Swiss-Prot) that are only available in the Alphafold Protein Structure Database. Therefore, I only need to download 4,274 entries' structures, because the remaining 509,531 entries' structures can be directly extracted from (Fig.2).


{{< /hint >}}

{{< figure src="../img/afdb_sprot.png" title="Fig.2 compressed prediction files for Swiss-Prot from Alphafold Protein Structure Database">}}







### description
In *UniProt*,  Swiss-Prot has 571,864 entries with its corresponding fasta file. 549,724 entries have 3D struture. And most (513,805) of these structure are from AlphaFold prediction.
{{< figure src="../img/plot.svg" alt=" ">}}



## OMG_Prot50 database
### download
1. https://huggingface.co/datasets/tattabio/OMG_prot50?sql=--+The+SQL+console+is+powered+by+DuckDB+WASM+and+runs+entirely+in+the+browser.%0A--+Get+started+by+typing+a+query+or+selecting+a+view+from+the+options+below.%0ASELECT+*+FROM+train+LIMIT+10%3B

<iframe
  src="https://huggingface.co/datasets/tattabio/OMG_prot50/embed/viewer/default/train"
  frameborder="0"
  width="100%"
  height="560px"
></iframe>