cmake_minimum_required(VERSION 2.8)
project(libuv C)

set(LIBUV_MAJOR_VERSION 0)
set(LIBUV_MINOR_VERSION 1)
set(LIBUV_PATCH_VERSION 0)
set(LIBUV_VERSION
  ${LIBUV_MAJOR_VERSION}.${LIBUV_MINOR_VERSION}.${LIBUV_PATCH_VERSION})

# Offer the user the choice of overriding the installation directories
set(INSTALL_LIB_DIR lib CACHE PATH "Installation directory for libraries")
set(INSTALL_BIN_DIR bin CACHE PATH "Installation directory for executables")
set(INSTALL_INCLUDE_DIR include CACHE PATH
  "Installation directory for header files")
if(WIN32 AND NOT CYGWIN)
  set(DEF_INSTALL_CMAKE_DIR CMake)
else()
  set(DEF_INSTALL_CMAKE_DIR lib/CMake/libuv)
endif()
set(INSTALL_CMAKE_DIR ${DEF_INSTALL_CMAKE_DIR} CACHE PATH
  "Installation directory for CMake files")

# Make relative paths absolute (needed later on)
foreach(p LIB BIN INCLUDE CMAKE)
  set(var INSTALL_${p}_DIR)
  if(NOT IS_ABSOLUTE "${${var}}")
    set(${var} "${CMAKE_INSTALL_PREFIX}/${${var}}")
  endif()
endforeach()

set(libuv_INCLUDE_DIR, ${PROJECT_SOURCE_DIR}/include)

# set up include-directories
include_directories( ${libuv_INCLUDE_DIR} )

add_subdirectory(src)

# The interesting stuff goes here
# ===============================

# Add all targets to the build-tree export set
export(TARGETS libuv
  FILE "${PROJECT_BINARY_DIR}/libuvTargets.cmake")

# Export the package for use from the build-tree
# (this registers the build-tree with a global CMake-registry)
export(PACKAGE libuv)

# Create the libuvConfig.cmake and libuvConfigVersion files
file(RELATIVE_PATH REL_INCLUDE_DIR "${INSTALL_CMAKE_DIR}"
   "${INSTALL_INCLUDE_DIR}")
# ... for the build tree
set(CONF_INCLUDE_DIRS "${PROJECT_SOURCE_DIR}" "${PROJECT_BINARY_DIR}")
configure_file(libuvConfig.cmake.in
  "${PROJECT_BINARY_DIR}/libuvConfig.cmake" @ONLY)
# ... for the install tree
set(CONF_INCLUDE_DIRS "\${LIBUV_CMAKE_DIR}/${REL_INCLUDE_DIR}")
configure_file(libuvConfig.cmake.in
  "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/libuvConfig.cmake" @ONLY)
# ... for both
configure_file(libuvConfigVersion.cmake.in
  "${PROJECT_BINARY_DIR}/libuvConfigVersion.cmake" @ONLY)

# Install the libuvConfig.cmake and libuvConfigVersion.cmake
install(FILES
  "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/libuvConfig.cmake"
  "${PROJECT_BINARY_DIR}/libuvConfigVersion.cmake"
  DESTINATION "${INSTALL_CMAKE_DIR}" COMPONENT dev)

# Install the export set for use with the install-tree
install(EXPORT libuvTargets DESTINATION
  "${INSTALL_CMAKE_DIR}" COMPONENT dev)
