# Hardware Environment

## General Characteristics

The IBM Power System S822LC compute nodes have the following technical characteristics:

| Commercial Name                       | IBM Power System S822LC | IBM Power System S822LC for HPC
|:-------------------------------------:|:-----------------------:|:-------------------------------:|
| Code Name                             | Firestone               | Minsky
| Model Type                            | 8335-GTA                | 8335-GTB
| \# CPUs                               | 2                       | 2
| Model                                 | POWER8                  | POWER8NV
| Physical Cores                        | 10                      | 10
| Physical Threads                      | 10 / 20 / 40 / 80       | 10 / 20 / 40 / 80
| Clock Frequency: Nominal              | 2.93 GHz                | 2.86 GHz
| Clock Frequency: Max. (Turbo)         | 3.49 GHz                | 4.03 GHz
| Memory                                | 128 GB                  | 128 GB
| Memory Controllers                    | 2                       | 2
| Memory Channels                       | 4                       | 4
| L3 Cache                              | 8 MB                    | 8 MB
| L4 Cache                              | 16 MB / Buffer Chip     | 16 MB / Buffer Chip
| Memory Bandwidth                      | 230 GB/s                | 230 GB/s
| Peak Performance (Double Precision)   | 235 GFlops              | 235 GFlops
| \# GPUs                               | 2                       | 2 / 4
| Model                                 | Tesla K80               | Tesla P100
| CUDA Cores                            | 4992                    | 3584
| Memory                                | 24 GB                   | 16 GB
| Peak Performance (Double Precision)   | 1.87 TFlops             | 5.3 TFlops

## Simultaneous Multi-Threading (SMT)

The S822LC compute nodes can be configured in 4 distinct SMT modes.  
Each SMT mode defines the number of logical cores (hardware threads) available on the node:

| SMT Mode | \# Logical Cores
|:--------:|:----------------:|
| 1 (Off)  | 20
| 2        | 40
| 4        | 80
| 8        | 160

> SMT mode configuration does not require the system to be restarted; therefore, SMT mode configuration can be performed on-the-fly.
