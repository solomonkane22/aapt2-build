cmake_minimum_required(VERSION 3.22)
project(aapt2 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_EXTENSIONS OFF)

# Enable warnings and linker flags for 16KB page size
add_compile_options(-Wall -Werror -Wno-unused-parameter -Wno-missing-field-initializers -fno-exceptions -fno-rtti)
add_link_options(-Wl,-z,max-page-size=16384 -Wl,-z,common-page-size=16384)

file(GLOB_RECURSE AAPT2_SRC
    compile/*.cpp
    configuration/*.cpp
    dump/*.cpp
    filter/*.cpp
    format/**/*.cpp
    io/*.cpp
    link/*.cpp
    optimize/*.cpp
    process/*.cpp
    split/*.cpp
    text/*.cpp
    util/*.cpp
    java/*.cpp
    trace/*.cpp
    xml/*.cpp
    *.proto
    Debug.cpp
    DominatorTree.cpp
    LoadedApk.cpp
    Resource*.cpp
    SdkConstants.cpp
    ValueTransformer.cpp
)

# Build libaapt2.so
add_library(libaapt2 SHARED ${AAPT2_SRC})

# Link libs (must be available in repo or from NDK)
target_link_libraries(libaapt2
    android
    log
    z
    png
    protobuf
)
