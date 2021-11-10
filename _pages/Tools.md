---
permalink: /tools/
title: "Tools"
overview: false
layout: single

sidebar:
  nav: "doc_nav"

toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky : true
---

This page links to IoTivity developer tools and sample applications.

## OCF Device Spy

### Introduction

The Open Connectivity Foundation (OCF) develops international standards for the Internet of Things (IoT).
OCF is distinguished by its unprecedented interoperability, state-of-the-art security, and certification program.
Additionally, OCF has a fully functional open source implementation and several development tools that enable fully operational OCF IoT
devices to be developed in minutes.

One of the tools OCF has produced is an closed source onboarding tool and generic client, called OCF Device Spy and OTGC.
Most IoT devices are servers in the IoT client-server model (lights, locks, meters, etc.).
Getting information from these servers and controlling them requires the use of a client.

### Onboarding Tool

OCF Device Spy is such a client. Before any server can be queried or controlled, a secure connection must be established between the client and the server.
This process is called onboarding. Strong security is critical in the IoT as IoT devices are often capable of having risky capabilities in the real world.
For example, locks can be unlocked, lights can be controlled, and sensitive data can be collected. 
OCF designed top-level security into its specifications from the start. OCF supports three different security methods.
These include “Just Works,” “Random PIN,” and “Public Key Infrastructure.”
This security ensures that all OCF devices are authenticated to be what they claim to be, authorized to take the actions they are intending to take and that these devices can be disabled if they misbehave.

Device Spy is a client that has passed OCF certification and implements all of the types of OCF security.
With PKI security it chains back to a secure root of trust from authorized security vendors. To be clear, OCF implements security as strong as what you use with your bank or to make online purchases.
Once Device Spy establishes a secure connection with a server device, nobody else can get access to that device unless the original instance of Device Spy explicitly gives the new client permission.

### Client

Once you finish onboarding and have a secure connection between Device Spy  and the OCF server you want to control, it list which resouces are implemented in the OCF server.

The list of  is built using an “introspection” file that reports all the resources available on the server device. 
The interaction level is based on the CoAP CRUDN methods with JSON as payloads, Device Spy will convert the input and output JSON data to CBOR to interact with the OCF Server.

Device Spy is currently only available for Windows and can be downloaded [here](https://openconnectivityfoundation.github.io/development-support/DeviceSpy/).

## Onboarding Tool and Generic Client (OTGC)

One of the tools OCF has produced is an open source onboarding tool and generic client.
Most IoT devices are servers in the IoT client-server model (lights, locks, meters, etc.). Getting information from these servers and controlling them requires the use of a client.

### Onboarding Tool

OTGC is such a client. Before any server can be queried or controlled, a secure connection must be established between the client and the server. 
This process is called onboarding. 
Strong security is critical in the IoT as IoT devices are often capable of having risky capabilities in the real world. 
For example, locks can be unlocked, lights can be controlled, and sensitive data can be collected. OCF designed top-level security into its specifications from the start.
OCF supports three different security methods. These include “Just Works,” “Random PIN,” and “Public Key Infrastructure.” This security ensures that all OCF devices are authenticated to be what they claim to be, authorized to take the actions they are intending to take and that these devices can be disabled if they misbehave.

OTGC is a client that has passed OCF certification and implements all of the types of OCF security.
With PKI security it chains back to a secure root of trust from authorized security vendors. To be clear, OCF implements security as strong as what you use with your bank or to make online purchases. 
Once OTGC establishes a secure connection with a server device, nobody else can get access to that device unless the original instance of OTGC explicitly gives the new client permission.

### Generic Client

Once you finish onboarding and have a secure connection between OTGC and the OCF server you want to control, it automatically creates a user interface to that server device.
For example, if the server implements a normal on/off light, OTGC will create an on/off switch you can use to control that light.
If the server device implements a thermostat, OTGC will create a control to let you set the desired temperature within the range of the thermostat.

The Generic Client is built using an “introspection” file that reports all the controls available on the server device and the permissions for each of those controls.
The authentication and authorization credentials of OTGC will determine the specific permissions the server device allows.

Each control on an OCF server device is called a “resource.” OCF has well over one hundred resources available that can be assigned to any OCF device. OTGC is currently available for Android and Linux.
