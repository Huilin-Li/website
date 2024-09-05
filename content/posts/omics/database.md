# databases

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
# split into 22 entry_i.txt file
n = 10000
for g, subdf in df.groupby(np.arange(len(df)) // n):
    entry_text = open('entry_'+str(g)+'.txt', 'w')
    for id in subdf['id'].tolist():
        entry_text.write("%s," % id)
```
 Obatin the batch-download script **batch_download.sh** from [Batch Downloads with Shell Script](https://www.rcsb.org/docs/programmatic-access/batch-downloads-with-shell-script)

```shell
#!/bin/bash
cd /(your path)
while IFS= read -r line; do
   ./../batch_download.sh -f ./../groups/${line} -p &
done < <(grep "" ./../groups/entry_list.txt)
```
where *entry_list.txt* has *entry_i.txt* line by line.
```shell
$ bash parallel_download.sh > download.log 
```
### desciption



## 