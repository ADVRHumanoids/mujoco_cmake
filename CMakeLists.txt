cmake_minimum_required(VERSION 3.0)
project(mujoco_cmake VERSION 2.1.0)

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

add_custom_command(
    COMMAND cp ${MUJOCO_SRC_DIR}/bin/compile ${CMAKE_CURRENT_BINARY_DIR}/mujoco_compile
    #OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/mujoco_compile
    TARGET mujoco
    COMMENT "renaming compile to mujoco_compile"
)

configure_file(mujoco_cmakeConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/mujoco_cmakeConfig.cmake
    @ONLY)

install(
    FILES
    ${CMAKE_CURRENT_BINARY_DIR}/mujoco_cmakeConfig.cmake
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

install(
    PROGRAMS
    ${CMAKE_CURRENT_BINARY_DIR}/mujoco_compile
    DESTINATION
    bin
    )