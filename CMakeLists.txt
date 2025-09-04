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
    ${CMAKE_CURRENT_SOURCE_DIR}/cmd/*.cpp
)

# สมมติ CMakeLists.txt อยู่ใน aapt2/
file(GLOB_RECURSE LIB_SOURCES
    ${CMAKE_CURRENT_SOURCE_DIR}/compile/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/configuration/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/dump/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/filter/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/format/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/io/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/link/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/optimize/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/process/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/split/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/text/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/util/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/java/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/trace/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/xml/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/Debug.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/DominatorTree.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/LoadedApk.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/Resource.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/ResourceParser.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/ResourceTable.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/ResourceUtils.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/ResourceValues.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/SdkConstants.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/ValueTransformer.cpp
)

file(GLOB_RECURSE PROTO_FILES
    ${CMAKE_CURRENT_SOURCE_DIR}/*.proto
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
add_executable(aapt2 Main.cpp ${TOOL_SOURCES})
target_link_libraries(aapt2 libaapt2)
