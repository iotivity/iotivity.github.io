---
permalink: /build_android/
title: "Building on Android"
overview: false
layout: single

sidebar:
  nav: "doc_nav"

toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky : true
---

To get started, you will need the following:

* git version control system
* SWIG
* Java Development kit
* Android SDK
* Android NDK
* Gradle build system

The following instructions describe how to obtain and run the Android developer tools on Linux.

Install Dependencies
These may be brought by installing the following packages on Ubuntu Linux:

sudo apt-get install git make openjdk-8-jdk swig
 
Download the Android SDK command line tools
Run the sdkmanager found in the tools/bin directory to install the platform-tools and Android platform:

```bash
./sdkmanager "platform-tools" "platforms;android-23"
```

Download the Android NDK and run the standalone toolchain
```bash
cd <ndk>/build/tools
sudo apt-get install python
./make_standalone_toolchain.py --arch <architecture> --api <level> --install-dir <path>
```

Valid values for are --arch

* arm
* arm64
* x86
* x86_64

The make_standalone_toolchain script only supports api level 16 and newer. We recommend using api level 23 or newer.

For example:

```bash
./make_standalone_toolchain.py --arch arm --api 23 --install-dir ~/android-arm-23
```

Note: running make_standalone_toolchain.py may print a WARNING stating that it is no longer necessary. 
This is expected. However, at this time the make files expect the stand alone tool chain.

Install Android Studio (optional)

## Compile IoTivity

The build must be run from <iotivity-root>/port/android/.

The Makefile uses the Android NDK that was installed above. You may either set ANDROID_API and ANDROID_BASE in the Makefile, or invoke make as follows:

```bash
make NDK_HOME=/opt/android-ndk ANDROID_API=23
```

You would pass additional options to make as described in the Linux build instructions.

For e.g.:

```bash
make NDK_HOME=~/android-arm-23 ANDROID_API=23 IPV4=1 DEBUG=1
```

A successful build would copy the library files (*.so and *.jar) into directories containing the Android sample apps.

When developing your own project, you would need to manually copy the libraries from <iotivity-root>/swig/iotivity-lite-java/libs to the appropriate location in your project's directory structure.

## Building and running the sample apps

All sample apps have the default out of the box behavior of IoTivity, which means they are are not onboarded or provisioned.
The default security will prevent the samples from communicating with one another until onboarding and provisioning has been completed.
Visit the Getting Started page for basic Onboarding and Provisioning instructions using IoTivity's sample Onboarding tool.

A sample server and client can be found in <iotivity-root>/swig/apps/<sample>/.

You must use gradlew to build the Android application package (apk).
Note that gradlew will require a local.properties to exist or ANDROID_HOME to be defined.
An installation of Android Studio should create the local.properties file.

```bash
export ANDROID_HOME=~/Android/sdk
```

To resolve any proxy issues, please refer to gradle user guide.

The server sample is located in android_simple_server/SimpleServer. To build and install the sample execute the following command:

Method 1

```bash
./gradlew installDebug
```

Method 2

```bash
./gradlew assembleDebug
```

To install the APK:

```bash
cd app/build/outputs/apk
adb install app-armeabi-debug.apk
```

The client sample is located in android_simple_client/SimpleClient. To build and install the sample execute the following command:

Method 1

```bash
./gradlew installDebug
```

Method 2

```bash
./gradlew assembleDebug
```

To install the APK:

```bash
cd app/build/outputs/apk
adb install app-armeabi-debug.apk
```

## Building your own Android Applications

The included sample apps were built using Android API 23. When creating your new Android project, a different API level may be chosen.

Also, in building these examples, the native code libraries were copied to specific directories in the project by the Makefile.

A project's directory structure is as follows:

```bash
	project/
	|   +-- libs/
		|   +-- iotivity-lite.jar
			|   +-- src/
			|   +-- main/
				|   +-- AndroidManifest.xml
				|   +-- java/
				|   +-- jniLibs/
					|   +-- armeabi/
						|   +-- libiotivity-lite-jni.so.so
					|   +-- x86-64/
						|   +-- libiotivity-lite-jni.so.so
```

This structure is reflected in the application's build.gradle file:

```text
   android {
        .
        .
        .
        sourceSets {
            main {
                jniLibs.srcDirs = ["src/main/jniLibs", "$buildDir/native-libs"]
            }
        }
        splits {
            abi {
                enable true
                reset()
                include 'x86_64', 'armeabi'
                universalApk false
            }
        }
    }
 
    dependencies {
        compile fileTree(dir: 'libs', include: ['*.jar'])
        .
        .
        .
    }
```

Lastly, appropriate permissions have to be granted in the AndroidManifest.xml file.

```text
    <manifest ...="">
 
        <uses-permission android:name="android.permission.INTERNET">
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE">
        <uses-permission android:name="android.permission.CHANGE_WIFI_STATE">
        <uses-permission android:name="android.permission.CHANGE_WIFI_MULTICAST_STATE">
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE">
        <uses-permission android:name="android.permission.CHANGE_NETWORK_STATE">
 
        <application .="" application="">
    </application></uses-permission></uses-permission></uses-permission></uses-permission></uses-permission></uses-permission></manifest>
```
