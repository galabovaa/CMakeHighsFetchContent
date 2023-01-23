# Example call of HiGHS from another cmake project.

cmake_minimum_required(VERSION 3.15)

set(CMAKE_CXX_COMPILER_NAMES clang++ icpc c++ ${CMAKE_CXX_COMPILER_NAMES})
set(CMAKE_CXX_STANDARD 11)

project(LOAD_HIGHS LANGUAGES CXX)

include(FetchContent)

message(CHECK_START "Fetching HiGHS")
list(APPEND CMAKE_MESSAGE_INDENT "  ")
set(CI OFF) # disable CI tests
FetchContent_Declare(
    highs
    GIT_REPOSITORY "https://github.com/ERGO-Code/HiGHS.git"
    GIT_TAG        "aa697541e9fd0aafdd8e3be722a913411288bc22"
)
set (FAST_BUILD ON CACHE INTERNAL "Fast Build")

FetchContent_MakeAvailable(highs)
list(POP_BACK CMAKE_MESSAGE_INDENT)
message(CHECK_PASS "fetched")

add_executable(main call_from_cpp.cc)
target_link_libraries(main highs::highs)