+++
title = "Bash Scripts"
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
id=`head -n $SLURM_ARRAY_TASK_ID $id_list | tail -n 1`
python3 script.py -in $id
```

## srun gpu
```console
srun -N 1 --ntasks-per-node 2 -p v100 -q gpu --gres=gpu:1 --mem=50G --pty /bin/bash
```

## run scripts
```console
sbatch test.slm
```

## check sequence process
```console
squeue -u username
```
## cancel sequence
```console
scancel jobID
```

## copy files into another folder
```console
cp ./*.pdb ../newfoler
```

## count files in a directory in command line
```console
cd directory
ls -l | wc -l
```