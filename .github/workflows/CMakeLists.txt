cmake_minimum_required(VERSION 3.18)

project(aapt2 LANGUAGES C CXX)

# ใช้ C++20 ตาม Android.bp
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# สำหรับ Android NDK
if(NOT ANDROID)
    message(FATAL_ERROR "This project is intended to be built with Android NDK")
endif()

set(CMAKE_SYSTEM_NAME Android)
set(CMAKE_ANDROID_ARCH_ABI arm64-v8a)
set(CMAKE_ANDROID_NDK $ENV{ANDROID_NDK_HOME})
set(CMAKE_ANDROID_STL_TYPE c++_static)
set(CMAKE_ANDROID_NATIVE_API_LEVEL 21)

# include directories
include_directories(
    include
    native_headers
    jni_headers
    third_party/protobuf/src
)

# source files
file(GLOB_RECURSE LIBAAPT2_SOURCES
    "compile/*.cpp"
    "configuration/*.cpp"
    "dump/*.cpp"
    "filter/*.cpp"
    "format/**/*.cpp"
    "io/*.cpp"
    "link/*.cpp"
    "optimize/*.cpp"
    "process/*.cpp"
    "split/*.cpp"
    "text/*.cpp"
    "util/*.cpp"
    "java/*.cpp"
    "*.cpp"
    "*.proto"
)

file(GLOB_RECURSE TOOL_SOURCES
    "cmd/*.cpp"
)

# define static library libaapt2
add_library(libaapt2 STATIC ${LIBAAPT2_SOURCES})

# link กับ library ของ Android (ตาม Android.bp)
find_library(log-lib log)
find_library(android-lib android)

target_link_libraries(libaapt2
    ${log-lib}
    ${android-lib}
    # เพิ่ม library อื่น ๆ ที่ต้องการ เช่น z, png, expat ตาม Android.bp
)

# define host executable aapt2
add_executable(aapt2 Main.cpp ${TOOL_SOURCES})
target_link_libraries(aapt2 libaapt2)
