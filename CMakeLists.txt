cmake_minimum_required(VERSION 3.21)
set(CMAKE_TLS_VERIFY 0) # Disable SSL verification because something is broken with it on Russia..
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_VISIBILITY_PRESET hidden)
if ("${CMAKE_SYSTEM_NAME}" STREQUAL "iOS" OR IOS)
    set(CMAKE_OSX_ARCHITECTURES "arm64")
else()
    set(CMAKE_OSX_ARCHITECTURES "arm64;x86_64")
endif()

project(main)

file(GLOB SOURCES src/*.cpp)
message(${SOURCES})
add_library(${PROJECT_NAME} SHARED ${SOURCES})
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/src)

add_subdirectory($ENV{GEODE_SDK} ${CMAKE_CURRENT_BINARY_DIR}/geode)

target_include_directories(${PROJECT_NAME} PUBLIC "${GEODE_DEPS_DIR}")

setup_geode_mod(${PROJECT_NAME})

target_compile_definitions(${PROJECT_NAME} PUBLIC 
    CMAKE_CURRENT_SOURCE_DIR="${CMAKE_CURRENT_SOURCE_DIR}"
    CMAKE_CURRENT_LIST_FILE="${CMAKE_CURRENT_LIST_FILE}"
)