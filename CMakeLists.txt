cmake_minimum_required(VERSION 3.22)
project(METIS C)

set(SHARED FALSE CACHE BOOL "build a shared library")

if(0)
  set(METIS_INSTALL FALSE)
else()
  set(METIS_INSTALL TRUE)
endif()

# Configure libmetis library.
if(SHARED)
  set(METIS_LIBRARY_TYPE SHARED)
else()
  set(METIS_LIBRARY_TYPE STATIC)
endif(SHARED)

include(./conf/gkbuild.cmake)

# METIS' custom options
option(IDX64 "enable 64 bit ints" On)
#option(REAL64 "enable 64 bit floats (i.e., double)" OFF)
if(IDX64)
 set(METIS_COPTIONS "${METIS_COPTIONS} -DIDXTYPEWIDTH=64")
else()
 set(METIS_COPTIONS "${METIS_COPTIONS} -DIDXTYPEWIDTH=32")
endif(IDX64)
#if(REAL64)
#  set(METIS_COPTIONS "${METIS_COPTIONS} -DREALTYPEWIDTH=64")
#else()
#  set(METIS_COPTIONS "${METIS_COPTIONS} -DREALTYPEWIDTH=32")
#endif(REAL64)
#
#set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${METIS_COPTIONS}")


# Add include directories.
# i.e., the -I equivalent


# List of paths that the compiler will search for library files.
# i.e., the -L equivalent


# Recursively look for CMakeLists.txt in subdirs.
add_subdirectory("include")
add_subdirectory("libmetis")

install(EXPORT metisTargets FILE "metisTargets.cmake" DESTINATION "share/metis")
file(WRITE "${CMAKE_INSTALL_PREFIX}/share/metis/metisConfig.cmake" [=[
  include(CMakeFindDependencyMacro)
  find_dependency(GKlib CONFIG)
  include("${CMAKE_CURRENT_LIST_DIR}/metisTargets.cmake")
]=])
