# Something (bugs) has to be noticed

- [Something (bugs) has to be noticed](#something-bugs-has-to-be-noticed)
  - [Common issues for all versions](#common-issues-for-all-versions)
  - [ROMS37 version with bio module modification](#roms37-version-with-bio-module-modification)
  - [ROMS32 version with bio module modification](#roms32-version-with-bio-module-modification)

## Common issues for all versions

- not enough memory  
  
```bash
=============================================================================
=   BAD TERMINATION OF ONE OF YOUR APPLICATION PROCESSES
=   RANK 0 PID 20373 RUNNING AT dn037
=   KILLED BY SIGNAL: 9 (Killed)
=============================================================================
```

- note the reference time in all input “\*.nc file and in the "\*.in" file
  
- varinfo.dat ( do not confuse the different versions)

- error when reading '*\*.in*'  
  This error may occurs when using the new PGI (nvfortran) compiler.  
  ![image](https://github.com/ELVIS-CHING/ROMS_related/assets/62006950/dcd58cb6-7f87-4dda-936d-fb4c032536aa)

  ```bash
  vi *.in
  ```
  and past the following command  
  ```bash
   :set fileformat=unix
  ```

## ROMS37 version with bio module modification

- in one version code, "indx" should be "ibio" in fennel.h of line 1726, 1740, and 1741  

## ROMS32 version with bio module modification

- must define the diagnostic output
  
- 'NDIA' in '*.in' file must be a nonzero value error  
  >   forrtl: severe (71): integer divide by zero

- 'RIVER_OM' : if you cannot undef this mode
  >   ROMS/Unility/inp_par.F: 3764 lost ifdef RIVER_OM

- in a version with module "RIVER_OM" added, the following code is reapted in Line 1686 and 1871, the latter one should be deleted  
```fortran
Bio(i,1,indx)=Bio(i,1,indx)+cff1_wc
```

![image](https://github.com/ELVIS-CHING/ROMS_related/assets/149153513/bb7635a0-4fbb-4582-a1ce-a9ffec8096f3)
