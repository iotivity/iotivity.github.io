---
permalink: /build_linux/
title: "Building on Linux"
overview: false
layout: single

sidebar:
  nav: "doc_nav"

---

he Linux build is fully specified in ``<iotivity-root>port/linux/Makefile``.
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
