# Job Submission Script

A submission script allows submitting user jobs through the workload scheduler.

A submission script is made of two distinct sections:

* Section \#1: Directives The header consists in a set of IBM Spectrum LSF directives. These directives are introduced by the following prefix: `#BSUB`. These directives are specified through the following syntax :  .
* Section \#2: Shell Script The script in itself is made of standard Bash commands. For parallel executions, the script involves a call to the parallel submission command - which depends on the MPI library used.

The main IBM Spectrum LSF directive keywords are the following:

| Option | Value | Purpose |
| :---: | :---: | :--- |
| -cwd | Absolute Path | Execution directory |
| -e | Filename | stderr file |
| -J | Job Name | User job name |
| -n | \# MPI Tasks | Total number of MPI tasks |
| -o | Filename | stdout file |
| -q | Queue | Target queue |
| -R | “span\[ptile=\]” | Number of MPI tasks per node |
| -W | HH:MM | Runlimit |
| -x |  | Exclusive job |

> Note: IBM Spectrum LSF uses an automatically-created directory inside the user Home Directory in order to temporarily store the elements related to a submitted job. The location of this directory is the following:
>
> ```text
> ${HOME}/.lsbatch
> ```

