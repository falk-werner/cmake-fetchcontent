cmake_minimum_required(VERSION 3.20)
project(sample-app)

find_package(PkgConfig REQUIRED)
pkg_check_modules(FOO IMPORTED_TARGET foo)
if (FOO_FOUND)
add_library(foo ALIAS PkgConfig::FOO)
else()
include(FetchContent)
FetchContent_Declare(
  foo
  GIT_REPOSITORY https://github.com/falk-werner/cmake-fetchcontent-libfoo
  GIT_TAG        main
)
FetchContent_MakeAvailable(foo)
endif()

add_executable(app src/main.c)
target_link_libraries(app PUBLIC foo)
