# Hardware Environment

## Technical Specifications

The IBM Power System compute nodes have the following technical characteristics:

| Commercial Name                         | IBM Power System<br>S822LC | IBM Power System<br>S822LC for HPC | IBM Power System<br>AC922
|:---------------------------------------:|:--------------------------:|:----------------------------------:|:--------------------------:|
| Model Type                              | 8335-GTA                   | 8335-GTB                           | 8335-GTH
| # Sockets                               | 2                          | 2                                  | 2
| CPU Model                               | POWER8                     | POWER8NV                           | POWER9
| Physical Cores                          | 20                         | 20                                 | 40
| Hardware Threads                        | 20/40/80/160               | 20/40/80/160                       | 40/80/160
| Clock Frequency: Nominal / Max. (Turbo) | 2.93 GHz / 3.49 GHz        | 2.86 GHz / 4.03 GHz                | 2.00 GHz / 2.87 GHz
| Peak Performance (Double Precision)     | 470 GFlops                 | 470 GFlops                         | 640 GFlops
| L3 Cache (Per Physical Core)            | 8 MB                       | 8 MB                               | 5 MB
| L4 Cache                                | 16 MB per Buffer Chip      | 16 MB per Buffer Chip              | -
| Memory Min. / Max.                      | 128 GB                     | 128 GB                             | 256 GB / 1 TB
| Memory Bandwidth                        | 230 GB/s                   | 230 GB/s                           | 340 GB/s
| # GPU Devices                           | 2                          | 2/4                                | 2/4/6
| GPU Model                               | NVIDIA Tesla K80           | NVIDIA Tesla P100                  | NVIDIA Tesla V100
| GPU Peak Performance (Double Precision) | 1.87 TFlops                | 5.3 TFlops                         | 7.5 TFlops
| GPU Memory                              | 24 GB                      | 16 GB                              | 16 GB
| GPU Memory Bandwidth                    | 480 GB/s                   | 732 GB/s                           | 900 GB/s

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
