cmake_minimum_required(VERSION 3.6)
project(zlib)

if (UNIX AND NOT APPLE)
    SET(CMAKE_C_COMPILER "clang")
    SET(CMAKE_CXX_COMPILER "clang++")
endif ()

set(zlib_sources
        ${AOSP_DIR}/external/zlib/src/adler32.c
        ${AOSP_DIR}/external/zlib/src/compress.c
        ${AOSP_DIR}/external/zlib/src/crc32.c
        ${AOSP_DIR}/external/zlib/src/deflate.c
        ${AOSP_DIR}/external/zlib/src/gzclose.c
        ${AOSP_DIR}/external/zlib/src/gzlib.c
        ${AOSP_DIR}/external/zlib/src/gzread.c
        ${AOSP_DIR}/external/zlib/src/gzwrite.c
        ${AOSP_DIR}/external/zlib/src/infback.c
        ${AOSP_DIR}/external/zlib/src/inflate.c
        ${AOSP_DIR}/external/zlib/src/inftrees.c
        ${AOSP_DIR}/external/zlib/src/inffast.c
        ${AOSP_DIR}/external/zlib/src/trees.c
        ${AOSP_DIR}/external/zlib/src/uncompr.c
        ${AOSP_DIR}/external/zlib/src/zutil.c
        )

include_directories(
        ${zlib_header}
)

set(zlib_cflag "-O3 -DUSE_MMAP -DZLIB_CONST")

if(UNIX)
    set(zlib_cflag "${zlib_cflag} -fPIC")
endif()

#添加宏定义，目的是为了移除一部分实现，如断言，否则编译成功，执行可能有问题
set(zlib_cflag "${zlib_cflag} -DNDEBUG -UDEBUG")

set(CMAKE_C_FLAGS ${zlib_cflag})
if(UNIX)
    if(USE_32BITS)
        message(STATUS "using 32bits")
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m32")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m32")
    endif()
endif()
add_library(zlib STATIC ${zlib_sources})
