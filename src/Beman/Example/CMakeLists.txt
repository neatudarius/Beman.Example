# SPDX-License-Identifier: MIT
# Copyright © 2024 Bret Brown

set(TARGET_LIBRARY "beman_example")

# Add the library.
add_library("${TARGET_LIBRARY}" STATIC "")
add_library(beman::example ALIAS "${TARGET_LIBRARY}")

# Using the full project version because example does not provide a stable ABI
# between shared library versions
set_property(TARGET "${TARGET_LIBRARY}" PROPERTY SOVERSION "${PROJECT_VERSION}")

# Set the C++ sources.
target_sources("${TARGET_LIBRARY}" PRIVATE
  example.cpp
)
target_include_directories(
  "${TARGET_LIBRARY}"
  PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../../../include>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/${CMAKE_LOWER_PROJECT_NAME}> # <prefix>/include/scratch
)
# target_sources(example PUBLIC
#   FILE_SET HEADERS
#   BASE_DIRS ../../../
#   FILES ../../../include/
# )

# GNUInstallDirs provides default definitions for CMAKE_INSTALL_LIBDIR
# See: https://cmake.org/cmake/help/latest/module/GNUInstallDirs.html
include(GNUInstallDirs)

# Install the library.
install(TARGETS "${TARGET_LIBRARY}"
  EXPORT "${TARGET_LIBRARY}"
  ARCHIVE
    COMPONENT libexample-dev
  FILE_SET HEADERS
    COMPONENT libexample-dev
  LIBRARY
    COMPONENT libexample
    NAMELINK_COMPONENT libexample-dev
)
install(EXPORT "${TARGET_LIBRARY}"
  DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/${TARGET_LIBRARY}"
  COMPONENT libexample-dev
  NAMESPACE beman::
)

if (BUILD_TESTING)
  enable_testing()

  add_subdirectory(tests)
endif ()
