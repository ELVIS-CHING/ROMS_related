# setting up MPI and NetCDF with nvfortran for ROMS  

This instruction takes hpc3 for example.  
After setting up all the packages, we can use nvfortran as pgi (modified '*Compilers/Linux-pgi.mk*' and set '*FORT = pgi*' in '*build.bash*').  

## load the nvfortran on server  

```bash
module load nvhpc/21.9
module load gnu8/8.3.0

module unload all the *netcdf-related packages*
```

## setting up MPI

find the MPI directory that included in nvfortran

```bash
which nvfortran
/opt/ohpc/pub/compiler/nvidia_hpc/Linux_x86_64/21.9/compilers/bin/nvfortran
```

then we can find MPI in  

```bash
/opt/ohpc/pub/compiler/nvidia_hpc/Linux_x86_64/21.9/comm_libs/mpi
```

add the following in '*~/.bashrc*' or '*~/.bash_profile*' (preferred)

```bash
MPI_HOME=/opt/ohpc/pub/compiler/nvidia_hpc/Linux_x86_64/21.9/comm_libs/mpi
export PATH=${MPI_HOME}/bin:$PATH
export LD_LIBRARY_PATH=${MPI_HOME}/lib:$LD_LIBRARY_PATH
export MANPATH=${MPI_HOME}/share/man:$MANPATH
```

run the following command

```bash
source ~/.bash_profile
or
source ~/.bashrc
```

## building NetCDF4 with nvfortran  

### set the directory for installation  

add the following in '*~/.bashrc*' or '*~/.bash_profile*' (preferred)  

```bash
NETCDF_HOME=/home/user/netcdf_nvfortran  # set you own directory for installation here
export PATH=${NETCDF_HOME}/bin:$PATH
export LD_LIBRARY_PATH=${NETCDF_HOME}/lib:$LD_LIBRARY_PATH
export LD_RUN_PATH=${NETCDF_HOME}/lib:$LD_RUN_PATH
```  

run the following command before installing each package

```bash
source ~/.bash_profile
or
source ~/.bashrc
```

4 package will be installed  

* szip  
* hdf5  
* netcdf-c  
* netcdf-fortran  

### szip  

```bash
wget https://support.hdfgroup.org/ftp/lib-external/szip/2.1.1/src/szip-2.1.1.tar.gz
tar xvaf szip-2.1.1.tar.gz
cd szip-2.1.1
mkdir build && cd build

../configure CC=nvc \
    --enable-shared \
    --enable-static \
    --prefix=$NETCDF_HOME
make -j
make install
cd ../..
```

### hdf5  

```bash
wget https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.13/hdf5-1.13.2/src/hdf5-1.13.2.tar.gz
tar xvaf hdf5-1.13.2.tar.gz
cd hdf5-1.13.2
mkdir build && cd build

../configure CC=mpicc FC=mpifort \
    CFLAGS="-fPIC -O1" \
    FCFLAGS="-fPIC -O1" \
    --enable-shared \
    --enable-static \
    --enable-fortran \
    --enable-hl \
    --enable-parallel \
    --with-zlib \
    --with-szlib=$NETCDF_HOME \
    --prefix=$NETCDF_HOME
make -j
make install
cd ../..
```

### netcdf-c  

```bash
wget https://github.com/Unidata/netcdf-c/archive/refs/tags/v4.9.0.tar.gz && mv v4.9.0.tar.gz netcdf-c-4.9.0.tar.gz
tar xvaf netcdf-c-4.9.0.tar.gz
cd netcdf-c-4.9.0
mkdir build && cd build

../configure CC=mpicc \
    CFLAGS="-fPIC -O1" \
    CPPFLAGS="-I$NETCDF_HOME/include" \
    LDFLAGS="-L$NETCDF_HOME/lib" \
    --enable-shared \
    --enable-static \
    --prefix=$NETCDF_HOME
make -j
make install
cd ../..
```

### netcdf-fortran  

```bash
wget https://github.com/Unidata/netcdf-fortran/archive/refs/tags/v4.6.0.tar.gz && mv v4.6.0.tar.gz netcdf-fortran-4.6.0.tar.gz
tar xvaf netcdf-fortran-4.6.0.tar.gz
cd netcdf-fortran-4.6.0
mkdir build && cd build

../configure CC=mpicc FC=mpifort \
    CFLAGS="-fPIC -O1 -I$NETCDF_HOME/include" \
    FCFLAGS="-fPIC -O1" \
    LDFLAGS="-L$NETCDF_HOME/lib -lnetcdf -lhdf5_hl -lhdf5" \
    --enable-shared \
    --enable-static \
    --prefix=$NETCDF_HOME
make
make install
```
