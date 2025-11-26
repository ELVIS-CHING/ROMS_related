# Something (bugs) has to be noticed

- [Something (bugs) has to be noticed](#something-bugs-has-to-be-noticed)
  - [Common issues for all versions](#common-issues-for-all-versions)
  - [ROMS37 version with bio module modification](#roms37-version-with-bio-module-modification)
  - [ROMS32 version with bio module modification](#roms32-version-with-bio-module-modification)

## Common issues for all versions

- About the tracer volume flux
  The original program for tracer volume flux output is (ROMS37 code, `set_avg.F`),

```fortran
                  AVERAGE(ng)%avgHuonT(i,j,k,it)=0.5_r8*                &
     &                                           GRID(ng)%Huon(i,j,k)*  &
     &        (OCEAN(ng)%t(i-1,j,k,Nout,it)+ OCEAN(ng)%t(i,j,k,Nout,it))
```  
  
  which can not match the diagnostic advection term. It is better to use the flux calculated in `step3d_t.F`
  
```fortran
!
!  Time-step corrected horizontal advection (Tunits m).
!
          DO j=Jstr,Jend
            DO i=Istr,Iend
              cff=dt(ng)*pm(i,j)*pn(i,j)
              cff1=cff*(FX(i+1,j)-FX(i,j))
              cff2=cff*(FE(i,j+1)-FE(i,j))
              cff3=cff1+cff2
              t(i,j,k,nnew,itrc)=Ta(i,j,k,itrc)*Hz(i,j,k)-cff3
!! CHENG WC, 2025-11-21, START
              uflux(i,j,k,itrc)=FX(i,j)
              vflux(i,j,k,itrc)=FE(i,j)
!! CHENG WC, 2025-11-21, END
#  ifdef DIAGNOSTICS_TS
              DiaTwrk(i,j,k,itrc,iTxadv)=DiaTwrk(i,j,k,itrc,iTxadv)-    &
     &                                   cff1
              DiaTwrk(i,j,k,itrc,iTyadv)=DiaTwrk(i,j,k,itrc,iTyadv)-    &
     &                                   cff2
              DiaTwrk(i,j,k,itrc,iThadv)=DiaTwrk(i,j,k,itrc,iThadv)-    &
     &                                   cff3
#  endif
            END DO
          END DO
```

  and modify the code in `set_avg.F`

```fortran
                  AVERAGE(ng)%avgHuonT(i,j,k,it)=uflux(i,j,k,it)
```

With this modification, the layer-integrated domain-sum advection term is still not able to match the flux through the boundaries, which is related to the sigma coordination (refer to the image below). For the whole water column integration, these two terms can match well.  
<img width="956" height="1106" alt="image" src="https://github.com/user-attachments/assets/a0f7413d-a3a4-4425-b886-83b7139a4a7d" />

- Values of output  

  For some output values, they are scaled in `wrt_avg.F` or `wrt_diags.F`.
  For example, the  diagnostic terms are multiplied by the time step `dt`, but the unit in the output nc file is $$s^{-1}$$. That's because in `wrt_diags.F`, all the terms are scaled by

```fortran
            scale=1.0_r8/dt(ng)
```

when they are written to the file.

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
