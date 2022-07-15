---
permalink: /build_linux/
title: "Building on Linux"
overview: false
layout: single

sidebar:
  nav: "doc_nav"

---

## Building with Make

The Linux build is fully specified in ``<iotivity-root>port/linux/Makefile``.
A successful build produces IoTivity static and dynamic libraries and sample applications which are all stored under ``<iotivity-root>port/linux/``.

* Run make for a release build without any debug output.

* Add DEBUG=1 to produce a debug build including verbose debugging logs.

* Add IPV4=1 to include IPv4 support in the build. The stack's default configuration includes only IPv6 support.

* Add TCP=1 to include support for CoAP over TCP (RFC 8323).

* Add CLOUD=1 to include OCF Device-to-Cloud (D2C) support.

* Add JAVA=1 to compile the project's Java bindings using SWIG.

* Add SECURE=0 to exclude the OCF security layer and mbedTLS. The security layer is compiled in by default and must be excluded only for testing purposes.

The build-time framework configuration that enables various conditionally compiled stack features is defined in ``<iotivity-root>port/linux/oc_config.h``.

Instructions for compiling the Java bindings can be found [here](/build_java).

## Building with CMake

Alternatively, build for CMake is specified by CMakeLists.txt files in ``<iotivity-root>`` directory and subdirectories.

To run a cmake first create the output directory in the ``<iotivity-root>`` directory:

```sh
mkdir build
```

Then move to the output directory and run CMake to generate the build system (by default Unix Makefiles will be generated, but other build systems are available):

```sh
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
```

This will generate a build system for a release build without any debug output. Other build options can be specified by adding them to the cmake invocation:

* Add `-DCMAKE_BUILD_TYPE=Debug` and `-DOC_DEBUG_ENABLED=ON` to produce a debug build including verbose debugging logs.

* Add `-DOC_IPV4_ENABLED=ON` to include IPv4 support in the build. The stack's default configuration includes only IPv6 support.

* Add `-DOC_TCP_ENABLED=ON` to include support for CoAP over TCP (RFC 8323).

* Add `-DOC_CLOUD_ENABLED=ON` to include OCF Device-to-Cloud (D2C) support.

All supported options are listed at the beginning of the ``<iotivity-root>/CMakeLists.txt`` file.

Finally, run the build itself:

```sh
cmake --build .
```

A successful build produces IoTivity static and dynamic libraries and sample applications which are all stored in the output directory.

The build-time framework configuration that enables various conditionally compiled stack features is defined in ``<iotivity-root>port/linux/oc_config.h``.

### Building with mbedTLS as a shared library

For secure builds the mbedTLS library is used, by default it is compiled as a static library and linked with the IoTivity libraries. It is possible to link IoTivity libraries dynamically with the mbedTLS library. To do so, use the following build option:

* Add `-DUSE_SHARED_MBEDTLS_LIBRARY=ON` to build mbedTLS targets as shared libraries (.so)

* Add `-DUSE_STATIC_MBEDTLS_LIBRARY=OFF` to skip building of mbedTLS targets as static libraries (.a)

Install built libraries into /usr/local/lib and headers into /usr/local/include (sudo is needed if /usr/local/lib or /usr/local/include are owned by the root user):

```sh
sudo cmake --install .
```

Please note that IoTivity-lite uses a patched version of the mbedTLS library. The applied patches can be found in ``<iotivity-root>/patches`` directory. Therefore, a native build of mbedTLS from the official mbedTLS repository won't link with the IoTivity-lite libraries.
If you want to save space by using mbedTLS in IoTivity-lite targets and in your custom target, you must use the patched version. The easiest way is to link your custom target with the mbedTLS libraries created by the IoTivity-lite installation.
