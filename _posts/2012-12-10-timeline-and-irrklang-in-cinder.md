---
layout: post
title: 在 Cinder 中使用 timeline 实现复杂的声音播放逻辑
---

{{ page.title }}
================

本文[首发](http://www.hudo.it/index.php/topic,495.0.html)于 hudo.it 社区

----------------------

在本文中，我们将实现的功能有：

- 使用 irrklang 中间件进行音频播放
- 使用 boost::filesystem 进行文件夹内所有歌曲的遍历
- 使用 timeline 进行音量的淡入
- 使用 timeline 进行音量的淡出
- irrklang 的回调函数
- 按任意键切歌
- 音量可视化

先给出源代码的[github 地址](https://github.com/vinjn/CreativeCoding/blob/master/_cinder_app/irrKlangBasic/src/irrKlangBasicApp.cpp)，比较长，比较复杂。

使用 irrklang 中间件进行音频播放
---
irrklang 是一个音频播放的跨平台 API，www.ambiera.com/irrklang，
它的作者与非著名开源图像引擎 irrlicht 的作者是同一个德国人 Nikolaus Gebhardt。   
irrlicht 在德语中是光的意思，而 klang 是声音的意思。   

言归正传，我们知道 Cinder 是支持音频播放的，位于 ci::audio 名字空间下   
在 Windows 平台是基于 DirectX 中的 DXAudio 实现的，但是这个实现很有问题，多番试验后，我放弃了。   

irrklang 的主管类是 irrklang::ISoundEngine，每个声音文件对应于一个 irrklang::ISoundSource 对象，真正进行播放的声音是 irrklang::ISound。   
注意，多个irrklang::ISound 可以指向同一个 irrklang::ISoundSource，因为它是声音的源头。

声音源的载入发生在程序初始化阶段，即 setup() 中，但是此时并不会播放声音，只是增加声音源。


	irrklang::ISoundSource* src = mSoundEngine->addSoundSourceFromFile(
	    pathName.string().c_str(), 
	    irrklang::ESM_AUTO_DETECT,
	    preload);


播放声音的函数在此，我们需要指定先前载入的声音源 source。

getNextSoundSource() 只是个返回随机声音源的函数，详情参考github代码。


	irrklang::ISoundSource* source = getNextSoundSource();
	bool loop = false; // 非loop模式，只播放一次
	bool pause = false; // 默认就开始播放，不暂停
	bool track = true; // 我们需要返回这个播放的sound，需要跟踪（track）它的状态
	mCurrentSound = mSoundEngine->play2D( source, loop, pause, track );


使用 boost::filesystem 进行文件夹内所有歌曲的遍历
---
为了程序可以自适应所有情况，我们希望指定一个文件夹路径后，可以播放文件夹内所有编码支持的歌曲。   
这时候 boost::filesystem 就发挥作用啦，当然在 Cinder 里，我们给它取了个小名叫 fs，即   

	namespace fs = boost::filesystem;


因此可以用 fs 来取代长长的一串名字。遍历所有声音文件并添加声音源的代码如下：

	fs::path root = getAssetPath("./"); // 设置根目录为 assets 文件夹   
	fs::directory_iterator end_iter;   
	for (fs::directory_iterator dir_iter(root); dir_iter != end_iter; ++dir_iter) // 遍历
	{   
	    if (fs::is_regular_file(*dir_iter) ) // 如果是个普通的文件
	    {
	        loadAudio(*dir_iter); // 那么加入到声音源中，此处未判断这是否是个声音文件……请读者自行研究实习
	    }
	}


使用 timeline 进行音量的淡入
---
timeline 是 Cinder_0.8.4 推出的神 feature，支持各种**只要想得到就能做得到**的功能，比如：   
在3秒内将半径从0变大到10，同时坐标从（0,10）移动到（300,300），完成后反过来进行一遍，并将这个过程一直持续下去。

我们这里的代码如下，实现了在4秒内将音量从0（静音）变到1.0（满音），easeInOutQuad 是一个确定变化如何扭曲进行的函数。

	mVolume = 0.0f;
	timeline().apply( &mVolume, 1.0f, 4.0f, easeInOutQuad );// fade in

你光这么写当然不可能实现声音淡入，这淡入的只是mVolume这个变量   
还需要设置sound的值，即：  

	mCurrentSound->setVolume( mVolume );
	

使用 timeline 进行音量的淡出
---
淡出比较复杂些，需要根据声音的播放长度进行自适应，因此首先要得到声音的播放长度:   
这里还做了些判断，虽然4秒是个不错的渐变持续时间，但是有没有想过只有1秒钟的声音肿么办？   
所以将进行特殊处理   
timeline().appendTo() 函数可以将这次渐变过程添加到之前的过程后面，   
即我们先fadeIn，mVolume从0到1，它持续 duration 秒   
再过了 length - duration * 2 秒， 我们开始fadeOut，mVolume从1到0   

	irrklang::ISoundSource* source = getNextSoundSource();
	float length = source->getPlayLength() * 0.001f;
	float duration = math<float>::min( length * 0.4f, 4.0f);
	timeline().appendTo( &mVolume, 1.0f, duration, easeInOutQuad ).delay( length - duration * 2 );// delayed fade out


irrklang 的回调函数
---
淡入淡出都有了，下面要做的是播放结束后自动切歌，这需要使用 irrklang 提供的一个回调接口   

	irrklang::ISoundStopEventReceiver
  
很显然，就是当声音播放停止后会调用这个接口，我们的实现如下：
	
	struct SoundStopCallback : public irrklang::ISoundStopEventReceiver
	{
	    void OnSoundStopped(irrklang::ISound* sound, irrklang::E_STOP_EVENT_CAUSE reason, void* userData)
	    {
	        irrKlangBasicApp* app = reinterpret_cast<irrKlangBasicApp*>( userData );
	        app::console() << "Sound stopped: " << sound->getSoundSource()->getName() << std::endl;
	        if (app)
	            app->setupNewSound();
	    }
	}mSoundStopCallback;

很简单，先是输出该声音的名称，然后再设置新的声音。

按任意键切歌
---
有了 SoundStopCallback 这个回调函数，其实切歌的逻辑就很明朗了，只需要将当前歌曲停止，那么回调函数就被运行。   
代码非常非常简单，就一行：  

	void keyDown( KeyEvent event )
	{
	    mCurrentSound->stop();
	}

音量可视化
---
lmap 函数于 Processing 中的 map 函数功能完全相同，但是 map 在 C++ 中是 STL 容器类，因此很猥琐地加了个 l 在 map 前面。  
lmap 用于将变量映射到新的取值范围内，比如mVolume的原取值范围是[0, 1]，经过这变换，就变成了[getWindowHeight(), 0]。  

	void draw()
	{
	    gl::clear();
	    gl::drawSolidCircle( Vec2f( 
	        lmap<float>( mPan, MIN_PAN, MAX_PAN, 0, getWindowWidth() ), 
	        lmap<float>( mVolume, MIN_VOLUME, MAX_VOLUME, getWindowHeight(), 0 )), 
	        10 );
	}

mPan 也是个非常有趣的功能，简要介绍下，用法如下：  

	mCurrentSound->setPan( mPan );

mPan的取值范围是[-1, 1]，-1 的时候你用耳机听，那么声音仿佛位于你的左侧，而 1 的时候则仿佛位于右侧。神奇吧！



