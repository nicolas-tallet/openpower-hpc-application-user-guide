# Processor Affinity

Processor Affinity can be handled in the following ways:

* Through the IBM Spectrum LSF Affinity directive
* Through the IBM Spectrum LSF _p8aff_ esub \[Valid for POWER8-based systems\]
* Through a user-specific script

## IBM Spectrum LSF Affinity Directive

The explicit resource requirement is expressed through the following directive inside the submission script:

```text
#BSUB -R "affinity[<pu_type>(#[,<options>]):cpubind=<level>:distribute=<policy>]"
```

This directive has the following syntax:

| Sub-Option | Value | Purpose |
| :---: | :---: | :---: |
| pu\_type\(\#\) | numa | Type and number of Processor Units assigned to each task |
|  | socket |  |
|  | core |  |
|  | thread |  |
| cpubind= | numa | Task binding policy |
|  | socket |  |
|  | core |  |
|  | thread |  |
| distribute= | balance | Task distribution onto the Processor Units inside a compute node |
|  | pack |  |

The following table details the IBM Spectrum LSF _affinity_ directive syntax to be used for several standard execution configurations:

| MPI Tasks / Node | Threads / Task | IBM Spectrum LSF Affinity Directive |
| :---: | :---: | :---: |
| 1 | 18 | \#BSUB -R "affinity\[thread\(1,exclusive=\(core,intask\)\)\*18:cpubind=thread:distribute=balance\]" |
| 1 | 36 | \#BSUB -R "affinity\[thread\(1,exclusive=\(core,intask\)\)\*36:cpubind=thread:distribute=balance\]" |
| 2 | 1 | \#BSUB -R "affinity\[thread\(1\):cpubind=thread:distribute=balance\]" |
| 2 | 18 | \#BSUB -R "affinity\[thread\(1,exclusive=\(core,intask\)\)\*18:cpubind=thread:distribute=balance\]" |
| 6 | 6 | \#BSUB -R "affinity\[thread\(1,exclusive=\(core,intask\)\)\*6:cpubind=thread:distribute=balance\]" |
| 9 | 4 | \#BSUB -R "affinity\[thread\(1,exclusive=\(core,intask\)\)\*4:cpubind=thread:distribute=balance\]" |
| 18 | 2 | \#BSUB -R "affinity\[thread\(1,exclusive=\(core,intask\)\)\*2:cpubind=thread:distribute=balance\]" |
| 36 | 1 | \#BSUB -R "affinity\[thread\(1\):cpubind=thread:distribute=balance\]" |
| 36 | 2 | \#BSUB -R "affinity\[thread\(2,same=core,exclusive=\(core,intask\)\):cpubind=thread:distribute=balance\]" |
| 36 | 4 | \#BSUB -R "affinity\[thread\(4,same=core,exclusive=\(core,intask\)\):cpubind=thread:distribute=balance\]" |

## IBM Spectrum LSF _p8aff_ esub

This mechanism is invoked through the following directive inside the IBM Spectrum LSF job submission script:

```text
#BSUB -a p8aff(<num_threads_per_task>,<smt>,<cpus_per_core>,<distribution_policy>)
```

The _p8aff_ function takes the following arguments:

| Argument | Value | Purpose |
| :---: | :---: | :---: |
| _num\_threads\_per\_task_ |  | Number of threads per task \(OMP\_NUM\_THREADS environment variable will be initialized with the given value\) |
| _smt_ | 1, 2, 4 | SMT mode to apply to all allocated compute nodes |
| _cpus\_per\_core_ | 1, 2, 3, 4 | Number of logical cores \(CPUs\) used by threads inside each physical core |
| _distribution\_policy_ | balance, pack | Task distribution inside a compute node |

> Except for the number of threads per task, all arguments are considered as optional, and can therefore remain unspecified. However, for clarity purpose, it is recommended to specify all four arguments.
>
> The list of arguments is comma-separated, with no space between separators and values.

Distribution policy is a two-option strategy that affects the way MPI tasks are spread onto the two sockets of the compute nodes:

* _Pack_:
  * No balancing is performed between the two sockets.
  * MPI tasks placement starts with socket \#0, and moves to socket \#1 whenever socket \#0 is full.
* _Distribute_:
  * A balancing is performed between the two sockets, so that they host approximately the same number of MPI tasks at the end.

The following examples show the difference between the two strategies in a SMT=1 and 4 MPI Task configuration:

* Strategy _Pack_:

| 0 | 4 | 8 | 12 | 16 | 20 | 24 | 28 | 32 | 36 | 40 | 44 | 48 | 52 | 56 | 60 | 64 | 68 | 72 | 76 | 80 | 84 | 88 | 92 | 96 | 100 | 104 | 108 | 112 | 116 | 120 | 124 | 128 | 132 | 136 | 140 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| X | X | X | X |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |

* Strategy _Distribute_:

| 0 | 4 | 8 | 12 | 16 | 20 | 24 | 28 | 32 | 36 | 40 | 44 | 48 | 52 | 56 | 60 | 64 | 68 | 72 | 76 | 80 | 84 | 88 | 92 | 96 | 100 | 104 | 108 | 112 | 116 | 120 | 124 | 128 | 132 | 136 | 140 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| X | X |  |  |  |  |  |  |  |  | X | X |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |

The following table details the IBM Spectrum LSF _p8aff_ esub parameters to be used for several standard execution configurations:

| MPI Tasks / Node | Threads / Task | IBM Spectrum LSF _p8aff_ esub Directive |
| :---: | :---: | :---: |
| 1 | 18 | \#BSUB -a p8aff\(18,4,1,balance\) |
| 1 | 36 | \#BSUB -a p8aff\(36,4,1,balance\) |
| 2 | 18 | \#BSUB -a p8aff\(18,4,1,balance\) |
| 6 | 6 | \#BSUB -a p8aff\(6,4,1,balance\) |
| 9 | 4 | \#BSUB -a p8aff\(4,4,1,balance\) |
| 18 | 2 | \#BSUB -a p8aff\(2,4,1,balance\) |
| 36 | 1 | \#BSUB -a p8aff\(1,4,1,balance\) |
| 36 | 2 | \#BSUB -a p8aff\(2,4,2,balance\) |
| 36 | 4 | \#BSUB -a p8aff\(4,4,4,balance\) |

> The _p8aff_ esub automatically sets the following two environment variables:
>
> * `LSB_EFFECTIVE_RSRCREQ`: Effective IBM Spectrum LSF Resource Requirement, including Affinity String used to control the Processor Affinity of the current job.
> * `LSB_JOB_SMT_MODE`: SMT mode in effect on the allocated nodes for the current job.

## IBM Spectrum LSF Open MPI-Specific _ompi_ Application

If the Open MPI library has been be built without integrated support for IBM Spectrum LSF, the following mechanism is required to enforce processor affinity:

* Add the _application_ option to the Spectrum LSF `bsub` submission command:

  ```text
  bsub -app ompi < job.sh
  ```

* Add the _rankfile_ option to the Open MPI `mpiexec` command:

  ```text
  mpiexec -rf ${LSB_RANK_HOSTFILE} <binary>
  ```

  instead of:

  ```text
  mpirun --hostfile ${LSB_DJOB_HOSTFILE} -np ${LSB_DJOB_NUMPROC} <binary>
  ```

> The Spectrum LSF _ompi_ predefined application takes care of generating the rankfile to be used by the Open MPI execution command.

