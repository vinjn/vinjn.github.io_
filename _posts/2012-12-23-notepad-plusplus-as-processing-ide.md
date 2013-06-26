---
layout: post
title: 使用 Notepad++ 打造 Processing 开发环境
---

{{ page.title }}
================

本文[首发](http://www.hudo.it/index.php/topic,520.0.html)于 hudo.it 社区   
后来我发现 github 上存在很多 Processing 的开发环境

------------------

Processing/p5 的开发环境，简称为PDE，适合做快速开发（Sketching），但是维护项目时痛苦、臃肿、蛋疼……   
使用外置编辑器来作为 p5 开发环境是作为程序员的我第一时间就想做的
在很久以前，这可以通过一种蛋疼的方式实现（略去不表）   

但是现在，我们有了 Processing-java.exe 这么个命令行工具，虽然没有阅读官方的使用说明，但是从 help 中我们不难得知它的用法   
	
	e:\processing-2.0b6>processing-java.exe
	
	Command line edition for Processing 0214 (Java Mode)
	
	--help               Show this help text. Congratulations.
	
	--sketch=<name>      Specify the sketch folder (required)
	--output=<name>      Specify the output folder (required and
	                     cannot be the same as the sketch folder.)
	
	--force             The sketch will not build if the output
	                     folder already exists, because the contents
	                     will be replaced. This option erases the
	                     folder first. Use with extreme caution!
	
	--build              Preprocess and compile a sketch into .class files.
	--run                Preprocess, compile, and run a sketch.
	--present            Preprocess, compile, and run a sketch full screen.
	
	--export             Export an application.
	--platform           Specify the platform (export to application only).
	                     Should be one of 'windows', 'macosx', or 'linux'.
	--bits               Must be specified if libraries are used that are
	                     32- or 64-bit specific such as the OpenGL library.
	                     Otherwise specify 0 or leave it out.

假设我们的 Sketch 文件夹是 D:\Array\，期望的 Output 文件夹是D:\Array\Build   
那么对应的命令行为

	e:\processing-2.0b6\processing-java.exe --sketch=D:\Array\ --output=D:\Array\Build --force --run
如果一切顺利，没有错误发生，那么就将运行指定 Sketch 文件夹下的代码。

我们不会满足于不停的敲打命令行，是吧，既然有了命令行代码，那么自然要将它与用得顺手的编辑器相结合了

-----

比如 Notepad++   

- 首先，我们要安装 NppExec 的插件，然后进入 Plugins->NppExec->Execute，在文本框中输入 
	
		e:\processing-2.0b6\processing-java.exe -- sketch="$(CURRENT_DIRECTORY)" --output="$(CURRENT_DIRECTORY)/build" --run --force
并点击 Save 按钮，保存为 p5_run（可随意取名字）

- 然后，进入 Plugins->NppExec->Advanced Options，在左下角的 Menu item->Associated script 下拉列表中选中 p5_run（或你取的名字），并点击 Add/Modify 按钮使 p5_run 出现在左上角的 Menu items 中。 

- 接着，进入 Run->Modify Shortcut/Delete Command...（即菜单中最底下的选项）->Plugin commands，你将看到一个很大的下拉条，找到 p5_run（很有可能在第40多的位置），双击，选择你想要的快捷键，我使用的是 Ctrl+F7，确认退出。

中间可能需要重启一次。

Happy Coding！

补充
---
上面的 script 只是实现了运行的功能，如果你想导出一个带可执行文件的完整工程，怎么办呢？看下面

	e:\processing-2.0b6\processing-java.exe --sketch="$(CURRENT_DIRECTORY)" --output="$(CURRENT_DIRECTORY)/build" --export --force --bits=32
这是32位版本的，如果是64位的也可以，只要

	e:\processing-2.0b6\processing-java.exe --sketch="$(CURRENT_DIRECTORY)" --output="$(CURRENT_DIRECTORY)/build" --export --force --bits=64


