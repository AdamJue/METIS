# Add this directory for internal users.
include_directories(.)

# Find sources.
file(GLOB metis_sources *.c)

# Build libmetis.
add_library(metis ${METIS_LIBRARY_TYPE} ${metis_sources})
find_package(GKlib CONFIG REQUIRED)
target_link_libraries(metis PUBLIC GKlib)
target_include_directories(metis PRIVATE "../include")


install(TARGETS metis EXPORT metisTargets
    INCLUDES DESTINATION include)