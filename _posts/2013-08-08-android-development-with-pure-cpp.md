---
layout: post
title: How to develop Android apps with pure C++
---

{{ page.title }}
================

This article takes /ndk/samples/native-activity as the example.

[For better formatting, refer to this](https://github.com/vinjn/vinjn.github.io/blob/master/_posts/2013-08-08-android-development-with-pure-cpp.md)

## install-adt
( android-dev-tool, which contains sdk && eclipse)
important sdk tools:

* c:\Downloads\adt-bundle-windows-x86-20130219\sdk\platform-tools\adb.exe
* c:\Downloads\adt-bundle-windows-x86-20130219\sdk\tools\android.bat
* c:\Downloads\adt-bundle-windows-x86-20130219\eclipse\plugins\org.apache.ant_1.8.3.v20120321-1730\bin\ant.bat


## install-ndk
important ndk tools:

* c:\android-ndk-r8e\ndk-build.cmd
* c:\android-ndk-r8e\ndk-gdb.py
 
 
## install-jdk
set environment variable JAVA_HOME to c:\Program Files\Java\jdk1.7.0_25\
 
 
## generate-buid.xml
`$ android update project -p "/project/full/path"`

you can simply `cd` to the root folder of your project and type:   
`$ android update project -p .`

### generate-buid.xml SUCCESS

>
Updated and renamed default.properties to project.properties   
Updated local.properties   
No project name specified, using Activity name 'NativeActivity'.   
If you wish to change it, edit the first line of build.xml.   
Added file `C:\android-ndk-r8e\samples\native-activity\build.xml`   
Added file `C:\android-ndk-r8e\samples\native-activity\proguard-project.txt`   


### generate-buid.xml ERROR

>
Error: The project either has no target set or the target is invalid.
Please provide a `-t` or `--target` to the `android.bat update` command.

or
>
BUILD FAILED
/home/adt-path/tools/ant/build.xml:543: Unable to resolve project target 'android-17'

`$ android list targets` to find supported targets 

`$ android update project -p . -t android-10` to explicitly specify the target id

A proper output would be:

```
Available Android targets:
id: 1 or "android-17"
     Name: Android 4.2.2
     Type: Platform
     API level: 17
     Revision: 2
     Skins: HVGA, QVGA, WQVGA400, WQVGA432, WSVGA, WVGA800 (default), WVGA854, WXGA720
     ABIs : armeabi-v7a
```

We can change the content of `default.properties` file.


## build-so
`$ ndk-build`


## build-apk
`$ ant clean debug` # clean to make sure .so file is replaced   
`$ adb install -r bin/NativeActivity-debug.apk` # -r means forcefully install


## run-apk
adb shell commands is `am start -n your.package.name/your.activity.name`   
`$ adb shell am start -n com.example.native_activity/android.app.NativeActivity`   

### run-apk ERROR

>
E/AndroidRuntime( 9570): java.lang.RuntimeException: Unable to start activity ComponentInfo{com.example.native_activity/android.app.NativeActivity}: java.lang.IllegalArgumentException: Unable to find native library: native-activity   
E/AndroidRuntime( 9570): Caused by: java.lang.IllegalArgumentException: Unable to find native library: native-activity

The solution is to change 

```XML
<application android:label="@string/app_name" android:hasCode="false">
```

changed to

```XML
<application android:label="@string/app_name" android:hasCode="true">
```

Then create an empty src/ folder and try `ant debug` again
