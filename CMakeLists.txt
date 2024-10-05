cmake_minimum_required(VERSION 3.11.0)
project(Pizzas)

include(FetchContent)
FetchContent_Declare(
        googletest
        URL https://github.com/google/googletest/archive/03597a01ee50ed33e9dfd640b249b4be3799d395.zip
)

FetchContent_MakeAvailable(googletest)

set(SRC_LIST
        src/Funghi.cpp
        src/Margherita.cpp
        src/Pizza.cpp
        src/Pizzeria.cpp)

set(FLAGS -Wall -Wextra -pthread)

add_library(${PROJECT_NAME}-lib STATIC ${SRC_LIST})
target_compile_options(${PROJECT_NAME}-lib PRIVATE ${FLAGS})
set_target_properties(${PROJECT_NAME}-lib PROPERTIES
        CXX_STANDARD 17
        CXX_STANDARD_REQUIRED ON)

add_executable(${PROJECT_NAME} src/main.cpp)
target_compile_options(${PROJECT_NAME} PRIVATE ${FLAGS})
target_link_libraries(${PROJECT_NAME} ${PROJECT_NAME}-lib)
set_target_properties(${PROJECT_NAME} PROPERTIES
        CXX_STANDARD 17
        CXX_STANDARD_REQUIRED ON)

enable_testing()
include(GoogleTest)

add_executable(${PROJECT_NAME}-gt test/test_main.cpp test/PizzeriaTest.cpp)
target_include_directories(${PROJECT_NAME}-gt PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src)
target_link_libraries(${PROJECT_NAME}-gt ${PROJECT_NAME}-lib gtest_main gmock_main)
target_compile_options(${PROJECT_NAME}-gt PRIVATE ${FLAGS})
set_target_properties(${PROJECT_NAME} PROPERTIES
        CXX_STANDARD 17
        CXX_STANDARD_REQUIRED ON)

gtest_discover_tests(${PROJECT_NAME}-gt)
