cmake_minimum_required(VERSION 3.10)

project(libhilbert
        VERSION 1.0 DESCRIPTION "Library for N-Dimensional Hilbert Mapping"
        LANGUAGES C)

set(LIBHILBERT_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include)
set(LIBHILBERT_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)

add_library(libhilbert
            # Headers
            # ${LIBHILBERT_INCLUDE_DIR}/N10.h
            # ${LIBHILBERT_INCLUDE_DIR}/binaryOps.h
            # ${LIBHILBERT_INCLUDE_DIR}/hilbertKey.h
            # Source files
            ${LIBHILBERT_SOURCE_DIR}/binaryOps.c
            ${LIBHILBERT_SOURCE_DIR}/hilbertKey.c)

add_library(libhilbert::libhilbert ALIAS libhilbert) # Library alias
set_target_properties(libhilbert PROPERTIES PREFIX "")

# Handle include paths
target_include_directories(
  libhilbert
  PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
         $<INSTALL_INTERFACE:include>
  PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src)

# Modify target compiler properties
target_compile_features(libhilbert
                        PUBLIC
                        c_std_99 # Require c99
                        )

target_compile_options(libhilbert
                       PRIVATE $<$<C_COMPILER_ID:GNU>:-O2> # Enable optimisation
                                                           # flag for GNU
                                                           # compilers
                       )

add_executable(hilbertTest src/main.c)
target_link_libraries(hilbertTest libhilbert m)
