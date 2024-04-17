+++
title = "Bash Scripts"
description = ""
tags = ["bash", "git"]
categories = ["Command-line",]
+++ 



# Bash Scripts


## How to discard changes in VScode?
Navigate to reporsitory (has `.github` folder) with changes. Then, `git restore <file>`
{{< figure src="../images/discard.png" alt=" ">}}
## Activate conda environment in bash script
```console
source ~/miniconda3/etc/profile.d/conda.sh
conda activate ENV
```
## SBATCH Array jobs
```console
#SBATCH -J array-job					
#SBATCH -a 1-3
#SBATCH -o array-job.%A.%a.log

id_list="./id_list.txt"
id=`head -n $SLURM ARRAY TASK ID $id_list | tail -n 1`
python3 script.py -in $id
```

