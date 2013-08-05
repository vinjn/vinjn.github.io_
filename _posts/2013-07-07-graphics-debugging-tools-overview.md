---
layout: post
title: An Overview of Graphics Debugging Tools
---

{{ page.title }}
================

> If you feel uncomfortable with the table style, feel free to check [this prettier one](https://github.com/vinjn/vinjn.github.io/blob/master/_posts/2013-07-07-graphics-debugging-tools-overview.md)

Tool | Vendor | Host     | Device      | D3D   | OpenGL    | Comment  
---  | ---   | ---  |  ---  | ---  |   ---     | ---     
apitrace | open-source | desktop-OS  | desktop-OS| 8~11  | OpenGL/ES | [link](http://apitrace.github.io/)
PIX  | Microsoft | Win | Win     | 9~11  |          | DX SDK, replaced by VS Graphics Debugger
VS Graphics Debugger|Microsoft|Win| Win| 9~11.1 | | Bundled with VS 2012 pro, [link](http://msdn.microsoft.com/en-us/library/hh315751.aspx)
GLIntercept| open-source | Win | Win |     | OpenGL    | [link](https://code.google.com/p/glintercept/)
WebGL-Inspector| open-source | desktop-OS| Browser |   | WebGL     | [link](http://benvanik.github.io/WebGL-Inspector)
Nsight | NVIDIA | desktop-OS| desktop-OS | 9~11| OpenGL    | Also supprots OpenCL/CUDA/C++ AMP, Needs Visual Studio / Eclipse, [link](http://www.nvidia.com/object/nsight.html)
PerfHUD ES| NVIDIA | desktop-OS | Tegra-Android |  | ES | [link](https://developer.nvidia.com/nvidia-perfhud-es )
CodeXL|AMD|Win/Linux| Win/Linux |  | OpenGL    | formly gDEBugger, also supports OpenCL,   [link](http://developer.amd.com/tools-and-sdks/heterogeneous-computing/codexl/)
GPU PerfStudio|AMD |Win|Win|10~11 |OpenGL| [link](http://developer.amd.com/tools-and-sdks/graphics-development/gpu-perfstudio-2/)
GPA| Intel |desktop-OS | desktop-OS/Atom-Android |9~11| ES | [link](http://software.intel.com/en-us/vcsource/tools/intel-gpa)
Android-glTracer| Google| any | Android |  | ES      | [link](http://developer.android.com/tools/help/gltracer.html)
Mali Graphics Debugger|ARM|Win/Linux| Android/Linux | | ES | Also supports OpenCL, [link](http://malideveloper.arm.com/develop-for-mali/tools/mali-graphics-debugger/)
PVRTrace/PVRTune | Imagination| desktop-OS | iOS/Android/PC | | ES | [link](http://www.imgtec.com/powervr/insider/sdkdownloads/index.asp)

My personal favorites are:

* GPA for D3D
* GLIntercept for OpenGL
* Android-glTracer for OpenGL-ES
