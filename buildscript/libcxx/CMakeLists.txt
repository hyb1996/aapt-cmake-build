cmake_minimum_required(VERSION 3.6)
project(libcxx)

if (UNIX AND NOT APPLE)
    SET(CMAKE_C_COMPILER "clang")
    SET(CMAKE_CXX_COMPILER "clang++")
endif ()

set(libcxx_sources
        ${AOSP_DIR}/external/libcxx/src/algorithm.cpp
        ${AOSP_DIR}/external/libcxx/src/any.cpp
        ${AOSP_DIR}/external/libcxx/src/bind.cpp
        ${AOSP_DIR}/external/libcxx/src/chrono.cpp
        ${AOSP_DIR}/external/libcxx/src/condition_variable.cpp
        ${AOSP_DIR}/external/libcxx/src/debug.cpp
        ${AOSP_DIR}/external/libcxx/src/exception.cpp
        ${AOSP_DIR}/external/libcxx/src/future.cpp
        ${AOSP_DIR}/external/libcxx/src/hash.cpp
        ${AOSP_DIR}/external/libcxx/src/ios.cpp
        ${AOSP_DIR}/external/libcxx/src/iostream.cpp
        ${AOSP_DIR}/external/libcxx/src/locale.cpp
        ${AOSP_DIR}/external/libcxx/src/memory.cpp
        ${AOSP_DIR}/external/libcxx/src/mutex.cpp
        ${AOSP_DIR}/external/libcxx/src/new.cpp
        ${AOSP_DIR}/external/libcxx/src/optional.cpp
        ${AOSP_DIR}/external/libcxx/src/random.cpp
        ${AOSP_DIR}/external/libcxx/src/regex.cpp
        ${AOSP_DIR}/external/libcxx/src/shared_mutex.cpp
        ${AOSP_DIR}/external/libcxx/src/stdexcept.cpp
        ${AOSP_DIR}/external/libcxx/src/string.cpp
        ${AOSP_DIR}/external/libcxx/src/strstream.cpp
        ${AOSP_DIR}/external/libcxx/src/system_error.cpp
        ${AOSP_DIR}/external/libcxx/src/thread.cpp
        ${AOSP_DIR}/external/libcxx/src/typeinfo.cpp
        ${AOSP_DIR}/external/libcxx/src/utility.cpp
        ${AOSP_DIR}/external/libcxx/src/valarray.cpp
        ${AOSP_DIR}/external/libcxx/src/variant.cpp
        ${AOSP_DIR}/external/libcxx/src/vector.cpp
        )

set(libcxx_cflag "")

if(UNIX)
    set(libcxx_cflag "${libcxx_cflag} -fPIC")
endif()

#启用libcxx
set(libcxx_cflag "${libcxx_cflag} ${global_libcxx_cxxflags}")

#添加宏定义，目的是为了移除一部分实现，如断言，否则编译成功，执行可能有问题
set(libcxx_cflag "${libcxx_cflag} -DNDEBUG -UDEBUG")

set(libcxx_cxxflag "${libcxx_cflag} -std=gnu++14")

include_directories(
        ${libcxx_header}
        ${libcxxabi_header}
        ${compiler_rt_header}
)

set(CMAKE_C_FLAGS ${libcxx_cflag})
set(CMAKE_CXX_FLAGS ${libcxx_cxxflag})
if(UNIX)
    if(USE_32BITS)
        message(STATUS "using 32bits")
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m32")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m32")
    endif()
endif()
add_library(cxx STATIC ${libcxx_sources})

if (UNIX AND NOT APPLE)
    target_link_libraries(cxx rt pthread dl)
endif ()
