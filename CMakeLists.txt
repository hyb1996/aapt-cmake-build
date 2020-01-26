cmake_minimum_required(VERSION 3.6)
project(aapt_cmake_build)


#android源码目录
set(AOSP android10-c2f2-release)

#纯头文件引用
set(native_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/frameworks/native/include
        )
set(safe_iop_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/external/safe-iop/include
        )
set(libbacktrace_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/libbacktrace
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/libbacktrace/include
        )
set(libsystem_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/libsystem/include
        )

#源码和头文件引用
set(androidfw_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/frameworks/base/libs/androidfw/include
        )
set(expat_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/external/expat
        ${CMAKE_SOURCE_DIR}/${AOSP}/external/expat/lib
        )
set(libpng_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/external/libpng
        )
set(protobuf_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/external/protobuf/android
        ${CMAKE_SOURCE_DIR}/${AOSP}/external/protobuf/src
        )
set(zlib_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/external/zlib
        ${CMAKE_SOURCE_DIR}/${AOSP}/external/zlib/src
        )

set(libcxx_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/external/libcxx/include
        )

set(libcxxabi_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/external/libcxxabi/include
        )

set(compiler_rt_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/external/compiler-rt/include
        )

set(base_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/base/include
        )
set(libcutils_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/libcutils
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/libcutils/include
        )
set(liblog_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/liblog/include
        )
set(libutils_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/libutils
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/libutils/include
        )
set(libziparchive_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/libziparchive
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/core/libziparchive/include
        )
set(libnativehelper_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/libnativehelper/include_jni
        )
#aapt头文件
set(aapt_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/frameworks/base/tools/aapt
        )
#aapt2头文件
set(aapt2_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/frameworks/base/tools/aapt2
        )
        
set(aidl_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/system/tools/aidl
        )

set(libzopfli_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/external/zopfli/src
        )

set(zipalign_header
        ${CMAKE_SOURCE_DIR}/${AOSP}/build/tools/zipalign
        )

set(global_libcxx_cxxflags "-nostdinc++ -DLIBCXX_BUILDING_LIBCXXABI -D_LIBCPP_BUILDING_LIBRARY")


MESSAGE(STATUS "当前CMAKE_SOURCE_DIR目录: ${CMAKE_SOURCE_DIR}")
#添加子目录构建脚本
add_subdirectory(${CMAKE_SOURCE_DIR}/script/libcxx)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/libcxxabi)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/compiler-rt)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/zlib)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/libpng)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/androidfw)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/base)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/liblog)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/libziparchive)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/expat)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/libcutils)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/libutils)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/protobuf/libprotobuf-cpp-lite)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/protobuf/aprotoc)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/aapt_static)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/aapt)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/aapt2_static)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/aapt2)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/aapt2_jni)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/aidl)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/aidl-cpp)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/aidl-common)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/libzopfli)
add_subdirectory(${CMAKE_SOURCE_DIR}/script/zipalign)


