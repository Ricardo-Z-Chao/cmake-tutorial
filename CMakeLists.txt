cmake_minimum_required(VERSION 3.15)

set(gcc_like_cxx "$<COMPILE_LANG_AND_ID:CXX,ARMClang,AppleClang,Clang,GNU,LCC>")
set(msvc_cxx "$<COMPILE_LANG_AND_ID:CXX,MSVC>")

project(Tutorial VERSION 1.0)

configure_file(TutorialConfig.h.in TutorialConfig.h)

add_subdirectory(MathFunctions)

add_executable(Tutorial tutorial.cxx)

add_library(tutorial_compiler_flags INTERFACE)
target_compile_features(tutorial_compiler_flags INTERFACE cxx_std_11)
target_compile_options(
  tutorial_compiler_flags
  INTERFACE
    "$<${gcc_like_cxx}:$<BUILD_INTERFACE:-Wall;-Wextra;-Wshadow;-Wformat=2;-Wunused>>"
    "$<${msvc_cxx}:$<BUILD_INTERFACE:-W3>>")

target_link_libraries(Tutorial PUBLIC MathFunctions tutorial_compiler_flags)
target_include_directories(Tutorial PUBLIC "${PROJECT_BINARY_DIR}")
