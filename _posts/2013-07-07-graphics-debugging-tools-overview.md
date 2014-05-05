---
layout: post
title: An Overview of Graphics Debugging Tools
---

{{ page.title }}
================

> If you feel uncomfortable with the table style, feel free to check [this prettier one](https://github.com/vinjn/vinjn.github.io/blob/master/_posts/2013-07-07-graphics-debugging-tools-overview.md)

**Desktop graphics debugger**

Tool | Vendor | Host     | Device      | D3D   | OpenGL    | Comment  
---  | ---   | ---  |  ---  | ---  |   ---     | ---     
apitrace | open-source | desktop-OS  | desktop-OS| 8~11  | OpenGL/ES | [link](http://apitrace.github.io/)
VOGL | Valve  | Linux  | Linux|    | OpenGL | [link](https://github.com/ValveSoftware/vogl/)
Renderdoc | Crytek | Windows | Windows | 11.0 / 11.1 | | [link](http://cryengine.com/renderdoc )
PIX  | Microsoft | Win | Win     | 9~11  |          | DX SDK, replaced by VS Graphics Debugger
VS Graphics Debugger|Microsoft|Win| Win| 9~11.1 | | Bundled with VS 2012 pro, [link](http://msdn.microsoft.com/en-us/library/hh315751.aspx)
GLIntercept| open-source | Win | Win |     | OpenGL    | [link](https://code.google.com/p/glintercept/)
Nsight | NVIDIA | desktop-OS| desktop-OS | 9/11| OpenGL 4.2   | Also supprots OpenCL/CUDA/C++ AMP, Needs Visual Studio / Eclipse, [link](http://www.nvidia.com/object/nsight.html)
CodeXL|AMD|Win/Linux| Win/Linux |  | OpenGL    | formly [gDEBugger](http://www.gremedy.com/), also supports OpenCL,   [link](http://developer.amd.com/tools-and-sdks/heterogeneous-computing/codexl/)
GPU PerfStudio|AMD |Win|Win|10/11 |OpenGL| [link](http://developer.amd.com/tools-and-sdks/graphics-development/gpu-perfstudio-2/)
GPA-d3d| Intel |Windows | Windows |9~11|   | [link](http://software.intel.com/en-us/vcsource/tools/intel-gpa)

**Android graphics debugger**

Tool | Vendor | Host | Device | OpenGL | Comment 
---  | ---   | ---  |  ---  | ---  |   ---     | ---     
Android-glTracer| Google| ES | [link](http://developer.android.com/tools/help/gltracer.html)
Mali Graphics Debugger|ARM|Win/Linux| Android/Linux | ES | Also supports OpenCL, [link](http://malideveloper.arm.com/develop-for-mali/tools/mali-graphics-debugger/)
PVRTrace/PVRTune | Imagination| desktop-OS | iOS/Android/PC |  ES | [link](http://www.imgtec.com/powervr/insider/sdkdownloads/index.asp)
PerfHUD ES| NVIDIA | desktop-OS | Tegra-Android | ES | [link](https://developer.nvidia.com/nvidia-perfhud-es )
Tegra Graphics Debugger | NVIDIA | desktop-OS | Tegra K1 |   4.x / ES | [link](https://developer.nvidia.com/tegra-graphics-debugger)
GPA-es| Intel |desktop-OS | Atom-Android | ES | [link](http://software.intel.com/en-us/vcsource/tools/intel-gpa)

**WebGL debugger**

Tool | Vendor | Browser | Comment 
---  | ---   | ---  |  ---  | ---  |   ---     | ---     
WebGL-Inspector| open-source | any |  [link](http://benvanik.github.io/WebGL-Inspector)
Chrome Canvas Inspector | Google | Chrome | [intro](http://www.html5rocks.com/en/tutorials/canvas/inspection/)
Google Web Tracing Framework | Google | any | [github](http://google.github.io/tracing-framework/)
Firefox WebGL Shader Editor | Mozilla | Firefox | [intro](https://hacks.mozilla.org/2013/11/live-editing-webgl-shaders-with-firefox-developer-tools/)

**My personal favorites:**

* GPA for D3D
* GLIntercept && gDebugger for OpenGL
* Android-glTracer for OpenGL-ES
* Chrome Canvas Inspector for WebGL
