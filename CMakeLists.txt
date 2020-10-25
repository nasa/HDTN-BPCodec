#BP codec build environment requires cmake version 3.13
#Testing has been done with gcc version 8.3.0 (Debian 8.3.0-6) Target: x86_64-linux-gnu
cmake_minimum_required(VERSION 3.13)
project(bpcodec)
set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_CXX_STANDARD 11)

add_library(bpcodec 
        src/codec/bpv7.cpp
        src/codec/bpv6.cpp
        src/codec/sdnv/cgoto/decode.cpp
        src/codec/sdnv/cgoto/encode.cpp
        src/codec/sdnv/loop/codec.cpp
        src/codec/cbor/cgoto/codec.cpp
        src/codec/cbor/standard/codec.cpp
        src/codec/cbor/memo/codec.cpp
        include/util/apple_endian.h)

# This library ships with a few different options for encoding / decoding values.
# Once can give them a try to see what the effects on performance look like.
#add_compile_definitions(BPV6_SDNV_CGOTO)
add_compile_definitions(BPV6_SDNV_LOOP)

#add_compile_definitions(BPV7_CBOR_CGOTO)
add_compile_definitions(BPV7_CBOR_STD)
#add_compile_definitions(BPV7_CBOR_MEMOIZE)

target_include_directories(bpcodec PUBLIC /opt/local/include include)

add_subdirectory(apps)

