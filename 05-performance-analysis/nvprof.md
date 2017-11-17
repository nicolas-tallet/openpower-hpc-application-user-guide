# NVprof (NVIDIA Profiler)

## NVprof Execution Options

* Profiling Mode

| Option                 | Purpose |
|:----------------------:|:-------:|
| --events <events>      | Display Occurences for Specified Events                                 |
| --metrics <metrics>    | Display Values for Specified Metrics                                    |
| --print-api-trace      | Display Chronological Timeline of all CUDA Runtime and Driver API Calls |
| --print-gpu-trace      | Display Chronological Timeline of all GPU Activities                    |

* Profiling Scope

| Option                     | Purpose |
|:--------------------------:|:-------:|
| --print-summary-per-gpu    | Print One Summary per GPU Device               |
| --profile-api-trace none   | Turn API Trace Off (Reduce Profiling Overhead) |

## How-To: Reduce Profiling Overhead

Profiling overhead can prove to be quite heavy, generating latency in the code execution.

This can be avoided through the following guidelines:

* For MPI applications:
  * Limit profiling to one single MPI task or a limited set of specific MPI tasks
  * Do not profile multiple tasks per GPU

* Profiling Domain Explicit Specification
  Profiling domain can be reduced through an explicit specification:
  * Add the following calls into the source code: `cuProfilerStart()` & `cuProfilerStop()`
  * Use the following *NVprof* option: `--profile-from-start on`

* Disable profiling of the Unified Memory by using the following *NVprof* option: `--unified-memory-profiling off`

* Other *NVprof* parameters are enabled by default and can be disabled if necessary
