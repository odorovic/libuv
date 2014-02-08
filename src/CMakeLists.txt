#configure_file(config.h.in "${CMAKE_CURRENT_BINARY_DIR}/config.h" @ONLY)

include(sources.cmake)

add_library(libuv SHARED ${libuv_SOURCES})

set_target_properties(libuv PROPERTIES
  PUBLIC_HEADER "${libuv_INCLUDE_DIR}/uv.h;")

install(TARGETS libuv
  # IMPORTANT: Add the foo library to the "export-set"
  EXPORT Targets
  RUNTIME DESTINATION "${INSTALL_BIN_DIR}" COMPONENT bin
  LIBRARY DESTINATION "${INSTALL_LIB_DIR}" COMPONENT shlib
  PUBLIC_HEADER DESTINATION "${INSTALL_INCLUDE_DIR}/libuv"
    COMPONENT dev)
