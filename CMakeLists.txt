# Example call of HiGHS from another cmake project.

cmake_minimum_required(VERSION 3.15)

set(CMAKE_CXX_COMPILER_NAMES clang++ g++ c++ ${CMAKE_CXX_COMPILER_NAMES})
set(CMAKE_CXX_STANDARD 11)

project(LOAD_HIGHS LANGUAGES CXX)

include(FetchContent)

set(BRANCH "master" CACHE STRING "branch")

message(CHECK_START "Fetching HiGHS")
list(APPEND CMAKE_MESSAGE_INDENT "  ")

message(STATUS "branch is ${BRANCH}")

FetchContent_Declare(
    highs
    GIT_REPOSITORY "https://github.com/ERGO-Code/HiGHS.git"
    GIT_TAG        "${BRANCH}"
)

# set(FAST_BUILD ON CACHE INTERNAL "Fast Build")
# set(ZLIB OFF CACHE INTERNAL "ZLIB OFF")

FetchContent_MakeAvailable(highs)
list(POP_BACK CMAKE_MESSAGE_INDENT)
message(CHECK_PASS "fetched")

if(APPLE)
  set(CMAKE_INSTALL_RPATH "@executable_path/../lib")
elseif(UNIX)
  set(CMAKE_INSTALL_RPATH "$ORIGIN/../lib")
endif()

add_executable(main call_from_cpp.cc)
target_link_libraries(main highs::highs)

install(TARGETS main)