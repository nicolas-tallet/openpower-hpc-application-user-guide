# IBM Spectrum MPI

## Invocation

The invocation commands of the MPI wrappers are the following:

| C     | C++    | Fortran
|:-----:|:------:|:-------:|
| mpicc | mpicxx | mpifort |
|       | mpic++ |         |

> The following Fortran wrappers are deprecated as of Open MPI v1.7, and replaced by `mpifort`:
> * `mpif77`
> * `mpif90`

> The following options allow to make the wrapper command explicit:
> * `-show`
> * `--showme:compile`: Flags for compilation
> * `--showme:link`: Flags for linking
>   These options are very useful to confirm, for instance, that the expected compiler is the one actually invoked.

Example: Checking the compiler and associated libraries called by the MPI Wrapper:
```
$ mpicc -show
xlc_r -I/opt/ibm/spectrum_mpi/include -pthread -L/opt/ibm/spectrum_mpi/lib -lmpi_ibm
```

## MPI Wrappers Customization

For each MPI wrapper, the following environment variables provide a way to customize the wrapper configuration:

| Wrapper Compiler | Compiler  | Preprocessor Flags | Compiler Flags | Linker Flags  | Linker Library Flags
|:----------------:|:--------: |:------------------:|:--------------:|:-------------:|:--------------------:|
| mpicc            | OMPI_CC   | OMPI_CPPFLAGS      | OMPI_CFLAGS    | OMPI_LDFLAGS  | OMPI_LIBS
| mpicxx           | OMPI_CXX  | OMPI_CPPFLAGS      | OMPI_CXXFLAGS  | OMPI_LDFLAGS  | OMPI_LIBS
| mpifort          | OMPI_FC   | OMPI_CPPFLAGS      | OMPI_FCFLAGS   | OMPI_LDFLAGS  | OMPI_LIBS

## CUDA-Awareness

The IBM Spectrum MPI library is actually CUDA-aware, as indicated in the official product documentation.  
However:

* GPU support is disabled by default, and must be explicitely enabled through the ‘-gpu’ option.
* GPU support has a certain number of limitations at this time. The official product documentation states:
  * Support for GPU-accelerated applications is provided only if you are using the IBM Spectrum MPI PAMI backend and the IBM collective library \(libcollectives\). These are the default options for IBM Spectrum MPI, but if you choose to use a different messaging protocol or collective component, note that it will not support GPUs.
  * The libcollectives component is the only collective component that is able to support CUDA buffers. As a result, when -gpu is specified with the mpirun command, libcollectives must be the preferred collective component. Therefore, you cannot specify `mpirun -gpu` with any of the following options:
  * `-mxm/-MXM`
  * `-mxmc/-MXMC`
  * `-tcp/-TCP`
  * `-ibv/-IBV`
  * `-ib/-IB`
  * `-openib/-OPENIB`
  * `-fca/-FCA`
  * `-hcoll/-HCOLL`
  * The following MPI functions are not CUDA-aware:
  * MPI\_Alltoallw
  * MPI\_Ialltoallw
  * MPI\_Ineighbor\_allgather
  * MPI\_Ineighbor\_allgatherv
  * MPI\_Ineighbor\_alltoall
  * MPI\_Ineighbor\_alltoallv
  * MPI\_Ineighbor\_alltoallw
  * MPI\_Neighbor\_allgather
  * MPI\_Neighbor\_allgatherv
  * MPI\_Neighbor\_alltoall
  * MPI\_Neighbor\_alltoallv
  * MPI\_Neighbor\_alltoallw
  * IBM Spectrum MPI behavior is undefined when an MPI application passes a GPU device address to an MPI API that is not CUDA aware; the application might trigger a segmentation fault and dump core, or it might give incorrect results.
  * Most MPI APIs are CUDA-aware, however, IBM Spectrum MPI does not supply header files or Fortran module files that identify which MPI APIs accept GPU device addresses as arguments. Refer to the preceding restriction for a list of MPI APIs that may not be used with GPU device addresses.
