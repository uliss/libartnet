cmake_minimum_required (VERSION 3.1)
project(libartnet LANGUAGES C)

include(CheckIncludeFiles)
include(CheckSymbolExists)
include(CheckCSourceCompiles)

check_include_files(linux/if_packet.h HAVE_LINUX_IF_PACKET_H)
check_include_files(endian.h HAVE_ENDIAN_H)
check_c_source_compiles("
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
int main(int argc, char** argv) { &inet_aton; }
" HAVE_INET_ATON)
check_c_source_compiles("
#include <sys/types.h>
#include <ifaddrs.h>
int main(int argc, char** argv) { &getifaddrs; }
" HAVE_GETIFADDRS)
check_c_source_compiles("
#include <net/if.h>
int main(int argc, char** argv) { struct sockaddr addr; addr.sa_len; }
" HAVE_SOCKADDR_SA_LEN)

configure_file(config.h.in libartnet-config.h)

set(LIBARTNET_SRCS
  artnet/artnet.c
  artnet/misc.c
  artnet/network.c
  artnet/receive.c
  artnet/tod.c
  artnet/transmit.c
)

set(LIBARTNET_HDRS
  artnet/artnet.h
  artnet/common.h
  artnet/misc.h
  artnet/packets.h
  artnet/private.h
  artnet/tod.h
)

add_library(artnet STATIC
  "${LIBARTNET_HDRS}"
  "${LIBARTNET_SRCS}"
)

target_include_directories(
  artnet
    PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>
)

target_compile_definitions(
  artnet
  PUBLIC
    HAVE_CONFIG_H=1   # enable the config.h file
)

if(WIN32)
  target_link_libraries(
    artnet
    PUBLIC
      ws2_32
      iphlpapi
   )
endif()

install(
  TARGETS artnet
  EXPORT artnet-exports
  RUNTIME DESTINATION lib
  ARCHIVE DESTINATION lib)

install(EXPORT artnet-exports
        DESTINATION lib/cmake/libartnet)
export(EXPORT artnet-exports)
