cmake_minimum_required(VERSION 3.24)
project(YourApp LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Warnings
if(MSVC)
  add_compile_options(/W4 /permissive-)
else()
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

add_library(your_app STATIC src/hello.cpp)
target_include_directories(your_app PUBLIC include)

add_executable(your_app_cli src/main.cpp)
target_link_libraries(your_app_cli PRIVATE your_app)

# Tests (Catch2 via vcpkg or Conan)
option(ENABLE_TESTING "Build tests" ON)
if(ENABLE_TESTING)
  include(CTest)
  find_package(Catch2 3 CONFIG REQUIRED)     # provided by vcpkg/Conan
  add_executable(tests tests/test_hello.cpp)
  target_link_libraries(tests PRIVATE your_app Catch2::Catch2WithMain)
  include(Catch)
  catch_discover_tests(tests)
endif()
