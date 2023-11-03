# about how to set the environments on different servers
The environments for running the ROMS model include the Fortran compiler, parallel computing tool (mpi), and NetCDF library (sometimes also needs HDF5).
The environment settings can be different on different servers. Usually, we use intel fortran (ifort) as the compiler; either mpich or OpenMPI is OK; and NetCDF4.

The environment settings should be specified before compiling the code, and may also need to be specified in the submitting script on some servers.
The Fortran compiler and mpi are specified in 'build.bash' (or 'makefile', refer to [step to run a model] )

- [servers that are using](#servers-that-are-using)
  
## servers that are using

* hqlx42/hqlx43
* hqlx24
* hpc3
* hkust_gz
* Tianjin
* GZ_gjp
* GZ_juno GZ_bl

