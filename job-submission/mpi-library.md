# MPI Library

## IBM Spectrum MPI `mpirun` Options

* General Options

| Option | Purpose |
| :---: | :---: |
| -display-allocation | Report List of Allocated Hosts |
| -report-bindings | Report Processor Affinity Through Visual Representation |
| -stdio p | Prefix _stdout_ by Rank |

> Output Example for Option `-display-allocation`:
>
> ```text
> ======================   ALLOCATED NODES   ======================
>     host01: slots=4 max_slots=0 slots_inuse=0 state=UP
> =================================================================
> ```
>
> Output Example for Option `-report-bindings`:
>
> ```text
>
> ```

* Interconnect Selection

| Option | Purpose |
| :---: | :---: |
| -mxm | Use MXM Interconnect instead of PAMI \(Default\) |
| -pami\_noib | Only Use PAMI Shared Memory Mechanism & Avoid Opening IB Device |
| -prot | Display Protocols to be Used inside and between Hosts |
| -verbsbypass {3.1,3.2,3.3} | Bypass PAMI Verbs \(Requires IB Verbs 3.1 / 3.2 / 3.3\) |

> InfiniBand \(POWER\): PAMI with MOFED 3.1, 3.2, and 3.3

