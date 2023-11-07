# step to run a model

* Set up the environments  
  make sure the compiler, mpi, NetCDF are set  
  more details in [enviroments of servers](https://github.com/ELVIS-CHING/ROMS_related/blob/main/enviroments%20of%20servers.md)
  
* Configure the functions (turning on the different options in '*.h' file)  
  check the functions needed for you case  
  [functions.md](https://github.com/ELVIS-CHING/ROMS_related/blob/main/functions.md)
  
* Compile the code  
  you will get the 'oceanM' file and the 'Build_roms' directory  
  **oceanM**: used to run the model  
  **Build_roms**: The exact codes used for computation. If you have added some codes, check the codes in this directory if you have made it correctly.  
  
* Prepare the input files  
  There is an official toolbox for preparing the input '*.nc' files, but I cannot find the link now (maybe I will update later).  
  Or ask someone for that.

* Set the parameters and input/output information  
  If you turn on some functions in the '*.h' file, remember to set the things in '\*.in' file

* Run
  The scripts for submitting the job may be different on different servers. Some servers need to include the environments in the script.  
  The fundamental one is
  > mpirun -np number_of_cores ./oceanM *.in > logfile

  The 'logfile' records the running information, including the parameters, input and output information.  
