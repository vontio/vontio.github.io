---
layout: post
category : cocos2d-x
tags : [cocos2d-x]
---
{% include JB/setup %}

**Why**

CCScrollView has a lower priority to handle touch event,if there is a CCMenuItemImage the touch event is handled by CCMenuItemImage and never goes to CCScrollView

**How**

- Create a new class called CCScrollViewEx extends CCScrollView
- set a higher priority of handling touch event
- detect click event and simulate a click event to others
- disable ccmenu click outside view rect

**Code  CCScrollViewEx**

{% highlight cpp %}

//header

class CCScrollViewEx : public CCScrollView
{
public:
	virtual bool init();
	virtual void registerWithTouchDispatcher();
	virtual bool ccTouchBegan(CCTouch *pTouch, CCEvent *pEvent);
	virtual void ccTouchEnded(CCTouch *pTouch, CCEvent *pEvent);
	void setMenuContent(CCMenu *menu){menuContent = menu;}
	CREATE_FUNC(CCScrollViewEx);
protected:
	CCPoint pressPoint;
	CCMenu *menuContent;
};

{% endhighlight %}

{% highlight cpp %}

//implements

bool CCScrollViewEx::init(){
bool bRet = false;
do
{
	CC_BREAK_IF(! CCScrollView::init());
		bRet = true;
	}while(0);
	return bRet;
}

void CCScrollViewEx::registerWithTouchDispatcher()
{
	CCDirector::sharedDirector()->getTouchDispatcher()->addTargetedDelegate(this, TOUCH_PRIORITY_POPUP_PANEL, true);
}
bool CCScrollViewEx::ccTouchBegan(CCTouch *pTouch, CCEvent *pEvent)
{
	pressPoint = pTouch->getLocationInView();
	return CCScrollView::ccTouchBegan(pTouch, pEvent);
}
void CCScrollViewEx::ccTouchEnded(CCTouch *pTouch, CCEvent *pEvent)
{
	#define MIN_DISTANCE 10
	CCPoint endPoint = pTouch->getLocationInView();
	float distance = sqrtf((endPoint.x - pressPoint.x) * (endPoint.x - pressPoint.x) + (endPoint.y - pressPoint.y) * (endPoint.y - pressPoint.y));

	if(distance < MIN_DISTANCE)
	{
		if (menuContent)
		{
			bool start = menuContent->ccTouchBegan(pTouch, pEvent);//simulate a click
			if (start)
				menuContent->ccTouchEnded(pTouch, pEvent);
		}
	}
	CCScrollView::ccTouchEnded(pTouch, pEvent);
}

{% endhighlight %}

{% highlight cpp %}
//code CCMenuEx

//create a subclass to handle touch outside the scrollView rect
bool CCMenuEx::ccTouchBegan(CCTouch *touch, CCEvent *event)
{
	if (validTouchRectInWorldSpace.size.width && validTouchRectInWorldSpace.size.height)//we have restriction
	{
		CCPoint touchLocation = touch->getLocation();
		if (!validTouchRectInWorldSpace.containsPoint(touchLocation))
			return false;
	}
	return CCMenu::ccTouchBegan(touch, event);
}

{% endhighlight %}

**remember to call setMenuContent of CCScrollViewEx and set validTouchRectInWorldSpace of CCMenuEx.**

