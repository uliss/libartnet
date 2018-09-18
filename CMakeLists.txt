
cmake_minimum_required (VERSION 3.1)

project(libartnet LANGUAGES C)

set(SRC_DIR
  "${CMAKE_CURRENT_SOURCE_DIR}/artnet")

set(LIBARTNET_SRCS
  "${SRC_DIR}/artnet.c"
  "${SRC_DIR}/misc.c"
  "${SRC_DIR}/network.c"
  "${SRC_DIR}/receive.c"
  "${SRC_DIR}/tod.c"
  "${SRC_DIR}/transmit.c"
)

set(LIBARTNET_HDRS
  "${SRC_DIR}/artnet.h"
  "${SRC_DIR}/common.h"
  "${SRC_DIR}/misc.h"
  "${SRC_DIR}/packets.h"
  "${SRC_DIR}/private.h"
  "${SRC_DIR}/tod.h"
)

add_library(libartnet
  "${LIBARTNET_HDRS}"
  "${LIBARTNET_SRCS}"
)

target_include_directories(
  libartnet
  PUBLIC
  "${CMAKE_CURRENT_SOURCE_DIR}"
)
