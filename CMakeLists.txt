cmake_minimum_required(VERSION 3.14)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# TODO: rename project and delete this comment
project(template VERSION 0.1.0)
string(APPEND CMAKE_CXX_FLAGS " -pedantic -Werror -Wall -Wextra")
string(APPEND CMAKE_CXX_FLAGS " -Wno-unused-command-line-argument")
string(APPEND CMAKE_CXX_FLAGS " -Wshadow -Wnon-virtual-dtor")

add_library(${PROJECT_NAME} STATIC
  # enum your files and delete this comment
  ${CMAKE_CURRENT_SOURCE_DIR}/sources/example.cpp
)

add_executable(demo
  ${CMAKE_CURRENT_SOURCE_DIR}/demo/main.cpp
)

target_include_directories(${PROJECT_NAME} PUBLIC
  "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
  "$<INSTALL_INTERFACE:include>"
)

target_link_libraries(demo ${PROJECT_NAME})


if(BUILD_TESTS)
  include(FetchContent)
  FetchContent_Declare(
          googletest
          GIT_REPOSITORY https://github.com/google/googletest.git
          GIT_TAG        release-1.10.0
  )
  FetchContent_MakeAvailable(googletest)
  include(GoogleTest)

  add_executable(tests
    # TODO: enum your files and delete this comment
    ${CMAKE_CURRENT_SOURCE_DIR}/tests/example.cpp
  )
  target_link_libraries(tests ${PROJECT_NAME} pthread gtest_main)
  enable_testing()
  add_test(NAME unit_tests COMMAND tests)
endif()
