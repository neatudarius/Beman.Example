# SPDX-License-Identifier: MIT
# Copyright © 2024 Bret Brown

set(TARGET_TEST "example_gtest")

# Add the test executable.
add_executable("${TARGET_TEST}")

# Set the C++ sources.
target_sources("${TARGET_TEST}" PRIVATE 
  example.t.cpp
)

# Include the GoogleTest CMake module.
include(GoogleTest)

# Link the library.
target_link_libraries("${TARGET_TEST}" PRIVATE beman::example GTest::gtest GTest::gtest_main)

# Add the tests.
gtest_add_tests("${TARGET_TEST}" "" AUTO)
