# turn on the functions in "\*.h" or "*.in" file  

There might be some differences in different versions of ROMS code. We may take ROMS3.7 as an example, and mark the version if it is for another version.

----------------------------------------------

## tidal forcing  

define the following options in "*.h" file

> \# define SSH_TIDES  
> \# define UV_TIDES  
> \# define RAMP_TIDES (not necessary but better to have it)  

## Coriolis force  

Sometimes, we can configure the idealized case to have some experiments. Removing the Coriolis force makes things more simple.  
There are two ways to do this:  

> (1) set f = 0.0 in the input grid file  
> (2) remove the "\# define UV_COR" in "*.h" file  

## about the particle sinking  

This is try to keep the vertical sinking process, but not deposits to the seabed.
This may help check the vertical sinking process of sediment or the BGC particle materials.
Add the following code 

```fortran
DO i=Istr,Iend
   FC(i,0) = 0.0_r8
END DO
```

in "Fennel.h"  
![1699622856735](https://github.com/ELVIS-CHING/ROMS_related/assets/62006950/cdedf157-6edf-4478-9ae6-60b8e888399a)


in "sed_settling.F"  
![image](https://github.com/ELVIS-CHING/ROMS_related/assets/62006950/c39f0124-20f2-429b-89da-083e326a8336)


## about bulk flux 

if def BULK_FLUX; should give wind speed (not wind stress) in forcing file;
if undef BULK_FLUX; should give wind stress(not wind speed) in forcing file.
