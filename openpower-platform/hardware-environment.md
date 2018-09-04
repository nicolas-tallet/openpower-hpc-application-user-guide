# Hardware Environment

## Technical Specifications

The IBM Power System compute nodes have the following technical characteristics:

| Commercial Name                          | S822LC<br>8335-GTB         | AC922<br>8335-GTX 18-Cores | AC922<br>8335-GTX 22-Cores
|:----------------------------------------:|:--------------------------:|:--------------------------:|:--------------------------:|
| # Sockets                                | 2                          | 2                          | 2
| CPU Model                                | POWER8NV                   | POWER9                     | POWER9
| Physical Cores                           | 20 (2x10)                  | 36 (2x18)                  | 44 (2x22)
| Hardware Threads (Per Physical Core)     | 1 / 2 / 4 / 8              | 1 / 2 / 4                  | 1 / 2 / 4
| Clock Frequency: Nominal / Max. (Turbo)  | 2.86 GHz / 4.03 GHz        | 3.15 GHz / 3.45 GHz        | 2.80 GHz / 3.10 GHz
| CPU Peak Performance (Double Precision)  | 458 GFlops                 | 907 GFlops                 | 986 GFlops
| L3 Cache (Per Physical Core)             | 8 MB                       | 5 MB                       | 5 MB
| L4 Cache                                 | 16 MB per Buffer Chip      | -                          | -
| Memory Max.                              | 1 TB                       | 2 TB                       | 2 TB
| Memory Bandwidth                         | 230 GB/s                   | 280 GB/s                   | 280 GB/s
| # GPU Devices                            | 4                          | 6                          | 6
| GPU Model                                | NVIDIA Tesla P100          | NVIDIA Tesla V100          | NVIDIA Tesla V100
| GPU Peak Performance (Double Precision)  | 5.3 TFlops                 | 7.8 TFlops                 | 7.8 TFlops
| GPU Memory                               | 16 GB                      | 16 GB                      | 16 GB
| GPU Memory Bandwidth                     | 732 GB/s                   | 900 GB/s                   | 900 GB/s
| Node Peak Performance (Double Precision) | 22 TFlops                  | 48 TFlops                  | 48 TFlops

## Simultaneous Multi-Threading (SMT)

The IBM Power System nodes can be configured in distinct SMT modes.  
Each SMT mode defines the number of logical cores (hardware threads) available on the node:

| SMT Mode | \# Logical Cores
|:--------:|:----------------:|
| 1 (Off)  | 20
| 2        | 40
| 4        | 80
| 8        | 160

> SMT mode configuration does not require the system to be restarted; therefore, SMT mode configuration can be performed on-the-fly.
