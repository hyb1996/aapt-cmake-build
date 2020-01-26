cmake_minimum_required(VERSION 3.6)
project(aapt2_static)

if (UNIX AND NOT APPLE)
    SET(CMAKE_C_COMPILER "clang")
    SET(CMAKE_CXX_COMPILER "clang++")
endif ()

if (UNIX)
    add_custom_target(protobuffer_h
            WORKING_DIRECTORY "${AOSP_DIR}"
            COMMAND ${CMAKE_BINARY_DIR}/buildscript/protobuf/aprotoc/aprotoc frameworks/base/tools/aapt2/Resources.proto frameworks/base/tools/aapt2/ResourcesInternal.proto frameworks/base/tools/aapt2/Configuration.proto --cpp_out=frameworks/base/tools/aapt2
            DEPENDS aprotoc
            )
endif ()

set(aapt2_static_sources
        ${AOSP_DIR}/frameworks/base/tools/aapt2/cmd/Compile.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/cmd/Convert.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/cmd/Diff.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/cmd/Dump.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/cmd/Link.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/cmd/Optimize.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/cmd/Util.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/compile/IdAssigner.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/compile/InlineXmlFormatParser.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/compile/NinePatch.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/compile/Png.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/compile/PngChunkFilter.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/compile/PngCrunch.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/compile/PseudolocaleGenerator.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/compile/Pseudolocalizer.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/compile/XmlIdCollector.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/configuration/ConfigurationParser.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/filter/AbiFilter.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/filter/ConfigFilter.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/format/Archive.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/format/Container.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/format/binary/BinaryResourceParser.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/format/binary/ResChunkPullParser.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/format/binary/TableFlattener.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/format/binary/XmlFlattener.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/format/proto/ProtoDeserialize.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/format/proto/ProtoSerialize.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/io/BigBufferStream.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/io/File.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/io/FileStream.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/io/FileSystem.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/io/StringStream.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/io/Util.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/io/ZipArchive.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/link/AutoVersioner.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/link/ManifestFixer.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/link/NoDefaultResourceRemover.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/link/ProductFilter.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/link/PrivateAttributeMover.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/link/ReferenceLinker.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/link/TableMerger.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/link/XmlCompatVersioner.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/link/XmlNamespaceRemover.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/link/XmlReferenceLinker.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/optimize/MultiApkGenerator.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/optimize/ResourceDeduper.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/optimize/VersionCollapser.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/process/SymbolTable.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/split/TableSplitter.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/text/Printer.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/text/Unicode.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/text/Utf8Iterator.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/util/BigBuffer.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/util/Files.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/util/Util.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/ConfigDescription.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/Debug.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/DominatorTree.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/Flags.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/java/AnnotationProcessor.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/java/ClassDefinition.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/java/JavaClassGenerator.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/java/ManifestClassGenerator.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/java/ProguardRules.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/LoadedApk.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/Locale.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/Resource.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/ResourceParser.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/ResourceTable.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/ResourceUtils.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/ResourceValues.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/SdkConstants.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/StringPool.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/xml/XmlActionExecutor.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/xml/XmlDom.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/xml/XmlPullParser.cpp
        ${AOSP_DIR}/frameworks/base/tools/aapt2/xml/XmlUtil.cpp
        )

#生成的文件在这个目录
if (EXISTS ${AOSP_DIR}/frameworks/base/tools/aapt2/frameworks/base/tools/aapt2/Resources.pb.cc)
    list(APPEND aapt2_static_sources ${AOSP_DIR}/frameworks/base/tools/aapt2/frameworks/base/tools/aapt2/Resources.pb.cc)
else ()
    message(STATUS "请先使用cmake /path/to/cmakeFiles --target protobuffer_h生成protobuffer相关文件${AOSP_DIR}/frameworks/base/tools/aapt2/frameworks/base/tools/aapt2/Resources.pb.cc，否则后续构建无法正常进行")
endif ()

#生成的文件在这个目录
if (EXISTS ${AOSP_DIR}/frameworks/base/tools/aapt2/frameworks/base/tools/aapt2/ResourcesInternal.pb.cc)
    list(APPEND aapt2_static_sources ${AOSP_DIR}/frameworks/base/tools/aapt2/frameworks/base/tools/aapt2/ResourcesInternal.pb.cc)
else ()
    message(STATUS "请先使用cmake /path/to/cmakeFiles --target protobuffer_h生成protobuffer相关文件${AOSP_DIR}/frameworks/base/tools/aapt2/frameworks/base/tools/aapt2/ResourcesInternal.pb.cc，否则后续构建无法正常进行")
endif ()

#生成的文件在这个目录
if (EXISTS ${AOSP_DIR}/frameworks/base/tools/aapt2/frameworks/base/tools/aapt2/Configuration.pb.cc)
    list(APPEND aapt2_static_sources ${AOSP_DIR}/frameworks/base/tools/aapt2/frameworks/base/tools/aapt2/Configuration.pb.cc)
else ()
    message(STATUS "请先使用cmake /path/to/cmakeFiles --target protobuffer_h生成protobuffer相关文件${AOSP_DIR}/frameworks/base/tools/aapt2/frameworks/base/tools/aapt2/Configuration.pb.cc，否则后续构建无法正常进行")
endif ()

set(aapt2_static_cflag "-Werror -Wall -Wno-unused-parameter -Wno-delete-non-virtual-dtor")

if (WIN32)
    set(aapt2_static_cflag "${aapt2_static_cflag} -D_GNU_SOURCE=1 -D__STDC_FORMAT_MACROS -Wno-error=maybe-uninitialized -Wno-error=unused-variable -Wno-strict-aliasing -Wno-unused-but-set-variable")
endif ()

if (UNIX)
    set(aapt2_static_cflag "${aapt2_static_cflag} -fPIC")
endif ()

if (UNIX AND NOT APPLE)
    set(aapt2_static_cflag "${aapt2_static_cflag} -Wno-unused-variable")
endif ()

if (UNIX AND NOT APPLE)
    #启用libcxx
    set(aapt2_static_cflag "${aapt2_static_cflag} ${global_libcxx_cxxflags}")
endif ()

#添加宏定义，目的是为了移除一部分实现，如断言，否则编译成功，执行可能有问题
set(aapt2_static_cflag "${aapt2_static_cflag} -DNDEBUG -UDEBUG")

if (APPLE)
    set(aapt2_static_cflag "${aapt2_static_cflag} -D_DARWIN_UNLIMITED_STREAMS")
endif ()

#官方的编译选项中没有此项
set(aapt2_static_cflag "${aapt2_static_cflag} -Wno-deprecated-declarations")

set(aapt2_static_cxxflag "${aapt2_static_cflag} -std=gnu++14 -Wno-missing-field-initializers -fno-exceptions")

if (UNIX AND NOT APPLE)
    set(aapt2_static_cxxflag "${aapt2_static_cxxflag} -Wno-unknown-attributes")
endif ()

include_directories(
        ${protobuf_header}
        ${androidfw_header}
        ${base_header}
        ${libziparchive_header}
        ${liblog_header}
        ${libcutils_header}
        ${libutils_header}
        ${libsystem_header}
        ${zlib_header}
        ${native_header}
        ${aapt2_header}
        ${libpng_header}
        ${expat_header}
        #生成的文件头文件
        ${AOSP_DIR}/frameworks/base/tools/aapt2/frameworks/base/tools/aapt2/
)

#mac不参与下面三个库的编译，否则libaapt2_jni有问题
if (UNIX AND NOT APPLE)
    include_directories(
            ${libcxx_header}
            ${libcxxabi_header}
            ${compiler_rt_header}
    )
endif ()


set(CMAKE/C_FLAGS ${aapt2_static_cflag})
set(CMAKE_CXX_FLAGS ${aapt2_static_cxxflag})
if (UNIX)
    if (USE_32BITS)
        message(STATUS "using 32bits")
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m32")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m32")
    endif ()
endif ()

add_library(aapt2_static STATIC ${aapt2_static_sources})
target_link_libraries(aapt2_static expat androidfw base cutils log png utils ziparchive zlib protobuf)

if (UNIX AND NOT APPLE)
    target_link_libraries(aapt2_static cxx cxxabi compiler_rt)
endif ()
