cmake_minimum_required(VERSION 3.7)
project(graph LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_VERBOSE_MAKEFILE OFF)
set(CMAKE_BUILD_TYPE Debug)

# -Wall -> enable all warnings that are useful to avoid
# -Werror -> make all warnings errors
# -Wl,-z,relro,-z,now -> enable full memory corruption mitigation technique
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wl,-z,relro,-z,now")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -ggdb")

list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/dependencies/cmake/Modules/")

get_filename_component(INCLUDE_DIR "./include" REALPATH)
include_directories(${INCLUDE_DIR})

find_package(Boost REQUIRED COMPONENTS system)
include_directories(${Boost_INCLUDE_DIRS})

file(GLOB_RECURSE SOURCE "./src/*.cpp")
add_library(graph STATIC ${SOURCE})

include(CTest)
add_executable(graph-test test/r_c_shortest_paths_test.cpp)
target_link_libraries(graph-test graph ${Boost_LIBRARIES})
add_test(graph-test graph-test)