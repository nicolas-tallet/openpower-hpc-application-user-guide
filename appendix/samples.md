# Samples

## IBM Spectrum LSF Typical Job Submission File

Example: Parallel Execution with 2 Compute Nodes, 8 Tasks Total, 4 Tasks / Node, 5 Threads / Task, 1 Task / GPU
```
#!/bin/bash

#BSUB -cwd /home/login/myjob
#BSUB -e myjob.%J.log
#BSUB -gpu "num=4:mode=exclusive_process:mps=no:j_exclusive=yes"
#BSUB -J MyJob
#BSUB -n 8
#BSUB -o myjob.%J.log
#BSUB -q default
#BSUB -R "span[ptile=4]"
#BSUB -W 01:00
#BSUB -x

ml xlc xlf smpi

mpirun -display-allocation -prot -report-bindings /home/login/myjob/myjob.bin
```
