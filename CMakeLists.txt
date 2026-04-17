cmake_minimum_required(VERSION 3.0)
project(mujoco_cmake VERSION 3.7.0)

set(MJ_VER ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH})


include(ExternalProject)

ExternalProject_Add(
    mujoco
    PREFIX "mujoco"
    URL https://github.com/google-deepmind/mujoco/releases/download/${MJ_VER}/mujoco-${MJ_VER}-linux-x86_64.tar.gz
    URL_HASH SHA256=f296190163faaa7424e8b9b09988b725bc67dba357e46da27ce9b0c96f3ed7f2
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

# configure and install cmake
configure_file(mujoco_cmakeConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/mujoco_cmakeConfig.cmake
    @ONLY)

install(
    FILES
    ${CMAKE_CURRENT_BINARY_DIR}/mujoco_cmakeConfig.cmake
    DESTINATION
    lib/cmake/${PROJECT_NAME}
    )

# install headers
install(
    DIRECTORY
    ${MUJOCO_SRC_DIR}/include/
    DESTINATION
    include/mujoco)

# install binaries (mujoco_compile + plugins)
install(
    PROGRAMS
    ${CMAKE_CURRENT_BINARY_DIR}/mujoco_compile
    DESTINATION
    bin
    )

install(
    DIRECTORY
    ${MUJOCO_SRC_DIR}/bin/mujoco_plugin
    DESTINATION
    bin
    FILES_MATCHING PATTERN *.so*)


install(
    FILES
    ${MUJOCO_SRC_DIR}/lib/libmujoco.so.${MJ_VER}
    DESTINATION
    lib)
