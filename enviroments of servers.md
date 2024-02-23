# About how to set the environments on different servers

The environments for running the ROMS model include the Fortran compiler, parallel computing tool (mpi), and NetCDF library (sometimes also needs HDF5).
The environment settings can be different on different servers. Usually, we use intel fortran (ifort) as the compiler; either mpich or OpenMPI is OK; and NetCDF4.

The environment settings should be specified before compiling the code, and may also need to be specified in the submitting script on some servers.  

* The Fortran compiler and mpi are specified in 'build.bash' (or 'makefile', refer to [steps to run a model](/steps%20to%20run%20a%20model.md))
* The path of NetCDF should be specified in '*Linux-ifort.mk*' (if ifort is used) in the "Compilers" directory.

The new HPCs usually use [Lmod](https://lmod.readthedocs.io/en/latest/index.html#) to manage the software packages. With Lmod, we can set up the environments easily by adding the modules in '\~/.bash_profile' or '~/.bashrc'. Every time log in to the server, it will load the settings in '\~/.bash_profile' automatically. Or use the command 'source ~/.bash_profile' to update the settings after modifying it.  
Some basic useful commands can be found in this [official link](https://lmod.readthedocs.io/en/latest/010_user.html), or this [simple introduction](https://qingli411.github.io/eesrf-hpc-user-guide/hpc1/environment.html) provided by Prof. Li Qing.  

**For using GPU, nvfortran is needed.** On most servers, we may need to set up MPI and NetCDF for nvfortran by ourself. Instructions are shown [here](/setting%20up%20nvfortran%20environment.md).  

## Check the environments (take hpc3 for example)

* ifort

> use the command 'which ifort', then it will show the location of ifort
![image](https://github.com/ELVIS-CHING/ROMS_related/assets/62006950/408c297a-2421-493a-a0fc-66302977f61b)

* mpi

> use the command 'mpifort --version', or 'mpi90 --version' to make sure that it's built on ifort
![1699191567951](https://github.com/ELVIS-CHING/ROMS_related/assets/62006950/e0955d6a-1961-4e0b-86fb-71e4b748b728)

* NetCDF

> NetCDF4 has the 'nf-config' command (not in NetCDF3), use the command 'nf-config --all' to show the package's location.
![1699191837253](https://github.com/ELVIS-CHING/ROMS_related/assets/62006950/7838adbb-9419-447b-99f6-3b5ab634f64a)

## Servers that are using

* hpc3  
**ifort + openmpi + NetCDF4**  
add the following commands in '\~/.bash_profile'  

> module swap gnu8/8.3.0 intel/19.1.1.217  
> module load netcdf-fortran/4.5.2 netcdf-cxx/4.3.1 netcdf/4.7.1  
> module load cmake/3.15.4  

in 'Linux-ifort.mk'  
location of NetCDF:
> ![image](https://github.com/ELVIS-CHING/ROMS_related/assets/62006950/b85fe4a2-731a-4516-9058-007340770dba)

```
ifdef USE_NETCDF4
#hpc3
    NETCDF_INCDIR ?= /opt/ohpc/pub/libs/intel/openmpi3/netcdf-fortran/4.5.2/include
    NETCDF_LIBDIR ?= /opt/ohpc/pub/libs/intel/openmpi3/netcdf-fortran/4.5.2/lib
      HDF5_LIBDIR ?= /opt/ohpc/pub/libs/intel/openmpi3/hdf5/1.10.5/lib
             LIBS := -L$(NETCDF_LIBDIR) -lnetcdff 
else
#hpc3
    NETCDF_INCDIR ?= /opt/ohpc/pub/libs/intel/openmpi3/netcdf-fortran/4.5.2/include
    NETCDF_LIBDIR ?= /opt/ohpc/pub/libs/intel/openmpi3/netcdf-fortran/4.5.2/lib
             LIBS := -L$(NETCDF_LIBDIR) -lnetcdff
endif
```

* hkust_gz

```
ifdef USE_NETCDF4
# hkustgz
    NETCDF_INCDIR ?= public/apps/module/intel-2023_2/openmpi-4_1/netcdf/4.9.2/include
    NETCDF_LIBDIR ?= public/apps/module/intel-2023_2/openmpi-4_1/netcdf/4.9.2/lib
      HDF5_LIBDIR ?= /opt/intelsoft/hdf5/lib
             LIBS += -L$(HDF5_LIBDIR) -lhdf5_hl -lhdf5 -lz
else
#hkustgz
    NETCDF_INCDIR ?= /public/apps/module/intel-2023_2/openmpi-4_1/netcdf/4.9.2/include
    NETCDF_LIBDIR ?= /public/apps/module/intel-2023_2/openmpi-4_1/netcdf/4.9.2/lib
             LIBS := -L$(NETCDF_LIBDIR) -lnetcdff -lnetcdf -lm
endif
```

* Tianjin
* GZ_gjp
  
* GZ_juno GZ_bl  
**ifort + openmpi + NetCDF4**  
add the following commands in '\~/.bash_profile'  

> module load gcc/9.2.0  
> module load intel/18.0.1  
> module load openmpi/3.1.4-icc-15.0.1  
> module load netcdf/4.5.0-icc-15  
> module load hdf5/1.8.21-icc-15

* hqlx24  
The hqlx servers can be a little complicated. They cannot load the '\~/.bashrc' automatically. You have to 'source ~/.bashrc' after login.

**ifort + openmpi + NetCDF4**  
add the following commands in '\~/.bashrc'  
> export INTEL_LICENSE_FILE=/usr/local/intel-oneapi/licenses/COM_LMSN-HGN8WF5P.lic  
> export PATH=/usr/local/netcdf4-intel/bin:$PATH  
> export LD_LIBRARY_PATH=/usr/local/netcdf4-intel/lib:$LD_LIBRARY_PATH  
> export PATH=/usr/local/intel-oneapi/mpi/latest/bin:$PATH  
> export LD_LIBRARY_PATH=/usr/local/intel-oneapi/mpi/latest/lib:$LD_LIBRARY_PATH  
> export I_MPI_F90=ifort

first  
> source ~/.bashrc

then  
> source /usr/local/intel-oneapi/setvars.sh

![1699242333562](https://github.com/ELVIS-CHING/ROMS_related/assets/62006950/2306fb04-6136-43da-b321-f53ac127df66)

in 'Linux-ifort.mk'  
![image](https://github.com/ELVIS-CHING/ROMS_related/assets/62006950/4bb73bbd-329a-4fe6-b945-dbb8e163f9f1)

* hqlx42/hqlx43
  
