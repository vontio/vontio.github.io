---
layout: post
tagline: "Show 2dtoolkit sprite on NGUI panel"
category : Unity
tags : [Unity]
---
{% include JB/setup %}

#### idea

use a third camera to show the above ui tk2dSprite

#### setting

layers(camera depth) : Default(1),UILayer(10),AboveUILayer(20)

#### about clip

- Add UIViewport to AboveUILayer camera
- set source camera to UILayer camera
- add topleft,bottomright to clipping panel

known issues if clipped:

if tk2dSprite is a child of UIWidget,and UIWidget have a scale animation,these two will not scale like one object.
because AboveUILayer camera viewport rect and orthographic size different from UILayer camera
