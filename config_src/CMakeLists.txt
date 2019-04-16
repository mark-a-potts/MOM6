list ( APPEND _files
config_src/dynamic/MOM_memory.h
config_src/solo_driver/atmos_ocean_fluxes.F90
config_src/solo_driver/coupler_types.F90
config_src/solo_driver/coupler_util.F90
config_src/solo_driver/MESO_surface_forcing.F90
config_src/solo_driver/MOM_surface_forcing.F90
config_src/solo_driver/Neverland_surface_forcing.F90
config_src/solo_driver/user_surface_forcing.F90
)

set ( config_src_files
      ${_files}
      PARENT_SCOPE
)
