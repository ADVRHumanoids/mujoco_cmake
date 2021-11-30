cmake_minimum_required(VERSION 3.0)
project(mujoco_hhcm VERSION 2.1.0)

set(MJ_VER ${PROJECT_VERSION_MAJOR}${PROJECT_VERSION_MINOR}${PROJECT_VERSION_PATCH})


include(ExternalProject)

ExternalProject_Add(
    mujoco
    PREFIX "mujoco"
    URL https://mujoco.org/download/mujoco${MJ_VER}-linux-x86_64.tar.gz
    URL_HASH SHA256=a436ca2f4144c38b837205635bbd60ffe1162d5b44c87df22232795978d7d012
    CONFIGURE_COMMAND ""
    BUILD_COMMAND ""
    INSTALL_COMMAND ""
)

set(MUJOCO_SRC_DIR ${CMAKE_CURRENT_BINARY_DIR}/mujoco/src/mujoco)

configure_file(mujoco_hhcmConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/mujoco_hhcmConfig.cmake
    @ONLY)

install(
    FILES
    ${CMAKE_CURRENT_BINARY_DIR}/mujoco_hhcmConfig.cmake
    DESTINATION
    lib/cmake/${PROJECT_NAME}
    )

install(
    DIRECTORY
    ${MUJOCO_SRC_DIR}/include/
    DESTINATION
    include/mujoco)

install(
    DIRECTORY
    ${MUJOCO_SRC_DIR}/bin/
    DESTINATION
    lib
    FILES_MATCHING PATTERN *.so*)
