OCEAN -> master.F

	CALL ROMS_initialize -> ocean_control.F 
         CALL initial -> initial.F
			   CALL meterics -> meterics.F			
	CALL ROMS_run -> ocean_control.F
		   CALL main3d -> main3d.F
			   CALL output (174)
				   CALL def_his
					   CALL def_info
					   CALL wrt_info
	CALL ROMS_finalize -> ocean_control.F

   

