# Frequently Asked Questions (FAQ)

## How To Display the Map of Physical Cores / Logical Cores?

The `ppc64_cpu` command displays the relationship between physical cores and logical cores (hardware threads) of the system:
```
$ ppc64_cpu --info
Core   0:    0*    1*    2*    3*    4*    5*    6*    7*
Core   1:    8*    9*   10*   11*   12*   13*   14*   15*
Core   2:   16*   17*   18*   19*   20*   21*   22*   23*
Core   3:   24*   25*   26*   27*   28*   29*   30*   31*
Core   4:   32*   33*   34*   35*   36*   37*   38*   39*
Core   5:   40*   41*   42*   43*   44*   45*   46*   47*
Core   6:   48*   49*   50*   51*   52*   53*   54*   55*
Core   7:   56*   57*   58*   59*   60*   61*   62*   63*
Core   8:   64*   65*   66*   67*   68*   69*   70*   71*
Core   9:   72*   73*   74*   75*   76*   77*   78*   79*
Core  10:   80*   81*   82*   83*   84*   85*   86*   87*
Core  11:   88*   89*   90*   91*   92*   93*   94*   95*
Core  12:   96*   97*   98*   99*  100*  101*  102*  103*
Core  13:  104*  105*  106*  107*  108*  109*  110*  111*
Core  14:  112*  113*  114*  115*  116*  117*  118*  119*
Core  15:  120*  121*  122*  123*  124*  125*  126*  127*
Core  16:  128*  129*  130*  131*  132*  133*  134*  135*
Core  17:  136*  137*  138*  139*  140*  141*  142*  143*
Core  18:  144*  145*  146*  147*  148*  149*  150*  151*
Core  19:  152*  153*  154*  155*  156*  157*  158*  159*
```
In the above example, the system is configured with SMT8. Each of the 20 physical cores (*Core 0* to *Core 19*) is made of 8 logical cores (hardware threads).

## How To Specify Pre-Processing Option (Macro) to IBM XL Fortran Compiler?

The IBM XL Fortran does not support the standard syntax related to pre-processing options.
With this compiler, it is required to add the *-WF* option to introduce macros, as shown in the example below:
```
xlf90_r -qfree=f90 '-WF,-Dmacro1=1,-Dmacro2' a.F
```

## How To Increase The IBM XL SMP Library OpenMP Stack Size?

By default, the IBM XL SMP library has a very limited OpenMP stack size value (4 MB).
It is therefore frequent to alter this default value through the following environment variable setting:
```
export XLSMPOPTS=”stack=16777216”
```
Note:
> Stack size must be specified in Bytes.

## How To Avoid Some PGI Compilation Errors?

By default, PGI Accelerator uses its own preprocessor.
It is often the case that the preprocessor must be set to the default system preprocessor to avoid compilation errors. Setting the CPP environment variable is normally enough:
```
export CPP=/usr/bin/cpp
```

## How To Open An Interactive Session Through IBM Spectrum LSF?

IBM Spectrum LSF allows opening an interactive session onto the compute nodes through the following command:
```
bsub -Ip -n <cpus> -q <queue> /bin/bash
```
Obviously, the interactive session will wait for the requested resource allocation to be possible before the session can actually open.
An additional, specific interconnect option is required by the `mpirun` command when launching an MPI execution in interactive mode:
```
-pami_noib
```

## How To Solve The ‘Accelerator Does Not Match’ Error Message?

The error message has the following syntax:
```
Current region was compiled for:
NVIDIA Tesla GPU sm30 sm35
Available accelerators:
device[1]: Native X86 (CURRENT DEVICE)
The accelerator does not match the profile for which this program was compiled
```
This message is linked to the fact that no visible GPU device has been defined for the given task.
The solution involves properly setting the environment variable `CUDA_VISIBLE_DEVICES`.

## How To Monitor GPU Activity on Interactive Nodes?

Two options exist for monitoring the GPU activity on one of the interactive nodes:
*	Option #1: NVIDIA System Management Interface Command
```
$ nvidia-smi pmon
# gpu     pid  type    sm   mem   enc   dec   command
# Idx       #   C/G     %     %     %     %   name
.   0  145349     C    64     0     0     0   matrixMul
.   1       -     -     -     -     -     -   -
.   2  145351     C    65     0     0     0   matrixMul
.   3       -     -     -     -     -     -   -
```
*	Option #2: `gpustat` Utility
```
host  Mon Feb 13 19:08:43 2017
[0] Tesla K80        | 51'C,  23 % |    64 / 11441 MB | login:matrixMul/40281(62M)
[1] Tesla K80        | 33'C,   0 % |     2 / 11441 MB |
[2] Tesla K80        | 37'C,   0 % |     2 / 11441 MB |
[3] Tesla K80        | 31'C,   0 % |     2 / 11441 MB |
```
