cmake_minimum_required(VERSION 3.10)
project(libaapt2 LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# เพิ่ม flag สำหรับ 16KB page size
add_compile_options(-fPIC -Wall -Wextra -Werror -D__BIONIC__ -DANDROID -D__ANDROID__ -D_PAGE_SIZE=16384)

# หาไฟล์ทั้งหมดจาก aapt2
file(GLOB_RECURSE AAPT2_SOURCES
    aapt2/*.cpp
    aapt2/*.c
)

# สร้าง shared library
add_library(libaapt2 SHARED ${AAPT2_SOURCES})

# include path
target_include_directories(libaapt2 PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/aapt2
)

# ลิงก์ lib ที่ aapt2 ต้องใช้
find_library(log-lib log)
find_library(android-lib android)
find_library(z-lib z)

target_link_libraries(libaapt2
    ${log-lib}
    ${android-lib}
    ${z-lib}
)
