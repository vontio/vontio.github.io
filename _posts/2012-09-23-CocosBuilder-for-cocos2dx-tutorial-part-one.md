---
layout: post
tagline: "CocosBuilder-for-cocos2dx-tutorial-part-one"
category : cocos2d-x
tags : [cocos2d-x,cocosbuilder]
---
{% include JB/setup %}

I. create a empty HelloCocos2dx project

use the cocos2dx template

II. setup CocosBuilder

create a CocosBuilder project called HelloCocos2dx
in CocosBuilder File->new File to create a Layer file HelloLayer
add a CCLabelTTF,make it center,in the property panel Code Connections section change Don't assign to Owner var,and input txtHello,change Sample Text to "Hello From CocosBuilder"
add a CCMenu,and add a CCMenuItemImage to the CCMenu,in the CCMenuItemImage property panel CCMenuItem section,input onSayHello: for Selector field and change target to Owner,in the CCMenuItemImage section select a image for normal state.
in CocosBuilder File->Project Settings set Publish Directory to xcode project Resources/UI,uncheck all the checkbox,now publish
III. connect with code

create a cpp class called LayerHello
LayerHello.h
{% highlight cpp %}
#ifndef __LayerHello__
#define __LayerHello__
#include "cocos2d.h"
#include "CCBMemberVariableAssigner.h"
#include "CCBSelectorResolver.h"
#include "CCLabelTTF.h"
USING_NS_CC_EXT;
class LayerHello : public cocos2d::CCLayer,public CCBMemberVariableAssigner,public CCBSelectorResolver
{
	public://================UI injections
	CCLabelTTF *txtHello;
	void onSayHello(CCObject *sender);
	//================

	virtual bool init();

	virtual bool onAssignCCBMemberVariable(CCObject * pTarget, cocos2d::CCString * pMemberVariableName, CCNode * pNode)
	{
		CCB_MEMBERVARIABLEASSIGNER_GLUE(this,"txtHello", CCLabelTTF,txtHello);
		return false;
	}
	virtual cocos2d::SEL_MenuHandler onResolveCCBCCMenuItemSelector(CCObject * pTarget, CCString * pSelectorName)
	{
		CCB_SELECTORRESOLVER_CCMENUITEM_GLUE(this,"onSayHello:",LayerHello::onSayHello);
		return NULL;
	}
	virtual cocos2d::extension::SEL_CCControlHandler onResolveCCBCCControlSelector(CCObject * pTarget, CCString * pSelectorName)
	{
		return NULL;
	}

	CREATE_FUNC(LayerHello);
};
#endif /* defined(__LayerHello__) */
{% endhighlight %}
LayerHello.cpp
{% highlight cpp %}

#include "LayerIncActorInfo.h"
#include "CCBReader.h"
#include "CCNodeLoaderLibrary.h"

LayerHello:: LayerHello()
: txtHello(0)
{

}

bool LayerHello::init()
{
	bool bRet = false;
	do
	{
	CC_BREAK_IF(! CCLayer::init());

	CCNodeLoaderLibrary * ccNodeLoaderLibrary = CCNodeLoaderLibrary::sharedCCNodeLoaderLibrary();
	CCBReader * ccbReader = new CCBReader(ccNodeLoaderLibrary,this,this);
	CCNode * node = ccbReader->readNodeGraphFromFile("UI/", "HelloLayer.ccbi", this);
	this->addChild(node);

	bRet = true;
	} while (0);

	return bRet;
}

void LayerHello::onSayHello(CCObject *sender)
{
	txtHello->setString("Hello From Code");
}
{% endhighlight %}
add LayerHello to your scene,click run to test it
in you game,click the CCMenuItemImage,the label should change to "Hello From Code"