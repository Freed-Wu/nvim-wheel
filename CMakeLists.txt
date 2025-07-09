cmake_minimum_required(VERSION 3.15)
# cmake-format: off
set(VERSION 0.11.2 CACHE STRING "version of nvim")
# cmake-format: on
project(
  nvim-wheel
  VERSION "${VERSION}"
  DESCRIPTION "Vim-fork focused on extensibility and usability"
  HOMEPAGE_URL "https://neovim.io/")

set(SOURCE_DIR "${CMAKE_SOURCE_DIR}/build")
include(ExternalProject)
if(CMAKE_OSX_ARCHITECTURES)
  set(CMAKE_ARGS "-DCMAKE_OSX_ARCHITECTURES=${CMAKE_OSX_ARCHITECTURES}")
  set(PATCH_COMMAND "${CMAKE_SOURCE_DIR}/scripts/patch.pl" "${SOURCE_DIR}/src/nvim/CMakeLists.txt" "${SOURCE_DIR}/cmake.deps/CMakeLists.txt" "${SOURCE_DIR}/cmake.deps/cmake/BuildLpeg.cmake")
endif()
ExternalProject_Add(nvim-populate
  "UPDATE_DISCONNECTED" "False" "DOWNLOAD_EXTRACT_TIMESTAMP" "TRUE" "EXTERNALPROJECT_INTERNAL_ARGUMENT_SEPARATOR" "URL" "https://github.com/neovim/neovim/archive/refs/tags/v${VERSION}.tar.gz"
  SOURCE_DIR          "${SOURCE_DIR}"
  BINARY_DIR          "${SKBUILD_NULL_DIR}"
  PATCH_COMMAND       "${PATCH_COMMAND}"
  CONFIGURE_COMMAND   "${CMAKE_COMMAND}" -B "${SOURCE_DIR}/.deps" -S "${SOURCE_DIR}/cmake.deps" "${CMAKE_ARGS}" &&
                      "${CMAKE_COMMAND}" --build "${SOURCE_DIR}/.deps" &&
                      "${CMAKE_COMMAND}" -B "${SKBUILD_NULL_DIR}" -S "${SOURCE_DIR}" "-DCMAKE_INSTALL_PREFIX=${SKBUILD_DATA_DIR}" "-DCMAKE_INSTALL_BINDIR=${SKBUILD_SCRIPTS_DIR}" "${CMAKE_ARGS}"
  BUILD_COMMAND       "${CMAKE_COMMAND}" --build "${SKBUILD_NULL_DIR}"
  INSTALL_COMMAND     "${CMAKE_COMMAND}" --install "${SKBUILD_NULL_DIR}"
  TEST_COMMAND        ""
  USES_TERMINAL_DOWNLOAD  YES
  USES_TERMINAL_UPDATE    YES
  USES_TERMINAL_PATCH     YES
)
