
set(CMAKE_INCLUDE_CURRENT_DIR ON)

#set(libuv_SOURCES_COMMON
#    fs-poll.c
#    inet.c
#    uv-common.c
#    version.c)



set(libuv_SOURCES_COMMON
        fs-poll.c
        inet.c
        queue.h
        uv-common.c
        uv-common.h
        version.c)

set(libuv_SOURCES_UNIX_COMMON
        unix/async.c
        unix/core.c
        unix/dl.c
        unix/fs.c
        unix/getaddrinfo.c
        unix/loop.c
        unix/loop-watcher.c
        unix/pipe.c
        unix/poll.c
        unix/process.c
        unix/signal.c
        unix/spinlock.h
        unix/stream.c
        unix/tcp.c
        unix/thread.c
        unix/threadpool.c
        unix/timer.c
        unix/tty.c
        unix/udp.c)

set(libuv_SOURCES_LINUX
        unix/linux-core.c
        unix/linux-inotify.c
        unix/linux-syscalls.c)

get_cmake_property(_variableNames VARIABLES)
foreach (_variableName ${_variableNames})
    message(STATUS "${_variableName}=${${_variableName}}")
endforeach()

if(WIN32)
    set(libuv_SOURCES ${libuv_SOURCES_COMMON} ${libuv_SOURCES_WIN32})
else()
    if(${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
        set(libuv_SOURCES
            ${libuv_SOURCES_COMMON}
            ${libuv_SOURCES_UNIX_COMMON} 
            ${libuv_SOURCES_LINUX})
    endif()
endif()


get_cmake_property(_variableNames VARIABLES)
foreach (_variableName ${_variableNames})
    message(STATUS "${_variableName}=${${_variableName}}")
endforeach()



add_library(uvlib ${libuv_SOURCES})

FIND_PACKAGE ( Threads REQUIRED )
TARGET_LINK_LIBRARIES ( uvlib ${CMAKE_THREAD_LIBS_INIT} ${CMAKE_DL_LIBS} )

