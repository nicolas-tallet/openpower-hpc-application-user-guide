---
layout: page
title: Compilers
---

# Compilers

## Global

### Invocation

The invocation commands of the compilers are the following:

| Compiler | C     | C++     | Fortran 77 | Fortran 90 | Fortran 03 | Fortran 08
|:--------:|:-----:|:-------:|:----------:|:----------:|:----------:|:----------:|
| GCC	     | gcc   | g++     | gfortran   | gfortran   | gfortran   | gfortran
| IBM XL   | xlc_r | xlC_r   | xlf_r      | xlf90_r    | xlf2003_r  | xlf2008_r
| LLVM     | clang | clang++ | xlflang    | xlflang    | xlflang    | xlflang
| PGI      | pgcc  | pgCC    | pgf90      | pgf90      | pgf90      | pgf90

> For IBM XL C/C++ & Fortran, the commands specified above constitute the « thread-safe » variants, to be used when dealing with multithreaded applications.
It is recommended to exclusively use these variants, even in the case of non-multithreaded applications, though.

### Compiler Options Overview

The table below provides the main equivalences between the options of the different compilers:

| Purpose                           | GCC                              | IBM XL C/C++ & Fortran    | PGI Accelerator                        | LLVM Clang & XLFlang
|:---------------------------------:|:--------------------------------:|:-------------------------:|:--------------------------------------:|:--------------------:|
| Architecture	                    | -mcpu=power8                     | -qarch=pwr8               | -m64                                   | -target powerpcle-unknown-linux-gnu -mcpu=pwr8
| Disabled Optimization	            | -O0                              | -O0 -qnoopt               | -O0                                    | -O0
| Recommended Optimization          | -O3 -mcpu=power8 -funroll-loops  | -O3 [-qipa] [-qhot]       | -fast -Mipa=fast,inline [-Msmartalloc] | -O2
| Profile-Guided Optimization (PGO)	| -fprofile-generate/-fprofile-use | -qpdf1/-qpdf2             | -Mpfi/-Mpfo                            | -fprofile-instr-generate/-fprofile-instr-use
| Inter-Procedural Optimization     | -flto                            | -qipa                     | -Mipa                                  | N/A
| Vectorization                     | -ftree-vectorize                 | -qsimd=auto               | -Mvect                                 | N/A
| Automatic Parallelization	        | -floop-parallelize-all           | -qsmp=auto                | -Mconcur                               | N/A
| OpenMP Support                    | -fopenmp                         | -qsmp=omp	               | -mp                                    | -fopenmp
| OpenACC Offload                   | N/A                              | N/A                       | -acc -Minfo=accel -ta:tesla:cc60       | ?
| OpenMP Offload                    | N/A                              | -qoffload -qtgtarch=sm_60 | N/A                                    | -fopenmptargets=nvptx64-nvidia-cuda
| Loop Optimization	                | -fpeel-loops -funroll-loops	     | -qhot                     |                                        | -funroll-loops
| Debugging Symbols	                | -g                               | -g                        | -g                                     | -g
| Verbose                           |                                  | -V                        |                                        |

> For IBM XL C/C++ & Fortran, the `-qxflag` option has been replaced by the `-qtgtarch` option.

### GPU Device Target Specification

| Compiler | Option            | Option For:<br>Tesla K80 | Option For:<br>Tesla P100 |
|:--------:|:-----------------:|:------------------------:|:-------------------------:|
| GCC	     |                   |                          |                           |
| IBM XL   | -qtgtarch={value} | -qtgtarch=sm_37          | -qtgtarch=sm_60           |
| LLVM     |                   |                          |                           |
| PGI      | -ta:{value}       | -ta:cc37                 | -ta:cc60                  |

## Compiler-Specific

### IBM XL C/C++ & IBM XL Fortran

#### Optimization Levels

The IBM XL C/C++ and IBM XL Fortran compilers allow the following optimization levels:

| Options                      | Purpose
|:----------------------------:|:-------:|
| -O0 -qnohot -qnosmp -qstrict | Disable any optimization
| -O2                          | Limited optimization (safe)
| -O3 -qstrict                 | Medium optimization without modification of the code semantics
| -O3                          | Medium optimization with potential modifications of the code semantics
| -O4                          | Agressive optimization + Light Inter-Procedural Analysis (IPA)
| -O5                          | Equivalent to -O4 level with deeper IPA

> Increasing the optimization level tends to lead to a potentially significant increase in compilation time.
It is therefore recommended to keep limited optimization levels while porting an application onto the platform.

For a given optimization level, the IBM XL compilers automatically introduce additional options by default. These options can be in turn complemented with other additional options.
The table below details the main possible combinations:

| Optimization Level | Additional Options Introduced By Default   | Additional Options | Other Options with Potential Benefits
|:------------------:|:------------------------------------------:|:------------------:|:-------------------------------------:|
| -O0                | -qsimd=auto                                | -qarch=pwr8        | |
| -O2                | -qmaxmem=8192                              | -qarch=pwr8        | -qmaxmem=-1
|                    | -qsimd=auto                                | -qtune=pwr8        | -qhot=level=0
|                    | -qstrict=all                               |                    | |
| -O3                | -qhot=noarraypad:level=0:novector:fastmath | -qarch=pwr8        | |
|                    | -qnostrict                                 | -qtune=pwr8        | |
|                    | -qmaxmem=-1                                |                    | |
|                    | -qsimd=auto                                |                    | |
| -O4                | -qarch=auto                                | -qarch=pwr8        | -qsmp=auto
|                    | -qcache=auto                               | -qcache            | |
|                    | -qhot=noarraypad:level=1:vector:fastmath   | -qtune=pwr8        | |
|                    | -qipa                                      |                    | |
|                    | -qmaxmem=-1                                |                    | |
|                    | -qnostrict                                 |                    | |
|                    | -qsimd=auto                                |                    | |
|                    | -qtune=auto                                |                    | |
| -O5                | -qarch=auto                                | -qarch=pwr8        | -qsmp=auto
|                    | -qcache=auto                               | -qcache            | |
|                    | -qhot=noarraypad:level=1:vector:fastmath   | -qtune=pwr8        | |
|                    | -qipa=level=2                              |                    | |
|                    | -qmaxmem=-1                                |                    | |
|                    | -qnostrict                                 |                    | |
|                    | -qsimd=auto                                |                    | |
|                    | -qtune=auto                                |                    | |

#### Other Compiler Options

* Floating-Point and Integer Control

| Option                    | Purpose
|:-------------------------:|:-------:|
| -qautodbl={dbl4,dbl8,dbl} | Convert single-precision floating-point calculations to double-precision and double-precision calculations to extended-precision
| -qdpc                     | Promote basic real constants as double-precision constants
| -qintsize={2,4*,8}        | Set default size of INTEGER / LOGICAL data entities
| -qrealsize={4*,8}         | Set default size of REAL / DOUBLE PRECISION / (DOUBLE) COMPLEX values

* Object Code Control

| Option            | Purpose
|:-----------------:|:-------:|
| -qsaveopt         | Save compilation-related information to the corresponding object file

> Compilation-related information stored thanks to the `-qsaveopt` option can be read through the `string -a` command.

### PGI Accelerator

* Default Compiler Options: Create User-Specific File in:
```
~/.mypgirc
```
Acceptable Directives:
  * Default Compute Capability:
  ```
  set DEFCOMPUTECAP=60;
  ```
  * Default CUDA Version:
  ```
  set DEFCUDAVERSION=8.0;
  ```

* `-fast` Flag Configuration:
```
$ pgcc -fast -help
Reading rcfile /opt/pgi/linuxpower/16.10/bin/.pgccrc
-fast               Common optimizations; includes -O2 -Munroll=c:1 -Mlre -Mautoinline
                    == -Mvect=simd
-M[no]vect[=[no]simd|[no]assoc]
                    Control automatic vector pipelining
    [no]simd        Generate [don't generate] SIMD instructions
    [no]assoc       Allow [disallow] reassociation
```
