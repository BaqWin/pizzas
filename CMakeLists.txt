cmake_minimum_required(VERSION 3.11.0)
project(Pizzas)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(SRC_LIST
    src/Funghi.cpp
    src/Margherita.cpp
    src/Pizza.cpp
    src/Pizzeria.cpp)

set(TEST_SRC_LIST
    test/PizzeriaTest.cpp
    test/test_main.cpp)

include(FetchContent)
FetchContent_Declare(
  googletest
  GIT_REPOSITORY https://github.com/google/googletest.git
  GIT_TAG release-1.12.1
)

set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

add_library(${PROJECT_NAME}-lib STATIC ${SRC_LIST})
add_executable(${PROJECT_NAME} src/main.cpp)

find_package(Threads)
target_link_libraries(${PROJECT_NAME}-lib Threads::Threads)
target_link_libraries(${PROJECT_NAME} ${PROJECT_NAME}-lib)

link_libraries(gmock)
add_executable(${PROJECT_NAME}-ut ${TEST_SRC_LIST})
target_link_libraries(${PROJECT_NAME}-ut ${PROJECT_NAME}-lib)

enable_testing()

include(GoogleTest)
gtest_discover_tests(${PROJECT_NAME}-ut)

