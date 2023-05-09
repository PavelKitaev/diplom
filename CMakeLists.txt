cmake_minimum_required( VERSION 3.2 )

project(diplom)
message( STATUS "Solution of the Dirichlet problem for the Poisson equation" )
include( "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Configure.cmake" )

################################ MPI ################################
option(USE_MPI ON)
if( USE_MPI )
    find_package( MPI )
    if( MPI_FOUND )
        include_directories( ${MPI_INCLUDE_PATH} )
    else( MPI_FOUND )
        set( USE_MPI OFF )
    endif( MPI_FOUND )
endif( USE_MPI )

############################### OpenMP ##############################
option(USE_OMP ON)
if( USE_OMP OR USE_SEQ )

    find_package(OpenMP REQUIRED)

    if( OpenMP_CXX_FOUND )
        message(STATUS  "OpenMP_CXX_FOUND DETECTED!")
        include_directories( ${OpenMP__CXX_INCLUDE_DIRS} )
    else( OpenMP_CXX_FOUND )
	message(STATUS  "OpenMP_CXX_FOUND NOT DETECTED!")
        set( USE_OMP OFF )
    endif( OpenMP_CXX_FOUND )
endif( USE_OMP OR USE_SEQ )

############################ std::thread ############################
option(USE_STD ON)
if( USE_STD )
    find_package( Threads )
endif( USE_STD )

################################ TBB ################################
option(USE_TBB ON)
if( USE_TBB )
    include(cmake/onetbb.cmake)
endif( USE_TBB )

############################## Modules ##############################
include_directories(3rdparty/unapproved)
add_subdirectory(modules)