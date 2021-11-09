---
permalink: /SIPD/
title: "Getting Involved"
overview: false
layout: single

sidebar:
  nav: "SIPD_nav"

toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky : true
---

## Getting involved

The infrastructure that enables secure IP communication of the vertical defined application.

What does it solve
The OCF Secure IP Device Framework enables vertical agnostic secure IP communication by means of a standardized framework. The open source implementation of theOCF Secure IP Device Framework is IoTivity, which is compliant to the OCF standards and is a verified implementation by means of the OCF certification program.

The OCF Secure IP Device Framework is compliant with most of the known security requirements documents.

Communication mechanisms covered by the OCF Secure IP Device Framework
IoT means interacting with the physical world, hence the physical device is important. This is also the most costly part to develop.

The Core Framework therefore is focusing on the code that is needed on the physical device. e.g. it covers:

Device 2 Device communication
Device 2 Cloud communication
This is is same communication that IoTivity is covering.

The full communication mechanisms are depicted in the image below.



OCF Secure IP Device Framework on the (Embedded) device
The OCF Secure IP Device Framework stack is designed:

To have a small footprint of the code
To communicate with small payloads, e.g. communication packages
Having the best in class security, by using latest technologies
Is based on widely accepted internet technologies, a huge amount of RFCs are being used.
Having a minimal required set of features
Having a huge set of optional features that are already available for a vendor to use
Designed in mind that vendors can concentrate on function of the device, not on the communication and security aspects.
The OCF Frame work communication has own content format, hence it is upgradable.
The payloads can be defined in any (existing) content types.
For example: CBOR, JSON, XML
Using CoAP allowing the same communication paradigms as used on top of HTTP, but then with smaller communication packages
The OCF Core Framework architecture is restfull, but the application is not limited to that paradigm
OCF Secure IP Device Framework solution space


Security aspects of the OCF Secure IP Device Framework
The OCF Secure IP Device Framework can handle payloads based on CoAP securely. Each Device will be onboarded into a secure domain. Only devices onboarded in the secure domain are allowed to talk to each other. On top of the secure domain, access controls are defined. The access control mechanisms are based per resource (URL) and Methods that are allowed on the resource. This gives a granular control of who is allowed to interact with which part of the functionality on the device. For example a guest is allowed to read the current temperature of the thermostat but not allowed to change the set point of the thermostat.

Another aspect of security is the adherence to external security requirements. OCF has investigated the external requirements against the OCF security specification.

The result of this comparison is captured below.