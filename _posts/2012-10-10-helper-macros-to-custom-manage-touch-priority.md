---
layout: post
tagline: "create a modal panel in 4 macros"
category : cocos2d-x
tags : [cocos2d-x]
---
{% include JB/setup %}

**Why**

touch priority is not same as view depth,manage touch priority in cocos2dx is more difficult.So,here comes an idea ,in every panel use macros to define the touch priority,and container define touch priory of this panel recursively,finally you will have only one touch entrance,each touch event will pass to this children to handle it in predefined priority.

**Code**

{% highlight cpp %}

#define CUSTOM_TOUCH_PRIORITY_INIT(_removeChildTouch) \
	__customTouchPriorityInit(_removeChildTouch)
#define CUSTOM_TOUCH_PRIORITY_BEGIN(_priority) \
	CCTouchDelegate *__currentTouchItem;bool __isChildTouchBegin; \
	std::vector<CCTouchDelegate*> __customTouchChildren; \
	virtual void registerWithTouchDispatcher(){ \
		CCDirector::sharedDirector()->getTouchDispatcher()->addTargetedDelegate(this, _priority, true);\
	} \
	virtual bool ccTouchBegan(CCTouch *pTouch, CCEvent *pEvent){ \
		if (!this->isVisible()){return false;} \
		for (CCNode *c = this->m_pParent; c != NULL; c = c->getParent()) \
		if (c->isVisible() == false){return false;} \
		if (__isChildTouchBegin){return false;} \
		std::vector<CCTouchDelegate*>::iterator itr = __customTouchChildren.begin();\
		while (itr!=__customTouchChildren.end()){ \
			if ((*itr)->ccTouchBegan(pTouch,pEvent)){ \
				__currentTouchItem = *itr; \
				__isChildTouchBegin = true; \
				break; \
			} \
			++itr; \
		} \
		return true; \
	} \
	void __customTouchPriorityInit(bool removeChildTouch){ \
		__currentTouchItem = NULL;__isChildTouchBegin = false;
#define CUSTOM_TOUCH_PRIORITY_ITEM(_item) \
	__customTouchChildren.push_back(_item)
#define CUSTOM_TOUCH_PRIORITY_END() \
	setTouchEnabled(true); \
	if(removeChildTouch){ \
		std::vector<CCTouchDelegate*>::iterator itr = __customTouchChildren.begin(); \
		while(itr!=__customTouchChildren.end()) \
		{((CCLayer*)(*itr))->setTouchEnabled(false);++itr;}} \
	} \
	virtual void ccTouchMoved(CCTouch *pTouch, CCEvent *pEvent) \
	{ \
		if (__isChildTouchBegin) \
			__currentTouchItem->ccTouchMoved(pTouch, pEvent); \
	} \
	\
	virtual void ccTouchEnded(CCTouch *pTouch, CCEvent *pEvent) \
	{ \
		if (__isChildTouchBegin) \
		{ \
			__currentTouchItem->ccTouchEnded(pTouch, pEvent); \
			__isChildTouchBegin = false; \
		} \
	} \
	\
	virtual void ccTouchCancelled(CCTouch *pTouch, CCEvent *pEvent) \
	{ \
		if (__isChildTouchBegin) \
		{ \
			__currentTouchItem->ccTouchCancelled(pTouch, pEvent); \
			__isChildTouchBegin = false; \
		} \
	}

{% endhighlight %}

**How to use**

create a CustomTouchPriority.h copy macros above into it,and include in your class file.

{% highlight cpp %}
//in class header file add these lines
public:

CUSTOM_TOUCH_PRIORITY_BEGIN(TOUCH_PRIORITY_MODAL_PANEL);//give a very low priory here TOUCH_PRIORITY_MODAL_PANEL = -99999999
CUSTOM_TOUCH_PRIORITY_ITEM(scrollView);//first priority
CUSTOM_TOUCH_PRIORITY_ITEM(menu);//second etc..
CUSTOM_TOUCH_PRIORITY_END();
{% endhighlight %}

{% highlight cpp %}
//in implements file add this line in init function to make it work
CUSTOM_TOUCH_PRIORITY_INIT(true);//true will set children touch = false,so the touch event is managed.
{% endhighlight %}

**when this panel is non visible remember to setTouchEnabled(false)**

