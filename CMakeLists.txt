cmake_minimum_required(VERSION 3.8)

project(signals CXX)

enable_testing()

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

include_directories(include)
file(GLOB SOURCES src/*.cc)

add_library(signals "")
target_sources(signals PRIVATE ${CMAKE_SOURCE_DIR}/include/signals.h
                               ${CMAKE_SOURCE_DIR}/src/signals.cpp)
target_include_directories(signals PUBLIC ${CMAKE_SOURCE_DIR}/src)
target_compile_options(signals PUBLIC
    $<$<CXX_COMPILER_ID:GNU>:-Wall>
    $<$<CXX_COMPILER_ID:Clang>:-Wall>
    $<$<CXX_COMPILER_ID:AppleClang>:-Wall>
    $<$<CXX_COMPILER_ID:MSVC>:/W4>)

find_package (Threads)
target_link_libraries (signals PUBLIC ${CMAKE_THREAD_LIBS_INIT})
    
option(SIGNALS__ENABLE_TESTS "Enable tests" OFF)
option(SIGNALS__ENABLE_DEMO "Enable demo" OFF)

if(SIGNALS__ENABLE_TESTS)
  add_subdirectory(tests)
endif()

if(SIGNALS__ENABLE_DEMO)
  add_subdirectory(demo)
endif()
