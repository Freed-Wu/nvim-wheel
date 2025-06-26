cmake_minimum_required(VERSION 3.15)
# cmake-format: off
set(VERSION 0.11.2 CACHE STRING "version of nvim")
# cmake-format: on
project(
  nvim-wheel
  VERSION "${VERSION}"
  DESCRIPTION "Vim-fork focused on extensibility and usability"
  HOMEPAGE_URL "https://neovim.io/")

include(ExternalProject)
ExternalProject_Add(nvim-populate
  "UPDATE_DISCONNECTED" "False" "DOWNLOAD_EXTRACT_TIMESTAMP" "TRUE" "EXTERNALPROJECT_INTERNAL_ARGUMENT_SEPARATOR" "URL" "https://github.com/neovim/neovim/archive/refs/tags/v${VERSION}.tar.gz"
  SOURCE_DIR          "${CMAKE_SOURCE_DIR}/build"
  BINARY_DIR          "${SKBUILD_NULL_DIR}"
  CONFIGURE_COMMAND   cmake -B "${SKBUILD_NULL_DIR}" -S "${CMAKE_SOURCE_DIR}/build" "-DCMAKE_INSTALL_PREFIX=${SKBUILD_DATA_DIR}" "-DCMAKE_INSTALL_BINDIR=${SKBUILD_SCRIPTS_DIR}"
  BUILD_COMMAND       cmake --build "${SKBUILD_NULL_DIR}"
  INSTALL_COMMAND     cmake --install "${SKBUILD_NULL_DIR}"
  TEST_COMMAND        ""
  USES_TERMINAL_DOWNLOAD  YES
  USES_TERMINAL_UPDATE    YES
  USES_TERMINAL_PATCH     YES
)
