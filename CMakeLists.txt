cmake_minimum_required(VERSION 3.21)

# Use C++20
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Architectures for multi‑platform builds
set(CMAKE_OSX_ARCHITECTURES "arm64;x86_64")
set(CMAKE_CXX_VISIBILITY_PRESET hidden)

project(XXLify VERSION 1.3.5)

# Source files
add_library(${PROJECT_NAME} SHARED
    src/main.cpp
    # Add more .cpp files here if needed
)

# Locate Geode SDK
if (NOT DEFINED ENV{GEODE_SDK})
    message(FATAL_ERROR "Unable to find Geode SDK! Please define GEODE_SDK environment variable.")
else()
    message(STATUS "Found Geode SDK at: $ENV{GEODE_SDK}")
endif()

# Add Geode
add_subdirectory($ENV{GEODE_SDK} ${CMAKE_CURRENT_BINARY_DIR}/geode)

# iOS‑specific handling
if (GEODE_PLATFORM_IOS)
    message(STATUS "Building XXLify for iOS")

    # iOS needs bitcode disabled for Geode mods
    set(CMAKE_XCODE_ATTRIBUTE_ENABLE_BITCODE "
