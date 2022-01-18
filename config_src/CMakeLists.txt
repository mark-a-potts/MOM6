list ( APPEND _files
config_src/memory/dynamic_nonsymmetric/MOM_memory.h
config_src/external/ODA_hooks/kdtree.f90
config_src/drivers/FMS_cap/MOM_surface_forcing_gfdl.F90
config_src/drivers/FMS_cap/ocean_model_MOM.F90
config_src/drivers/solo_driver/atmos_ocean_fluxes.F90
config_src/drivers/solo_driver/MESO_surface_forcing.F90
config_src/drivers/solo_driver/MOM_driver.F90
config_src/drivers/solo_driver/MOM_surface_forcing.F90
config_src/drivers/solo_driver/user_surface_forcing.F90
config_src/external/drifters/MOM_particles.F90
config_src/external/drifters/MOM_particles_types.F90
config_src/external/ODA_hooks/ocean_da_core.F90
config_src/external/ODA_hooks/ocean_da_types.F90
config_src/external/ODA_hooks/write_ocean_obs.F90
config_src/infra/FMS1/MOM_coms_infra.F90
config_src/infra/FMS1/MOM_constants.F90
config_src/infra/FMS1/MOM_couplertype_infra.F90
config_src/infra/FMS1/MOM_cpu_clock_infra.F90
config_src/infra/FMS1/MOM_data_override_infra.F90
config_src/infra/FMS1/MOM_diag_manager_infra.F90
config_src/infra/FMS1/MOM_domain_infra.F90
config_src/infra/FMS1/MOM_ensemble_manager_infra.F90
config_src/infra/FMS1/MOM_error_infra.F90
config_src/infra/FMS1/MOM_interp_infra.F90
config_src/infra/FMS1/MOM_io_infra.F90
config_src/infra/FMS1/MOM_time_manager.F90
)

if ( NOT ENABLE_OCEAN_BGC )
    list ( APPEND _files
    config_src/external/GFDL_ocean_BGC/FMS_coupler_util.F90
    config_src/external/GFDL_ocean_BGC/generic_tracer.F90
    config_src/external/GFDL_ocean_BGC/generic_tracer_utils.F90
    )
endif ()

set ( config_src_files
      ${_files}
      PARENT_SCOPE
)
