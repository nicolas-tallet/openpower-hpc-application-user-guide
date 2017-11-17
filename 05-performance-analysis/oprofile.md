# OProfile

OProfile is an Open-Source project including a statistical tool for performance profiling.
OProfile is available inside the Linux distribution itself, or through the *IBM SDK for LoP* (up-to-date version with full POWER8 support).

OProfile mainly relies on the following commands:

| Command    | Purpose
|:----------:|---------
| ophelp     | List available hardware counters
| operf      | Record events
| opreport   | Display performance report
| opannotate | Annotate source code with events

## Performance Data Collection
Performance data collection is achieved through the ‘operf’ command:
```
$ operf <binary>
```

## Performance Data Reporting
Performance data reporting is achieved through the ‘opreport’ command:
```
$ opreport
CPU_CLK_UNHALT...|
. samples|      %|
------------------
.   25202 100.000 cg.W.x
CPU_CLK_UNHALT...|
. samples|      %|
------------------
.   18509 73.4426 cg.W.x
.    5190 20.5936 libiomp5.so
.    1443  5.7257 no-vmlinux
.      37  0.1468 ld-2.19.so
.      12  0.0476 libifcoremt.so.5
.       7  0.0278 libc-2.19.so
.       2  0.0079 libpthread-2.19.so
```
