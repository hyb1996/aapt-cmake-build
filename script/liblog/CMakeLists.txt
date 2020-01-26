cmake_minimum_required(VERSION 3.6)
project(liblog)

if(UNIX AND NOT APPLE)
    SET (CMAKE_C_COMPILER "/usr/bin/clang")
    SET (CMAKE_CXX_COMPILER "/usr/bin/clang++")
endif()

set(liblog_sources
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/config_read.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/config_write.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/log_event_list.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/log_event_write.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/logger_lock.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/logger_name.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/logger_read.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/logger_write.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/logprint.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/stderr_write.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/event_tag_map.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/log_time.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/properties.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/pmsg_reader.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/pmsg_writer.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/logd_reader.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/logd_writer.cpp
        )

set(liblog_host_sources
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/fake_log_device.cpp
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/fake_writer.cpp
        )

if(WIN32)
    list(APPEND liblog_host_sources ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/uio.c)
elseif(UNIX)
    list(APPEND liblog_host_sources  ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/event_tag_map.cpp)
endif()

set(liblog_cflag "-DFAKE_LOG_DEVICE=1 -Werror -fvisibility=hidden -DLIBLOG_LOG_TAG=1006 -DSNET_EVENT_LOG_TAG=1397638484")

if(UNIX)
    set(liblog_cflag "${liblog_cflag} -fPIC")
endif()

if(UNIX AND NOT APPLE)
    #启用libcxx
    set(liblog_cflag "${liblog_cflag} ${global_libcxx_cxxflags}")
endif()

#添加宏定义，目的是为了移除一部分实现，如断言，否则编译成功，执行可能有问题
set(liblog_cflag "${liblog_cflag} -DNDEBUG -UDEBUG")

set(liblog_cxxflag "${liblog_cflag} -std=gnu++14")

include_directories(
        ${liblog_header}
        ${libcutils_header}
        ${libsystem_header}
        ${libutils_header}
)

#mac不参与下面三个库的编译，否则libaapt2_jni有问题
if(UNIX AND NOT APPLE)
    include_directories(
            ${libcxx_header}
            ${libcxxabi_header}
            ${compiler_rt_header}
    )
endif()

set(CMAKE_C_FLAGS ${liblog_cflag})
set(CMAKE_CXX_FLAGS ${liblog_cxxflag})
if(UNIX)
    if(USE_32BITS)
        message(STATUS "using 32bits")
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m32")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m32")
    endif()
endif()
add_library(log STATIC ${liblog_sources} ${liblog_host_sources})

if(UNIX AND NOT APPLE)
    target_link_libraries(log cxx cxxabi compiler_rt)
endif()


