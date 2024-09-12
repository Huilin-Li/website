+++
title = "databases: PDB, Swiss-Prot"
bookToc=true
+++


## [PDB database](https://www.rcsb.org/docs/programmatic-access/file-download-services)
### download
1. https://files.rcsb.org/pub/pdb/data/structures/all/pdb/

{{< figure src="../img/pdb_db.png" width="300" alt=" ">}}

2. save this website *PDB - FTP Archive over HTTP.html*
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
        PDB_id_list.append(line.split(".")[0].split("pdb")[1])
# PDB_id_list[:5]
# ['100d', '101d', '101m', '102d', '102l']
```
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
### desciption



## [Swiss-Prot database](https://www.uniprot.org/uniprotkb?query=*&facets=reviewed%3Atrue)
### download
[UniProt](https://www.uniprot.org/uniprotkb?query=*&facets=reviewed%3Atrue) provides the reviewed Swiss-Prot database.
{{< figure src="../img/uniprot.png" width="500" alt=" ">}}
Each *Entry* might (1) only have 3D structure from [Alphafold Protein Structure Database](https://alphafold.ebi.ac.uk/download#swissprot-section); (2) only have 3D structure from [PDB database](https://www.rcsb.org/docs/programmatic-access/file-download-services)





#### option1: directly download compressed prediction files for Swiss-Prot from [Alphafold Protein Structure Database](https://alphafold.ebi.ac.uk/download#swissprot-section)

{{< figure src="../img/afdb_sprot.png" width="600" alt=" ">}}

#### option2: manually download by *Entry* from [UniProt](https://www.uniprot.org/uniprotkb?query=*&facets=reviewed%3Atrue)
1. download proteins in *fasta* format.
{{< figure src="../img/uniprot.png" width="500" alt=" ">}}
2. 






### description
In *UniProt*,  Swiss-Prot has 571,864 entries with its corresponding fasta file. 549,724 entries have 3D struture. And most (513,805) of these structure are from AlphaFold prediction.
{{< figure src="../img/swprot_distribution.png" alt=" ">}}



https://www.uniprot.org/uniprotkb/A0A6N3IN21/entry#structure