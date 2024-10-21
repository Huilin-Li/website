+++
title = "use unipressed.UniprotkbClient analysis UniProt(Swiss-Prot) DB"
bookToc=true
+++
We can reveal these information.

{{< hint warning >}}
To minimize duplications between the UniProt Swiss-Prot database and the PDB database, I focused on downloading the structures of 513,805 entries from UniProt (Swiss-Prot) that are only available in the Alphafold Protein Structure Database. Therefore, I only need to download 4,274 entries' structures, because the remaining 509,531 entries' structures can be directly extracted from (Fig.2).


{{< /hint >}}



```
{'AFDB': 'hasAFDB',
 'PDB': 'no',
 'annotationScore': 1.0,
 'comments': nan,
 'entryAudit': "{'firstPublicDate': '1988-08-01', 'lastAnnotationUpdateDate': "
               "'2022-05-25', 'lastSequenceUpdateDate': '1988-08-01', "
               "'entryVersion': 36, 'sequenceVersion': 1}",
 'entryType': 'UniProtKB reviewed (Swiss-Prot)',
 'extraAttributes': "{'countByFeatureType': {'Chain': 1}, 'uniParcId': "
                    "'UPI000013C2E9'}",
 'features': "[{'type': 'Chain', 'location': {'start': {'value': 1, "
             "'modifier': 'EXACT'}, 'end': {'value': 79, 'modifier': "
             "'EXACT'}}, 'description': 'Putative uncharacterized protein Z', "
             "'featureId': 'PRO_0000066556'}]",
 'geneLocations': nan,
 'genes': nan,
 'index': 860,
 'keywords': "[{'id': 'KW-1185', 'category': 'Technical term', 'name': "
             "'Reference proteome'}]",
 'organism': "{'scientificName': 'Ovis aries', 'commonName': 'Sheep', "
             "'taxonId': 9940, 'lineage': ['Eukaryota', 'Metazoa', 'Chordata', "
             "'Craniata', 'Vertebrata', 'Euteleostomi', 'Mammalia', "
             "'Eutheria', 'Laurasiatheria', 'Artiodactyla', 'Ruminantia', "
             "'Pecora', 'Bovidae', 'Caprinae', 'Ovis']}",
 'organismHosts': nan,
 'primaryAccession': 'P08105',
 'proteinDescription': "{'recommendedName': {'fullName': {'value': 'Putative "
                       "uncharacterized protein Z'}}}",
 'proteinExistence': '4: Predicted',
 'references': "[{'referenceNumber': 1, 'citation': {'id': '6193483', "
               "'citationType': 'journal article', 'authors': ['Powell B.C.', "
               "'Sleigh M.J.', 'Ward K.A.', 'Rogers G.E.'], "
               "'citationCrossReferences': [{'database': 'PubMed', 'id': "
               "'6193483'}, {'database': 'DOI', 'id': "
               "'10.1093/nar/11.16.5327'}], 'title': 'Mammalian keratin gene "
               'families: organisation of genes coding for the B2 high-sulphur '
               "proteins of sheep wool.', 'publicationDate': '1983', "
               "'journal': 'Nucleic Acids Res.', 'firstPage': '5327', "
               "'lastPage': '5346', 'volume': '11'}, 'referencePositions': "
               "['NUCLEOTIDE SEQUENCE [GENOMIC DNA]']}]",
 'secondaryAccessions': nan,
 'sequence': "{'value': "
             "'MSSSLEITSFYSFIWTPHIGPLLFGIGLWFSMFKEPSHFCPCQHPHFVEVVIPCDSLSRSLRLRVIVLFLAIFFPLLNI', "
             "'length': 79, 'molWeight': 9128, 'crc64': 'A663EB489F6290C3', "
             "'md5': '80A5E20AFD024495655E6A9FA7B6166B'}",
 'uniProtKBCrossReferences': "[{'database': 'EMBL', 'id': 'X01610', "
                             "'properties': [{'key': 'ProteinId', 'value': "
                             "'CAA25758.1'}, {'key': 'Status', 'value': '-'}, "
                             "{'key': 'MoleculeType', 'value': "
                             "'Genomic_DNA'}]}, {'database': 'PIR', 'id': "
                             "'S07912', 'properties': [{'key': 'EntryName', "
                             "'value': 'S07912'}]}, {'database': "
                             "'AlphaFoldDB', 'id': 'P08105', 'properties': "
                             "[{'key': 'Description', 'value': '-'}]}, "
                             "{'database': 'Proteomes', 'id': 'UP000002356', "
                             "'properties': [{'key': 'Component', 'value': "
                             "'Unplaced'}]}]",
 'uniProtkbId': 'Z_SHEEP'}
```







```python
from unipressed import UniprotkbClient
import pandas as pd

# Entries in Uniprot(Swiss-Prot)
uniprot_sp_entries_txt = open("/path/uniprot_sprot.fasta/SwissProt_entries.txt", "r")
uniprot_sp_entries = uniprot_sp_entries_txt.read().split('\n')
print(len(uniprot_sp_entries), uniprot_sp_entries[-3:])

# Split the whole entries in Uniprot(Swiss-Prot) into some chunks with the size of 1000
chunks = [uniprot_sp_entries[x:x+1000] for x in range(0, len(uniprot_sp_entries), 1000)]
print("We have ", len(chunks), " chunks, and the last chunk has ", len(chunks[-1])," entries.")


# analyze using unipressed
no_uniProtKBCrossReferences_list = []
no_uniProtKBCrossReferences_count = 0


for i in range(len(chunks)):
    print(i, end=": ")
    chunk = chunks[i]
    db_dic = UniprotkbClient.fetch_many(chunk)
    df = pd.DataFrame(db_dic)
    print("chunk size is ", len(df), end="; ")

    # no_uniProtKBCrossReferences list and count
    no_uniProtKBCrossReferences_primaryAccession_list = df[df["uniProtKBCrossReferences"].isna()]["primaryAccession"].tolist()
    no_uniProtKBCrossReferences_list.extend(no_uniProtKBCrossReferences_primaryAccession_list)
    no_uniProtKBCrossReferences_count = no_uniProtKBCrossReferences_count + len(no_uniProtKBCrossReferences_primaryAccession_list)
    print("no_uniProtKBCrossReferences_count is ", no_uniProtKBCrossReferences_count)

    # has uniProtKBCrossReferences df
    db_df = df[~df["uniProtKBCrossReferences"].isna()]
    # has PDB structure or has AFDB structure
    db_df.loc[:,["PDB"]] = db_df["uniProtKBCrossReferences"].apply(lambda x: "hasPDB" if "PDB" in pd.DataFrame(x)["database"].tolist() else "no")
    db_df.loc[:,["AFDB"]] = db_df["uniProtKBCrossReferences"].apply(lambda x: "hasAFDB" if "AlphaFoldDB" in pd.DataFrame(x)["database"].tolist() else "no")
    db_df.to_csv("/path/UniprotkbClient_results/db_df_"+str(i)+".csv")




# write no_uniProtKBCrossReferences_list to txt file
no_uniProtKBCrossReferences_file = open("/path/UniprotkbClient_results/no_uniProtKBCrossReferences_entries.txt", "w")
for line in no_uniProtKBCrossReferences_list:
    no_uniProtKBCrossReferences_file.write(line+"\n")
no_uniProtKBCrossReferences_file.close()
```