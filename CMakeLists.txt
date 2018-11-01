cmake_minimum_required(VERSION 3.2)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED TRUE)
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

project(bx VERSION 1.0.0)

add_library(bx
    src/allocator.cpp
    src/bx.cpp
    src/commandline.cpp
    src/crtnone.cpp
    src/debug.cpp
    src/dtoa.cpp
    src/easing.cpp
    src/file.cpp
    src/filepath.cpp
    src/hash.cpp
    src/math.cpp
    src/mutex.cpp
    src/os.cpp
    src/process.cpp
    src/semaphore.cpp
    src/settings.cpp
    src/sort.cpp
    src/string.cpp
    src/thread.cpp
    src/timer.cpp
    src/url.cpp
)

target_include_directories(bx
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    PRIVATE 3rdparty
)

set(targets_export_name "bxTargets")
set(ConfigPackageLocation lib/cmake/bx)

# Create the CMake version file.
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/bxConfigVersion.cmake
    VERSION ${PACKAGE_VERSION}
    COMPATIBILITY AnyNewerVersion
)

# Create the Config file.
include(CMakePackageConfigHelpers)
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/bxConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/bxConfig.cmake
    INSTALL_DESTINATION ${ConfigPackageLocation}
)

# Install the different headers and libraries.
install(
    DIRECTORY
        include/bx
        include/compat
        include/tinystl
    DESTINATION include COMPONENT Development
)

install(TARGETS bx
    EXPORT ${targets_export_name}
    LIBRARY DESTINATION lib COMPONENT Runtime
    ARCHIVE DESTINATION lib COMPONENT Development
    RUNTIME DESTINATION bin COMPONENT Runtime
    BUNDLE DESTINATION bin COMPONENT Runtime
    PUBLIC_HEADER DESTINATION include COMPONENT Development
    BUNDLE DESTINATION bin COMPONENT Runtime
)

# Install the generated CMake files.
install(
    EXPORT
        ${targets_export_name}
    NAMESPACE
        "bx::"
    DESTINATION
        ${ConfigPackageLocation}
)

install(
    FILES
        ${CMAKE_CURRENT_BINARY_DIR}/bxConfigVersion.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/bxConfig.cmake
    DESTINATION
        ${ConfigPackageLocation}
)

