---
layout: post
title: A naive approach to translating P5 project to OF project
---

{{ page.title }}
================

There is no one-for-all solution to automatically translate a processing sketch to a openFrameworks one.

Because:

1. P5 is Java and OF is C++.
2. P5 hides the class from users, while OF requires understanding of class and objects.
3. Different 3rd party libraries.

The following header file is a naive approach to this hard problem, however it does satisfy some find-replace tasks. 

Enjoy!

processing.h

    #pragma once
    
    #define millis() ofGetSystemTime()
    #define random(a,b) ofRandom(a,b)
    #define color ofColor
    #define strokeWeight(w) ofSetLineWidth(w)
    #define stroke(clr) ofSetColor(clr)
    #define rect(a,b,c,d) ofRect(a,b,c,d)
    #define vertex(x,y) ofVertex(x,y)
    #define frameCount ofGetFrameNum()
    #define String std::string
    #define background(r,g,b) ofBackground(r,g,b)
    #define LINES
    #define beginShape(a) ofBeginShape()
    #define endShape(b) ofEndShape(b)
    #define final const
    #define boolean bool
    
    #undef class
    #define class struct
    
    #define println(str) printf("%s\n", str)
    
    inline float sq(float n){return n*n;}
