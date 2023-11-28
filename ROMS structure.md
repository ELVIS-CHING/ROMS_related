# program sturcture  

(routine name -> file name containing the routine)

## OCEAN -> master.F

### CALL ROMS_initialize -> ocean_control.F

CALL initial -> initial.F

> CALL meterics -> meterics.F

### CALL ROMS_run -> ocean_control.F

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
>>
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
### CALL ROMS_finalize -> ocean_control.F
>
