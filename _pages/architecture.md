---
layout: single
title: Architecture
overview: true
permalink: /architecture/

sidebar:
  nav: "doc_nav"

toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky : true
---
## Introduction

IoTivity is an open-source, reference implementation of the OCF Secure IP Device Framework.

The OCF Secure IP Device Framework provides a versatile communications layer with best-in-class security for Device-to-Device (D2D) and Device-to-Cloud (D2C) connectivity over [IP](https://en.wikipedia.org/wiki/Internet_Protocol).
IoT interoperability is achieved through the use of consensus-derived, industry standard [data models](https://openconnectivity.org/developer/oneiota-data-model-tool/) spanning an array of usage verticals.
The OCF Secure IP Device Framework may be harnessed alongside other IoT technologies in a synergistic fashion to lend a comprehensive and robust IoT solution.

Please review the following resources for more details:

* [OCF Security](https://openconnectivity.org/specs/OCF_Security_Specification.pdf)
* [Device Commissioning](https://openconnectivity.org/specs/OCF_Onboarding_Tool_Specification.pdf)
* [Cloud Connectivity](https://openconnectivity.org/specs/OCF_Device_To_Cloud_Services_Specification.pdf)
* [Bridging](https://openconnectivity.org/specs/OCF_Bridging_Specification.pdf)
* [Headless Configuration](https://openconnectivity.org/specs/OCF_Wi-Fi_Easy_Setup_Specification_v2.1.2.pdf)

The IoTivity project offers device vendors and application developers royalty-free access to [OCF technologies](https://openconnectivity.org/developer/specifications/) under the Apache 2.0 license.

## IoTivity stack Features

![IoTivity stack features](/assets/images/iotivitylitearchitecture_2.png)

* OS agnostic: The IoTivity device stack and modules work cross-platform (pure C code) and execute in an event-driven style.
  The stack interacts with lower level OS/hardware platform-specific functionality through a set of abstract interfaces. This decoupling of the common OCF standards related functionality from platform adaptation code promotes ease of long-term maintenance and evolution of the stack through successive releases of the OCF specifications.
  ![IoTivity porting layer](/assets/images/iotivityliteport.png)

* Porting layer: The platform abstraction is a set of generically defined interfaces which elicit a specific contract from implementations.
  The stack utilizes these interfaces to interact with the underlying OS/platform. The simplicity and boundedness of these interface definitions allow them to be rapidly implemented on any chosen OS/target. Such an implementation constitutes a "port".
* Optional support for static memory: On minimal environments lacking heap allocation functions, the stack may be configured to statically allocate all internal structures by setting a number of build-time parameters, which by consequence constrain the allowable workload for an application.
* C and Java APIs: The API structure and naming closely aligns with OCF specification constructs, aiding ease of understanding.

## Next steps

Visit the [Getting Started](/getting-started) guides

Review the [Documentation](/documentation)

Go to the [source](https://github.com/iotivity/iotivity-lite)
