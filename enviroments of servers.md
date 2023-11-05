# about how to set the environments on different servers
The environments for running the ROMS model include the Fortran compiler, parallel computing tool (mpi), and NetCDF library (sometimes also needs HDF5).
The environment settings can be different on different servers. Usually, we use intel fortran (ifort) as the compiler; either mpich or OpenMPI is OK; and NetCDF4.

The environment settings should be specified before compiling the code, and may also need to be specified in the submitting script on some servers.  
* The Fortran compiler and mpi are specified in 'build.bash' (or 'makefile', refer to [steps to run a model](https://github.com/ELVIS-CHING/ROMS_related/blob/main/steps%20to%20run%20a%20model.md) )
* The path of NetCDF should be specified in 'Linux-ifort.mk' (if ifort is used) in the "Compilers" directory. 
    
## servers that are using
The new HPCs usually use [Lmod](https://lmod.readthedocs.io/en/latest/index.html#) to manage the software packages. With Lmod, we can set up the environments easily by adding the modules in '\~/.bash_profile' or '~/.bashrc'. Some basic useful commands can be found in this [official link](https://lmod.readthedocs.io/en/latest/010_user.html), or this [simple introduction[(https://qingli411.github.io/eesrf-hpc-user-guide/hpc1/environment.html) provided by Prof. Li Qing.

* hpc3
>  module swap gnu8/8.3.0 intel/19.1.1.217  
  module load netcdf-fortran/4.5.2 netcdf-cxx/4.3.1 netcdf/4.7.1  
  module load cmake/3.15.4  
* hkust_gz
* Tianjin
* GZ_gjp
* GZ_juno GZ_bl
* hqlx42/hqlx43
* hqlx24
