cmake_minimum_required(VERSION 3.6)
project(libutils)

if (UNIX AND NOT APPLE)
    SET(CMAKE_C_COMPILER "clang")
    SET(CMAKE_CXX_COMPILER "clang++")
endif ()

set(libutils_sources
        ${AOSP_DIR}/system/core/libutils/CallStack.cpp
        ${AOSP_DIR}/system/core/libutils/FileMap.cpp
        ${AOSP_DIR}/system/core/libutils/JenkinsHash.cpp
        ${AOSP_DIR}/system/core/libutils/NativeHandle.cpp
        ${AOSP_DIR}/system/core/libutils/Printer.cpp
        ${AOSP_DIR}/system/core/libutils/PropertyMap.cpp
        ${AOSP_DIR}/system/core/libutils/RefBase.cpp
        ${AOSP_DIR}/system/core/libutils/SharedBuffer.cpp
        ${AOSP_DIR}/system/core/libutils/Static.cpp
        ${AOSP_DIR}/system/core/libutils/StopWatch.cpp
        ${AOSP_DIR}/system/core/libutils/String8.cpp
        ${AOSP_DIR}/system/core/libutils/String16.cpp
        ${AOSP_DIR}/system/core/libutils/StrongPointer.cpp
        ${AOSP_DIR}/system/core/libutils/SystemClock.cpp
        ${AOSP_DIR}/system/core/libutils/Threads.cpp
        ${AOSP_DIR}/system/core/libutils/Timers.cpp
        ${AOSP_DIR}/system/core/libutils/Tokenizer.cpp
        ${AOSP_DIR}/system/core/libutils/Unicode.cpp
        ${AOSP_DIR}/system/core/libutils/VectorImpl.cpp
        ${AOSP_DIR}/system/core/libutils/misc.cpp
        )

set(libutils_cflag "-Wall -Werror -DLIBUTILS_NATIVE=1")

if(UNIX)
    set(libutils_cflag "${libutils_cflag} -fPIC")
endif()

#aosp中没有此编译选项
set(libutils_cflag "${libutils_cflag} -Wno-unused-variable")
if(UNIX)
    set(libutils_cflag "${libutils_cflag} -Wno-unused-private-field")
endif()

if(UNIX AND NOT APPLE)
    #启用libcxx
    set(libutils_cflag "${libutils_cflag} ${global_libcxx_cxxflags}")
endif()

#添加宏定义，目的是为了移除一部分实现，如断言，否则编译成功，执行可能有问题
set(libutils_cflag "${libutils_cflag} -DNDEBUG -UDEBUG")

if(APPLE)
    set(libutils_cflag "${libutils_cflag} -Wno-unused-parameter")
endif()

if(WIN32)
    set(libutils_cflag "${libutils_cflag} -DMB_CUR_MAX=1 -D_GNU_SOURCE=1 -D__STDC_FORMAT_MACROS")
endif()

if(UNIX AND NOT APPLE)
    set(libutils_sources
            "${libutils_sources}"
            ${AOSP_DIR}/system/core/libutils/Looper.cpp
            ${AOSP_DIR}/system/core/libutils/ProcessCallStack.cpp
            )
endif()

set(libutils_cxxflag "${libutils_cflag} -std=gnu++14")

include_directories(
        ${libutils_header}
        ${safe_iop_header}
        ${liblog_header}
        ${libbacktrace_header}
        ${libcutils_header}
        ${libsystem_header}
)

#mac不参与下面三个库的编译，否则libaapt2_jni有问题
if(UNIX AND NOT APPLE)
    include_directories(
            ${libcxx_header}
            ${libcxxabi_header}
            ${compiler_rt_header}
    )
endif()

set(CMAKE_C_FLAGS ${libutils_cflag})
set(CMAKE_CXX_FLAGS ${libutils_cxxflag})
if(UNIX)
    if(USE_32BITS)
        message(STATUS "using 32bits")
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m32")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m32")
    endif()
endif()
add_library(utils STATIC ${libutils_sources})
target_link_libraries(utils log)
