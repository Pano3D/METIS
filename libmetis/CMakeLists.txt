# Add this directory for internal users.
include_directories(.)
# Find sources.
file(GLOB metis_sources *.c)
# Build libmetis.
add_definitions(-fPIC)
add_library(metis ${METIS_LIBRARY_TYPE} ${GKlib_sources} ${metis_sources})
if(UNIX)
  target_link_libraries(metis PRIVATE m)
endif()

if(WIN32)
	set_target_properties(metis PROPERTIES
		PREFIX ""
		COMPILE_FLAGS /w
		RUNTIME_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}/../../../bin")
endif()

if (APPLE)
	set_target_properties(metis PROPERTIES
		INSTALL_NAME_DIR
		"${CMAKE_INSTALL_PREFIX}/lib")
 endif()

install(TARGETS metis EXPORT METIS-exports
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
