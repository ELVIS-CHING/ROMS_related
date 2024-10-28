# program structure  

(routine name -> file name containing the routine)

## OCEAN -> master.F

### CALL ROMS_initialize -> Drivers/nl_ocean.h  

> CALL initialize_parallel -> Modules/mod_parallel.F  
> 
> CALL inp_par -> Utility/inp_par.F  
>> ***load the lateral boundary condition (LBC) information***  
>> CALL read_BioPar -> read_biopar.F (fennel_inp.h)  
>>> load_lbc -> inp_par.F
>>
>> CALL lbc_report -> lbc.F
> 
> CALL wclock_on  
> 
> CALL mod_arrays -> Modules/mod_arrays.F  
> 
> CALL initial -> initial.F  
>> CALL meterics -> meterics.F  

### CALL ROMS_run -> Drivers/nl_ocean.h

> CALL main3d -> main3d.F
>> ***get the forcing and boundary data***  
>> CALL get_data  
>> CALL set_data  
>>
>> ***initialized the field***  
>> CALL ini_zeta  
>> CALL set_depth  
>> CALL ini_fields  
>>
>> ***compute the mass flux***  
>> CALL set_massflux  
>> CALL rho_eos  
>>
>> ***vertical boundary condtions***  
>> CALL_bulk flux  
>> CALL set_vbc  
>> CALL set_tides  
>>
>> ***vertical velocity***  
>> CALL omega  
>> CALL wvelocity  
>>
>> ***set time-averaged***  
>> CALL set_zeta  
>> CALL set_diags  
>> CALL set_avg  
>>
>> ***write out***  
>> CALL output  
>>> CALL def_his  
>>>> CALL def_info  
>>>> CALL wrt_info  
>>>
>>> CALL wri_his
>>>> CALL netcdf_put_fvar  
>>>> CALL scale_omeg  
>>>> CALL netcdf_sync  
>>>
>>> CALL def_avg  
>>>> CALL netcdf_create  
>>>> CALL def_info  
>>>> CALL netcdf_enddef  
>>>> CALL wrt_info  
>>>> CALL netcdf_check_dim  
>>>> CALL netcdf_inq_var  
>>>> CALL netcdf_open  
>>>
>>***compute right-hand-side terms for 3D equation***  
>> CALL rhs3d  
>> CALL my_prestep  
>>
>> ***solve the vertically interated equations***  
>> CALL step2d  
>>
>> ***set depth and thickness with new time zeta***  
>> CALL set_depth  
>>
>> ***3D momentum equations***  
>> CALL step3d_uv  
>>
>> ***vertical velocity and mixing***  
>> CALL omega  
>> CALL my25_corstep  
>>
>> ***biology and sediment***  
>> CALL biology  
>> CALL sediment  
>>
>> ***tracer equation***  
>> CALL step3d_t  
>>

### CALL ROMS_finalize -> Drivers/nl_ocean.h

> CALL wrt_rst (ng)  
> 
> CALL wclock_off  
> 
> CALL close_out
