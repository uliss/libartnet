
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

target_compile_definitions(
  libartnet PUBLIC
  HAVE_INET_ATON=1
  HAVE_INET_NTOA=1
  HAVE_MALLOC=1
  HAVE_MEMSET=1
  HAVE_REALLOC=1
  HAVE_SELECT=1
  HAVE_STDBOOL_H=1
  HAVE_STDDEF_H=1
  HAVE_STDINT_H=1
  HAVE_STDLIB_H=1
  HAVE_STRCHR=1
  HAVE_STRDUP=1
  HAVE_STRERROR=1
  HAVE_STRING_H=1
  HAVE_VPRINTF=1
  IPV6=1
  PACKAGE="libartnet"
  SELECT_TYPE_ARG1=int
  SELECT_TYPE_ARG234=(fd_set*)
  SELECT_TYPE_ARG5=(struct timeval*)
  STDC_HEADERS=1
)
# UNIX
if (UNIX)
  target_compile_definitions(
    libartnet PUBLIC
    HAVE_ARPA_INET_H=1
    HAVE_DLFCN_H=1
    HAVE_GETIFADDRS=1
    HAVE_SYS_IOCTL_H=1
    HAVE_SYS_SELECT_H=1
    HAVE_SYS_SOCKET_H=1
    HAVE_SYS_STAT_H=1
    HAVE_SYS_TYPES_H=1
    HAVE_UNISTD_H=1
    
    SELECT_TYPE_ARG1=int
    SELECT_TYPE_ARG234=(fd_set*)
    SELECT_TYPE_ARG5=(struct timeval*)
    
    
  )
  
  if (NOT APPLE)
    target_compile_definitions(
      libartnet PUBLIC
      HAVE_LINUX_IF_PACKET_H=1 
      HAVE_ENDIAN_H=1
    )
  endif()
elseif (WIN32)
  target_compile_definitions(
    libartnet PUBLIC   
  )
)



if(WIN32)
  target_link_libraries(
    libartnet
    PUBLIC  # maybe not
    ws2_32
    IPHLPAPI
   )
endif()

add_executable(
  test
  main.c
)

target_link_libraries(
  test
  PRIVATE
  libartnet
)
