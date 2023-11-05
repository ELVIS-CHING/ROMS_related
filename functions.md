# turn on the functions in '*.h' or '*.in' file  
There might be some differences in different versions of ROMS code. We may take ROMS3.7 as an example, and mark the version if it is for another version. 

* include tidal forcing
define the following options in '*.h' file
> \# define SSH_TIDES  
> \# define UV_TIDES  
> \# define RAMP_TIDES (not necessary but better to have it)  
