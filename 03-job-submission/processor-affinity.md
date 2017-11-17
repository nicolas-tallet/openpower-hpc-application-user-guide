---
layout: page
title: Processor Affinity
---

# Processor Affinity

## IBM Spectrum LSF *p8aff* esub [Recommended Option]

This mechanism is invoked through the following directive inside the IBM Spectrum LSF job submission script:
```
#BSUB -a p8aff(*num_threads_per_task*,*smt*,*cpus_per_core*,*distribution_policy*)
```
The *p8aff* function takes the following arguments:

| Argument               | Value         | Purpose
|:----------------------:|:-------------:|:-------:
| *num_threads_per_task* | 1-160         | Number of threads per task (OMP_NUM_THREADS environment variable will be initialized with the given value)
| *smt*                  | 1, 2, 4, 8    | SMT mode to apply to all allocated compute nodes
| *cpus_per_core*        | 1-8           | Number of logical cores (CPUs) used by threads inside every physical core
| *distribution_policy*  | balance, pack | Task distribution inside a compute node

> Except for the number of threads per task, all arguments are considered as optional, and can therefore remain unspecified.
However, for clarity purpose, it is recommended to specify all four arguments.

> The list of arguments is comma-separated, with no space between separators and values.

Distribution policy is a two-option strategy that affects the way MPI tasks are spread onto the two sockets of the compute nodes:
*	*Pack*:
  * No balancing is performed between the two sockets.
  * MPI tasks placement starts with socket #0, and moves to socket #1 whenever socket #0 is full.
*	*Distribute*:
  * A balancing is performed between the two sockets, so that they host approximately the same number of MPI tasks at the end.

The following examples show the difference between the two strategies in a SMT=1 and 4 MPI Task configuration:

* *Pack*:

| 0  | 8  | 16 | 24 | 32 | 40 | 48 | 56 | 64 | 72 | 80 | 88 | 96 | 104 | 112 | 120 | 128 | 136 | 144 | 152 |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| X  | X  | X  | X  |    |    |    |    |    |    |    |    |    |     |     |     |     |     |     |     |

* *Distribute* Strategy:

| 0  | 8  | 16 | 24 | 32 | 40 | 48 | 56 | 64 | 72 | 80 | 88 | 96 | 104 | 112 | 120 | 128 | 136 | 144 | 152 |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| X  | X  |    |    |    |    |    |    |    |    | X  | X  |    |     |     |     |     |     |     |				

The following table details the IBM Spectrum LSF *p8aff* esub parameters to be used for several standard execution configurations:

| MPI Tasks / Node | Threads / Task | IBM Spectrum LSF *p8aff* esub Directive
|:----------------:|:--------------:|:---------------------------------------:
| 1                | 20             | #BSUB -a p8aff(20,1,1,balance)
| 2                | 10             | #BSUB -a p8aff(10,1,1,balance)
| 4                | 5              | #BSUB -a p8aff(5,1,1,balance)
| 10               | 2              | #BSUB -a p8aff(2,1,1,balance)
| 20               | 1              | #BSUB -a p8aff(1,1,1,balance)
| 20               | 2              | #BSUB -a p8aff(2,2,2,balance)
| 20               | 4              | #BSUB -a p8aff(4,4,4,balance)

> The *p8aff* esub automatically sets the following two environment variables:
* `LSB_EFFECTIVE_RSRCREQ`: Effective IBM Spectrum LSF Resource Requirement, including Affinity String used to control the Processor Affinity of the current job.
* `LSB_JOB_SMT_MODE`: SMT mode in effect on the allocated nodes for the current job.

## IBM Spectrum LSF Explicit Resource Requirement Specification

The explicit resource requirement is expressed through the following directive inside the submission script:
```
#BSUB -R “affinity[<pu_type>(#):cpubind=<level>:distribute=<policy>]”
```
This directive has the following syntax:

| Sub-Option          | Value   | Purpose
|:-------------------:|:-------:|:-------:
| pu_type(#)          | numa    | Type and number of Processor Units assigned to each task
|                     | socket  |
|                     | core    |
|                     | thread  |
| cpubind=level      | numa    | Task binding policy
|                     | socket  |
|                     | core    |
|                     | thread  |
| distribute=policy 	| balance | Task distribution onto the Processor Units inside a compute node
|                     | pack    |

## IBM Spectrum LSF Open MPI-Specific *ompi* Application

This mechanism is invoked through:
* The *application* option to the `bsub` submission command:
```
bsub -app ompi < job.sh
```
* The *rankfile* option to the Open MPI `mpiexec` command:
```
mpiexec -rf ${LSB_RANK_HOSTFILE} <binary>
```
instead of:
```
mpirun --hostfile ${LSB_DJOB_HOSTFILE} -np ${LSB_DJOB_NUMPROC} <binary>
```

> The *ompi* predefined application takes care of generating the rankfile to be used by the Open MPI execution command.

## Standalone Shared Memory (Multi-Threads) Job Submission

* Option #1: Mixed IBM XL + OpenMP Environment Variables
```
export OMP_PROC_BIND=true
export XLSMPOPTS={ startproc=<cpu-number>:stride=<stride> | procs=<cpu-list> }
```

> When settings defined by OMP and XLSMPOPTS environment variables conflict, priority is given to OMP environment variables.

* Option #2: Pure OpenMP Environment Variables
```
export OMP_PROC_BIND=true
export OMP_PLACES={<place>}:<count>:<stride>
```

> What's the difference between OMP_PLACES and GOMP_CPU_AFFINITY?
