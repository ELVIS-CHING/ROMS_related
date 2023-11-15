# turn on the functions in "\*.h" or "*.in" file  

There might be some differences in different versions of ROMS code. We may take ROMS3.7 as an example, and mark the version if it is for another version.

- [turn on the functions in "\*.h" or "\*.in" file](#turn-on-the-functions-in-h-or-in-file)
  - [tidal forcing](#tidal-forcing)
  - [Coriolis force](#coriolis-force)
  - [about the particle sinking](#about-the-particle-sinking)
  - [atmospheric forcing](#atmospheric-forcing)

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


## atmospheric forcing  

specify **wind stress** or use **BULK_FLUX**
- wind stress  
remember to include "#undef ANA_SMFLUX" or delete "#define ANA_SMFLUX " in the "*.h" file
there should be "sustr" and "svstr" for the wind stress in two directions in the forcing input file  

- BULK_FLUX  
remember to include "#define BULK_FLUX" in the "*.h" file  
can also define the following functions at the same time  
> \#define EMINUSP  
> \#define SOLAR_SOURCE  
> \#define LONGWAVE
 
the following variables are needed in the forcing input file (may refer to /Data/ROMS/CDL/frc_bulk.cdl):  
> surface u-wind component (**wind speed rather than wind stress**)  
> surface v-wind component (**wind speed rather than wind stress**)   
> surface air pressure  
> surface air temperature  
> surface air relative humidity  
> cloud fraction  
> solar shortwave radiation flux  
> rain fall rate  

references for the BULK_FLUX function  
- [Bulk parameterization of air-sea fluxes for Tropical Ocean-Global Atmosphere Coupled-Ocean Atmosphere Response Experiment](https://agupubs.onlinelibrary.wiley.com/doi/pdf/10.1029/95JC03205)  
- [Bulk Parameterization of Air–Sea Fluxes: Updates and Verification for the COARE Algorithm](https://journals.ametsoc.org/view/journals/clim/16/4/1520-0442_2003_016_0571_bpoasf_2.0.co_2.xml)

***notice***  
For the atmospheric forcing, you may specify data for one point or the whole grid.  
* data for one point  
  The whole grid will use the same forcing. In this case, wind stress (speed) is given on the eastward and northward direction. The model will rotate them to the $\xi$ and $\eta$ directions ([Curvilinear Coordinates](https://www.myroms.org/wiki/Curvilinear_Coordinates)) automatically.
* data for the whole grid  
  Values are given for each model grid. In this case, wind stress (speed) is given on the **$\xi$ and $\eta$ directions**.  
