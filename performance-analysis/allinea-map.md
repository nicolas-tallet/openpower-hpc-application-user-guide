---
layout: page
title: Allinea Map
---
# Allinea Map

## Setup Performance Analysis

* Setup Performance Analysis Environment:
```
$ make-profiler-libraries
Creating shared libraries in /u/ntallet
Created the libraries:
   libmap-sampler.so       (and .so.1, .so.1.0, .so.1.0.0)
   libmap-sampler-pmpi.so  (and .so.1, .so.1.0, .so.1.0.0)
.
To instrument a program, add these compiler options:
   compilation for use with MAP - not required for Performance Reports:
      -g (and -O3 etc.)
   linking (both MAP and Performance Reports):
      -L/u/ntallet -lmap-sampler-pmpi -lmap-sampler -Wl,--eh-frame-hdr -Wl,-rpath=/u/ntallet
.
Note: These libraries must be on the same NFS/Lustre/GPFS filesystem as your
  program.

Before running your program (interactively or from a queue), set
LD_LIBRARY_PATH:
   export LD_LIBRARY_PATH=/u/ntallet:$LD_LIBRARY_PATH
   map  ...
or add -Wl,-rpath=/u/ntallet when linking your program.
```
