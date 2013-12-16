---
layout: post
tagline: "create plugin"
category : cocos2d-x
tags : [cocos2d-x,cocosbuilder]
---
{% include JB/setup %}

**I. Compile CocosBuilder**

- download source code of cocosbuilder
https://github.com/cocos2d/CocosBuilder
- get submodules of cocosbuilder,run these commands in the cocosbuilder source folder
{% highlight bash %}
git submodule init
git submodule update
{% endhighlight %}
- at this point,you have all sources of cocosbuilder,it's time to compile cocosbuilder
- goto CocosBuilder folder open CocosBuilder.xcodeproj
- change build target to CocosBuilder,if there is no error,you will get CocosBuilder binary in the build folder.If you get errors,please stop and check from step 1.

**II. Create plugin**

- goto Plugin Nodes folder,duplicate CCRotatingSprite Folder,name it to your plugin name, let's call it CCMySprite
- Now, open the project and slowly click twice on the project name in the file view to rename it. Name it to CCMySprite,with popup window click ok.
- click the run button.This will compile the plugin and copy it into CocosBuilder's Plugins folder(inside the app bundle),For testing and debugging,double click the CocosBuilder program in the build folder.You can see the output of the program using Console.app
- go back to xcode,change CCRotatingSprite class to what ever you want,cause I am create a plugin for CCMySprite,so I will rename this class to CCBPMySprite(cocosbuilder name convention)
- now it's time to code for the CCBPMySprite(in cocosbuilder and cocos2dx you may use different class,cause you are too lazy to port your c++ class to obj c),let's just add a simple string property.
- after complete CCBPMySprite class,open CCBPProperties.plist modify inheritsFrom to what ever your class inherits for example CCSprite,and VERY IMPORTANT className to your cocos2dx class name,this time is CCMySprite,and change editorClassName to CCBPMySprite,and a property named string of type Text ( Property Ref , Property Types)


{% highlight xml %}
<dict>
	<key>default</key>
	<string>Sample Text</string>
	<key>displayName</key>
	<string>Set My String</string>
	<key>type</key>
	<string>Text</string>
	<key>name</key>
	<string>string</string>
</dict>
{% endhighlight %}

- goto build folder,right click CocosBuilder -> Show Package Contents,goto Contents/plugins folder delete your plugin folder
- click run button,compile your plugin again,remember to delete plugin folder first(I don't know why it can't override the old one)
- run the CocosBuilder in the build folder to test your plugin,goto step 5 until you complete the plugin.

**III. Parse it**

- create a loader for your plugin,let's call it CCMySpriteLoader,and it should inherit from CCSpriteLoader.(open CCSpriteLoader to learn how to write a loader)
- I have added a string property called string to my CCBPMySprite,so I should parse it by type text
{% highlight cpp %}
void CCMySpriteLoader::onHandlePropTypeText(CCNode * pNode, CCNode * pParent, CCString * pPropertyName, CCString * pText, CCBReader * pCCBReader){
	if(pPropertyName->compare("string") == 0){((CCMySprite*)pNode)->setMyString(pText);//assume you have implement a method called setMyString,just CCLog out}else{CCNodeLoader::onHandlePropTypeText(pNode,pParent,pPropertyName,pText,pCCBReader);
}
{% endhighlight %}
- inject your loader to the parse chain
{% highlight cpp %}
CCNodeLoaderLibrary::sharedCCNodeLoaderLibrary()->registerCCNodeLoader("CCMySprite", CCMySpriteLoader::loader());//it means when parse CCMySprite use CCMySpriteLoader to parse it
{% endhighlight %}
- now I have inject my Editor value of string to my CCMySprite class,at this point the CCMySprite plugin is complete.