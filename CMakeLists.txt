# (C) Copyright 2018 UCAR.
#
# This software is licensed under the terms of the Apache Licence Version 2.0
# which can be obtained at http://www.apache.org/licenses/LICENSE-2.0.

################################################################################
# MOM6
################################################################################

cmake_minimum_required( VERSION 3.12)
project( mom6 VERSION 2020.4.0 LANGUAGES Fortran )

# ecbuild integration
find_package(ecbuild 3.3.2 REQUIRED)
include( ecbuild_system NO_POLICY_SCOPE )
ecbuild_declare_project()

list( APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
include( mom6_compiler_flags )

# ocean_bgc option
option( ENABLE_OCEAN_BGC "Compile and use ocean bgc modules in MOM6")


################################################################################
# Dependencies
################################################################################

find_package(OpenMP COMPONENTS C Fortran)
find_package(MPI REQUIRED COMPONENTS C Fortran)
find_package(NetCDF REQUIRED COMPONENTS Fortran )
find_package(fms 2020.4.0 REQUIRED )
find_package(gsw 3.0.5 REQUIRED )


################################################################################
# Sources
################################################################################

set( MOM6_LINKER_LANGUAGE Fortran )

# mom6 library
#-------------------------------------------------------------------------------
add_subdirectory( config_src )
add_subdirectory( src )

list( APPEND mom6_src_files
    ${config_src_files}
    ${src_files}
)

if (${ENABLE_OCEAN_BGC})
    add_definitions( -D_USE_GENERIC_TRACER )
    add_subdirectory( pkg/ocean_BGC )
    list( APPEND mom6_src_files ${ocean_bgc_src_files} )
endif()

ecbuild_add_library( TARGET   mom6
                     SOURCES  ${mom6_src_files}
                     INSTALL_HEADERS LISTED
                     LINKER_LANGUAGE ${MOM6_LINKER_LANGUAGE}
                    )

# target_include_directories( mom6 PRIVATE
#    ${CMAKE_Fortran_MODULE_DIRECTORY} )

target_link_libraries(mom6 PUBLIC NetCDF::NetCDF_Fortran)
target_link_libraries(mom6 PUBLIC MPI::MPI_Fortran)
target_link_libraries(mom6 PUBLIC OpenMP::OpenMP_C OpenMP::OpenMP_Fortran)
target_link_libraries(mom6 PUBLIC fms)
target_link_libraries(mom6 PUBLIC gsw)

# Fortran module output directory for build and install interfaces
set(MODULE_DIR module/${PROJECT_NAME}/${CMAKE_Fortran_COMPILER_ID}/${CMAKE_Fortran_COMPILER_VERSION})
set_target_properties(${PROJECT_NAME} PROPERTIES Fortran_MODULE_DIRECTORY ${CMAKE_BINARY_DIR}/${MODULE_DIR})
install(DIRECTORY ${CMAKE_BINARY_DIR}/${MODULE_DIR}/ DESTINATION ${MODULE_DIR})
target_include_directories(${PROJECT_NAME} INTERFACE
                            $<BUILD_INTERFACE:${CMAKE_BINARY_DIR}/${MODULE_DIR}>
                            $<INSTALL_INTERFACE:${MODULE_DIR}>)

# Installed .h include locations
target_include_directories(mom6 PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/config_src/memory/dynamic_nonsymmetric>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src/framework>
  $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)


# mom6.x executable
#-------------------------------------------------------------------------------
set ( mom6_exe_files
    config_src/drivers/solo_driver/MOM_driver.F90
)

ecbuild_add_executable( TARGET   mom6.x
                        SOURCES  ${mom6_exe_files}
                        LIBS     mom6
                        LINKER_LANGUAGE ${MOM6_LINKER_LANGUAGE}
                       )


################################################################################
# Finalise configuration
################################################################################

ecbuild_install_project( NAME mom6 )
ecbuild_print_summary()
