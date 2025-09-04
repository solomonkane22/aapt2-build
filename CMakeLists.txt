cmake_minimum_required(VERSION 3.10)
project(libaapt2 LANGUAGES CXX)

# ตั้งค่า C++ standard
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)

# เพิ่ม compile flags
add_compile_options(
    -Wall
    -Werror
    -Wno-unused-parameter
    -Wno-missing-field-initializers
    -fno-exceptions
    -fno-rtti
    -Wno-deprecated-declarations
)

# รวม include directories (header libs)
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/jni_headers
    ${CMAKE_CURRENT_SOURCE_DIR}/native_headers
)

# หาไฟล์ source ทุก folder ตาม android.bp
file(GLOB_RECURSE TOOL_SOURCES
    aapt2/cmd/*.cpp
)

# สมมติ CMakeLists.txt อยู่ใน aapt2/
file(GLOB_RECURSE LIB_SOURCES
    aapt2/compile/*.cpp
    aapt2/configuration/*.cpp
    aapt2/dump/*.cpp
    aapt2/filter/*.cpp
    aapt2/format/*.cpp
    aapt2/io/*.cpp
    aapt2/link/*.cpp
    aapt2/optimize/*.cpp
    aapt2/process/*.cpp
    aapt2/split/*.cpp
    aapt2/text/*.cpp
    aapt2/util/*.cpp
    aapt2/java/*.cpp
    aapt2/trace/*.cpp
    aapt2/xml/*.cpp
    aapt2/Debug.cpp
    aapt2/DominatorTree.cpp
    aapt2/LoadedApk.cpp
    aapt2/Resource.cpp
    aapt2/ResourceParser.cpp
    aapt2/ResourceTable.cpp
    aapt2/ResourceUtils.cpp
    aapt2/ResourceValues.cpp
    aapt2/SdkConstants.cpp
    aapt2/ValueTransformer.cpp
)

file(GLOB PROTO_FILES
    aapt2/*.proto
)

# สร้าง libaapt2 static library
add_library(libaapt2 STATIC
    ${LIB_SOURCES}
    ${PROTO_FILES}
)

# เพิ่ม linker flags
set_target_properties(libaapt2 PROPERTIES
    LINK_FLAGS "-Wl,-z,max-page-size=16384 -Wl,-z,common-page-size=16384"
)

# สร้าง executable aapt2
add_executable(aapt2 aapt2/Main.cpp ${TOOL_SOURCES})
target_link_libraries(aapt2 libaapt2)
