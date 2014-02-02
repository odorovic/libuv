
cmake_minimum_required(VERSION 2.6)


# Ensures that we do an out of source build
MACRO(MACRO_ENSURE_OUT_OF_SOURCE_BUILD MSG)
    STRING(COMPARE EQUAL "${CMAKE_SOURCE_DIR}"
"${CMAKE_BINARY_DIR}" insource)
    GET_FILENAME_COMPONENT(PARENTDIR ${CMAKE_SOURCE_DIR} PATH)
    STRING(COMPARE EQUAL "${CMAKE_SOURCE_DIR}"
"${PARENTDIR}" insourcesubdir)
    IF(insource OR insourcesubdir)
        MESSAGE(FATAL_ERROR "${MSG}")
    ENDIF(insource OR insourcesubdir)
ENDMACRO(MACRO_ENSURE_OUT_OF_SOURCE_BUILD)

project(libuv)

set(CMAKE_MODULE_PATH ${libuv_SOURCE_DIR}/cmake/modules ${CMAKE_MODULE_PATH})

macro_ensure_out_of_source_build("${PROJECT_NAME} requires an out of source build.")


include_directories(${libuv_SOURCE_DIR}/include)

add_subdirectory(src)

