---
permalink: /gs_docker/
title: "Containerized Iotivity"
overview: false
layout: single

sidebar:
  nav: "doc_nav"

toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky : true

title: Quick Start Guide for Containerized IoTivity-Lite Resources
author: Andy Dolan
date: November 4, 2021
---

# Introduction

This guide provides a brief introduction to the use of container images that
provide various examples of applications built with IoTivity-Lite. These images
include:

* The `ocfadmin/iotivity-examples` image - a collection of basic IoTivity-Lite
  applications that can be used to quickly show how OCF Devices built with
  IoTivity-Lite interact with each other.
* The `ocfadmin/iotivity-builder` image - a portable development environment
  that can be used to compile and run IoTivity's example applications and more.
* The `ocfadmin/devicebuilder` image - a containerized version of OCF's
  DeviceBuilder, which can be used to generate source code scaffolding for
  IoTtivity-Lite.

These images are available on [DockerHub](https://hub.docker.com/u/ocfadmin),
and their source code/image definitions are available on [GitHub](https://github.com/openconnectivityfoundation/Dockerized_IoTivity).

Note that these containers are prototypes for demonstration purposes.

## Prerequisites

The remainder of this guide assumes the use of [Docker](https://www.docker.com/)
and [`docker-compose`](https://docs.docker.com/compose/). The images described
in this guide are Linux-based, but their primary functionalities should be
consistent on any platform that can run Docker.

You should be familiar with basic `docker` and `docker-compose` commands. But no
special configuration of the Docker runtime should be required unless explicitly
stated. More advanced examples may utilize volumes and/or bind mounts.

# Example IoTivity-Lite Applications

## Quick Start with `docker-compose`

The fastest way to try out an example OCF domain where example Devices built
with IoTivity-Lite can be discovered, onboarded, and configured to interoperate
is with the `ocfadmin/iotivity-examples` examples image.

### Starting the Example

The following `docker-compose` file can be used to quickly spin up a full
example:

```yaml
# docker-compose.yml
version: '3.8'

services:
  obt:
    container_name: obt
    image: ocfadmin/iotivity-examples
    tty: true
    stdin_open: true
    command: onboarding_tool
    networks:
      - ocfnet

  simpleserver:
    container_name: simpleserver
    image: ocfadmin/iotivity-examples
    tty: true
    stdin_open: true
    command: simpleserver
    networks:
      - ocfnet

  simpleclient:
    container_name: simpleclient
    image: ocfadmin/iotivity-examples
    tty: true
    stdin_open: true
    command: simpleclient
    networks:
      - ocfnet

# Note that IPv6 is not officially supported in compose file v3....
# IPv6 support must be enabled in /etc/docker/daemon.json
networks:
  ocfnet:
    enable_ipv6: true
    ipam:
      driver: default
      config:
        - subnet: 172.16.2.0/24
          gateway: 172.16.2.1
        - subnet: 2001:db8:2::/64
          gateway: 2001:db8:2::1

```

This file can also be downloaded from [GitHub](https://github.com/openconnectivityfoundation/Dockerized_IoTivity/blob/master/docker-compose.yml).

This file defines instances of the three example IoTivity-Lite applications that
the examples image comes with - `simpleserver`, `simpleclient`, and
`onboarding_tool`. These each represent OCF Devices, and can interact together.

`simpleserver` acts as a lamp and `simpleclient` controls the lamp.

This example can be run with the following commands, assuming that the compose
file (`docker-compose.yml`) is present in the current working directory. Note
that, if you haven't yet pulled the `ocfadmin/iotivity-examples` image, that
`docker-compose` will pull it for you.

```bash
# Use the docker-compose file to spin up application instances
docker-compose up -d

# In one terminal, follow logs for the simpleserver and simpleclient containers
docker-compose logs -f simpleserver simpleclient

# In another temrinal, attach to onboarding_tool container
docker attach obt
# Output will be blank; enter 0 and hit return to cause OBT to display menu
```

At this point, you should see some minimal output from the `simpleserver` and
`simpleclient` logs, and the OBT's main menu should appear on screen. An example
of what this might look like appears below.

![Example output of the commands above.](./dockerized_iotivity_images/compose_up.png)

From here, the OBT can be used to discover, onboard, and configure the
`simpleserver` and `simpleclient` Devices.

### Onboarding Devices

For our example, we will first onboard `simpleserver` (the lamp) using the
Random PIN Ownership Transfer Method. We will then onboard `simpleclient` (the
light controller/"phone") with the Just-Works Ownership Transfer Method.

This can be done by following these steps (entered into the OBT):

#### Onboarding Lamp Device (Random PIN)

1. Discover the unowned devices by entering "1". Two devices (and their network
   addresses) should appear in the OBT's output.
1. Enter "9" to "Request Random PIN from device for OTM". Then enter the
   appropriate number to select the "Lamp" Device.
1. In the output for the Devices, a Random PIN should appear. Enter "10" in the
   OBT to perform "Random PIN Ownership Transfer Method". Once again enter the
   appropriate number to select the "Lamp" Device.
1. When prompted, enter the PIN that was displayed in the `simpleserver`'s log
   output and hit return.
1. If the pin was entered correctly, the OBT should report that it has
   "Successfully performed OTM" on the Lamp Device.

#### Onboarding Controller/Phone Device (Just-Works)

1. Enter "8" to perform "Just-Works Ownership Transfer Method".
1. Enter the appropriate number to select the Device labeled "Kishen's IPhone".
1. The OBT should indicate that it has "Successfully performed OTM".

#### Verify Ownership Status

At this point, both Devices should have been onboarded by the OBT. To verify
this, enter "4" into the OBT; both Devices should appear in the output as owned
Devices.

### Provisioning Devices

With both Devices onboarded, we can now use the OBT to provision credentials and
access control lists (ACLs/ACEs) to allow them to interoperate with each other.

The following steps can be used to perform this provisioning:

1. Enter "12" into the OBT to "Provision pairwise credentials".
1. Choose either Device as "device 1", then the other Device as "device 2".
1. Next, Enter "13" to "Provision ACE2".
1. Select the Lamp.
1. Select "Kishen's IPhone" as the Subject.
1. Enter "1" for the "number of resources in this ACE".
1. Enter "1" (for Yes) for "Have resource href?"
1. Enter "/a/light" for the "resource href".
1. Enter "1" (for Yes) to ACE2 Permissions for RETRIEVE, UPDATE, and NOTIFY (you
   may also simply enter "1" for all 5 permissions).

The screenshot below illustrates what setting the 5 ACE permissions might look
like.

![Example of provisioning ACE to Device.](./dockerized_iotivity_images/ace_permissions.png)

### Device Interoperation

At this point, the `simpleserver` and `simpleclient` Devices should have
everything they need to interoperate with each other. To demonstrate the Devices
interacting with each other, we will need to restart the `simpleclient` Device.

Enter "99" to exit the OBT, and ensure that your other terminal is still
following the logs from `docker-compose` (`docker-compose logs -f simpleserver
simpleclient`).

Use `docker restart simpleclient` to restart the `simpleclient` Device. Observe
the logs in your other terminal to see that the phone/controller Device is able
to update the state of the Lamp Device and observe its state changes!

The image below displays what this output might look like.

![Example of output from `simpleserver` and `simpleclient` interacting.](./dockerized_iotivity_images/restart_simpleclient.png)

## Running Manually

The `ocfadmin/iotivity-examples` image can also be instantiated individually.
Each of the applications that come with the image can be started with the
following commands. Each invocation specifies the `-i` and `-t` flags to ensure
that output (and input, in the case of the OBT) is properly captured.

```bash
# Start simpleserver
docker run --name=simpleserver -i -t ocfadmin/iotivity-examples simpleserver

# Start simpleclient
docker run --name=simpleclient -i -t ocfadmin/iotivity-examples simpleclient

# Start onboarding tool (must specify -i and -t to properly attach to input)
docker run --name=obt -i -t ocfadmin/iotivity-examples onboarding_tool
```

# DeviceBuilder Image

The OCF [DeviceBuilder](https://iotivity.org/tools/device-builder) tool can be
used to generate stubs of IoTivity-Lite application code. The
`ocfadmin/devicebuilder` image enables the use of this tool without the need to
install and configure it. Refer to the DeviceBuilder documentation for details
on how DeviceBuilder can be used.

The DeviceBuilder image is generally used with the following command:

```bash
docker run --rm -v <path to input file directory>:/devbuilder input.json <device type>
```

This image requires the use of either volumes or bind mounts, to allow the
container process to access the input JSON file.

Generated code is produced in a directory named `output`, which is created in a
location relative to the volume or bind mount.

## Example Use

In this example, assume that the following JSON file is located in the current
working directory and named `speaker_model.json`:

```json
[
  {
    "path": "/speaker",
    "rt": [ "oic.r.switch.binary", "oic.r.audio" ],
    "if": [ "oic.if.a", "oic.if.baseline" ]
  }
]
```

The DeviceBuilder image can be used to generate IoTivity-Lite application code
with the following command:

```bash
docker run --rm -v "$(pwd)":/devbuilder ocfadmin/devicebuilder speaker_model.json oic.d.speaker speaker-server
```

This command specifies the following:

* The current working directory is bind-mounted to `/devbuilder` in the
  container process.
* The input JSON file is specified (`speaker_model.json`).
  * Note that this file is relative to the `/devbuilder` directory in the
    container.
* The device type identifier `oic.d.speaker`.
* The name/title of the device, "speaker-server".
  * Note that this argument is optional.

The result of running this command is a new directory named `output` being
created in the working directory. This directory includes generated source code
that can be compiled with IoTivity-Lite, as well as schema files. An example of
the structure of this output directory appears below.

![Example `output` directory after invoking the `ocfadmin/devicebuilder` image.](./dockerized_iotivity_images/devicebuilder_output.png)

# IoTivity-Builder Image

The `ocfadmin/iotivity-builder` image can be used to compile IoTivity-Lite
applications. It includes the entire IoTivity-Lite code base, and can be used
with bind mounts or volumes to compile new code (such as that generated by
DeviceBuilder).

This guide will demonstrate how to use the image to create a simple development
environment shell that can be used to compile and run the existing IoTivity-Lite
example applications. For more advanced examples for the use of the image, refer
to the documentation and guides on [GitHub](https://github.com/openconnectivityfoundation/Dockerized_IoTivity/tree/master/build_environment).

To execute an interactive shell from which IoTivity-Lite applications can be
complied, use the following command:

```
docker run --name=iot-dev -i -t --entrypoint=/bin/bash ocfadmin/iotivity-builder
```

This will open a shell in the container's working directory of
`/iotivity-lite/port/linux`, inside which the default example applications that
come with IoTivity-Lite can be compiled and run.

For example, the following commands (run inside the container) can be used to
build the `simpleserver` example application with debug logging enabled:

```bash
make cleanall
make DEBUG=1 simpleserver
```

Once the applications have been compiled, their executables can be run directly
within the container:

```bash
./simpleserver
```
