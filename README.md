## BPCodec ##

BPcodec is a stand-alone library designed to support encoding and decoding the bundle protocol format.  Both version 6 and version 7 of the bundle protocol are supported.  The version 6 encode and decode functions have been tested with JPL's ION and GSFC's bplib [1]. The version 7 decode functions have been tested on bundles from uPCN. The version 7 encode functions will be released in the near term. The version 7 implemention is in early stages (pre-alpha) but is being released for research and collaborative purposes in the overall DTN community.

Note that the use of this library will produce results that are compatible with the bundle protocol from a wire perspective.  It does not, however, intend to offer a complete implementation of either version 6 or version 7 of the Bundle Protocol - semantics related to receipt, forwarding, and custody must be independently implemented and observed by a system that wishes to advertise compatibility with such.

## Build Environment ##
* BPcodec build environment requires CMake version 3.13
* Testing has been done with gcc version 8.3.0 (Debian 8.3.0-6) 
* Target: x86_64-linux-gnu 
* Tested on Debian 10
* Known issues:
* Ubuntu distributions may install an older CMake version that is not compatible
* Mac OS may not support recvmmsg and sendmmsg functions, recvmsg and sendmsg could be used

## Overview ##

The core of the library is found in bpv6.h and bpv7.h - each header file is responsible for the corresponding protocol version.  

The library is geared around two primary operations:

* Taking a byte string and properly decoding it into a C structure, and
* Taking a C structure and properly encoding it into a byte string

The library expects some familiarity with the bundle protocol: for example, one should know that the primary block should come first, and that the primary block is followed by additional canonical blocks.  The library also does not presently enforce constraints (e.g. "the payload block shall always be the last block in a bundle").

The "test" directory includes a small collection of bundles used to validate that encoding / decoding work as expected.  These bundles are utilized by a small collection of benchmark utilities included with the library.  

Note that this library does not yet support the generation of bundle security headers.

## Applications ##
The "apps" directory includes several tools built upon the basic bpcodec functions.
* The encode-benchmark simply tests the CBOR (BP v7) and SDNV (BP v6) functions on generated values.
* The bundle-benchmark reads bundles from files in the "test" directory. These bundles were generated from ION (JPL), bplib (GSFC) and uPCN in order to test the bundle decoding on a variety of implementations. The ION and bplib bundles are BP v6 and the uPCN bundle is BP v7. In the current release not all bundle extension blocks are supported, although this functionality will be added to the release in the near term.
* The bpgen tool will generate BP v6 bundles of a specified size as well as an approximate data rate. Source and destination node numbers can also be specified. Endpoint IDs currently follow the IPN style of addressing (node_number.service_number). This tool has been used with both ION and bplib in previous testing. The BP v7 functions have not been integrated into this tool yet, although this is planned in the near term. It will generate a csv file of statistics from the bundles sent. 
* The bpsink tool will listen for and receive bundles. It will generate a csv file of statistics from the bundles received. 

## Quick Start ##
* git clone https://github.com/nasa/HDTN-BPCodec.git
* cd ./HDTN-BPCodec
* mkdir ./build
* cd ./build
* cmake ..
* make

The simplest test you can start with is to run bpgen with bpsink. Bpgen will generate bundles and send them out as UDP packets on 127.0.0.1 port 4556.

In one terminal:
* cd ./HDTN-BPCodec/build/apps
* ./bpgen    (there are command line parameters you can set as well but you don't need to)


In another terminal:
* cd ./HDTN-BPCodec/build/apps
* ./bpsink

## Future Work ##
There are several areas currently under development with functionality that will be released in the near term.
* BP v7 primary and canonical block encode functions are being developed.
* BP v6 extension blocks are in the process of being tested with current BP v6 implementations of ION and BPlib.
* CRC validation is planned to be added to the BP v7 functions.

## References ##
1. Hylton, A., Raible, D., Clark, G.A., Dudukovich, R., Tomko, B., Laura, & Burk (2019). Rising Above the Cloud- Toward High-Rate Delay-Tolerant Networking in Low- Earth Orbit.
