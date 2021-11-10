---
permalink: /build_windows/
title: "Building on Windows"
overview: false
layout: single

sidebar:
  nav: "doc_nav"

---

A Visual Studio project file can be found in port/windows/vs2015/IoTivity-Lite.sln. Open the solution file in Visual Studio 2015 or newer.
If the version of Visual Studio is newer a prompt should pop up asking if you would like to upgrade the visual studio project files.
You may agree to upgrade the files.

Select the version of the samples you would like to build. Debug/Release, x86/x64.
From the build menu select Build Solution.

The samples can be run from Visual Studio by right clicking on the SimpleServer or SimpleClient project from the Solution Explorer and select Debug > Start new instance.
Alternatively, the binaries may be run from the output folder port/windows/vs2015/{Debug|Release}/{Win32|x64}/.

The build-time framework configuration that enables various conditionally compiled stack features is hard coded into the visual studio projects.

To alter this configuration, the properties pages for individual projects need to be modified.
Right click on the project and select Properties. Find C/C++ > Preprocessor > Preprocessor Definitions.
Locate the macro(s) associated with the feature(s) you wish to enable or disable. For example to disable the OCF security layer, remove OC_SECURITY from the Preprocessor Definitions. 
The Preprocessor Definitions must match over all projects for them to build, link and run successfully.
Due to the difficulty in keeping all the projects in sync, it is recommended to avoid modifying the Preprocessor Definitions unless absolutely necessary.

Instructions for compiling the Java bindings can be found here.
