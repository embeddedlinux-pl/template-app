cmake_minimum_required(VERSION 3.19)

# Define application name and settings.
set(APP template-app)
include(app/settings.cmake)

include(cmake/conan.cmake)
include(cmake/coverage.cmake)
include(cmake/doxygen.cmake)
include(cmake/osal.cmake)
include(cmake/platform.cmake)
include(cmake/sanitizers.cmake)
include(cmake/utils.cmake)

project(template-app VERSION 1.0.0 LANGUAGES ASM C CXX)

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug)
endif ()

conan_init()

# Project-wide compilation options.
add_compile_options(-Wall -Wextra -Wpedantic -Werror $<$<COMPILE_LANGUAGE:CXX>:-std=c++17>)

if (USE_ASAN OR USE_LSAN OR USE_UBSAN)
    # Enable compiler sanitizers.
    enable_sanitizers()
endif ()

if (COVERAGE)
    # Generate coverage report.
    add_lcov_coverage(IGNORE ${CMAKE_CURRENT_SOURCE_DIR}/test/*)
endif ()

# Define application-wide logger settings.
add_definitions(-DAPPLICATION_LOGGER)
include_directories(app/utils/include/utils/app)

add_subdirectory(${platform_SOURCE_DIR}/app ${platform_BINARY_DIR})
add_subdirectory(${osal_SOURCE_DIR}/lib ${osal_BINARY_DIR})
add_subdirectory(${utils_SOURCE_DIR}/lib ${utils_BINARY_DIR})

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

add_subdirectory(app)

# Generate documentation.
doxygen_addTarget(${CMAKE_CURRENT_SOURCE_DIR}/lib README.md)
