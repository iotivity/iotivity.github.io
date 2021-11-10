---
permalink: /DeviceBuilder/
title: "Code Generation using Device Builder"
overview: false
layout: single

sidebar:
  nav: "doc_nav"

toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky : true
---

## Introduction

The Open Connectivity Foundation is dedicated to ensuring secure interoperability for consumers, businesses and industries by delivering a standard communications platform,  the IoTivty open source implementation allowing devices to communicate regardless of form factor, operating system, service provider, transport technology or ecosystem.

IoTivity has all OCF Secure IP Device Framework components built into the stack, and the vertical resources (e.g. for sensors or actuators) are to be implemented in the IoTivity application.

The Device Builder tool chain is available to assist with the creation of IoTivity application code that defines the vertical resources.

The typical layering of the application is described in the following image. 


![Stack layer](/assets/images/lite-stack.png)

## Device Builder Tool Chain
The setup of development chain can be found [here](https://openconnectivity.github.io/IOTivity-Lite-setup/).

## Vertical Resources
The application that contains the vertical resources (e.g. the function of the device) can be generated.

The input for the vertical resources are in Open API specifications, a widely used API specification format to design APIs.

The API design uses HTTP verbs + JSON as payload definition. 
The HTTP verbs and JSON payloads are translated into IoTivity application code to use CoAPs & the CBOR encoding for payloads. 
Having HTTP & JSON as input enables readability for the application designer, since HTTP and JSON are widely used everywhere. 
Hence there is abundant information out there on how to model the resources for the vertical.

Already quite a lot of OCF models are available in Swagger2.0 format:

* Domain vertical resources are available [here](https://github.com/openconnectivityfoundation/IoTDataModels).
* Core optional resources are available [here](https://github.com/openconnectivityfoundation/core-extensions).
Note that most of the core optional resources are already baked into the IoTivity stack and are thus available if an application needs that functionality.

## Tool Chain

The development chain comprises of a setup of the tool chain and also a local copy of the IoTivity code base.

The tool chain is depicted in the following image:


![Tool Chain](/assets/images/toolchain.png)

All components will be set up with a single command. Follow the instructions here.

The following folder structure after installation will be available:

![folder structure](/assets/images/folderlayout.png)

## Development Flow
The development flow is based on bash scripts, hence the flow is generalized for Linux based systems. The development flow is depicted the figure below:

![development process](/assets/images/dev-process.png)

Explanation about the scripts:

* edit_input.sh, script to edit which resources should be generated. using Nano.
* gen.sh, script that generates the code and places the results in the development tree.
* edit_code.sh, script to edit the code in the development tree, using Nano. 
* build.sh, script to build the code, wrapper around Make, Linux only
* run.sh, script to run the created executable, Linux only.

These scripts are for convenience only. The scripts work well to generate application code for a headless system without a GUI; For e.g. a Raspberry Pi board.

Please check [here](https://openconnectivity.github.io/IOTivity-Lite-setup/#development-flow) for more information.

Please check here for more information.