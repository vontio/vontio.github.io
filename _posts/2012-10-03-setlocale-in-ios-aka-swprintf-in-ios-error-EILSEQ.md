---
layout: post
category : cocos2d-x
tags : [cocos2d-x,iOS]
---
{% include JB/setup %}

when work with swprintf (or wchar functions),you probably want set locale to en_US.UTF-8,but in ios there is no en_US.UTF-8 locale,this is what you should to

**How**

- copy en_US.UTF-8 to your ios project resource path,and add to project
{% highlight bash %}
sudo copy -r /usr/share/locale/en_US.UTF-8/ your_ios_resource_path/en_US.UTF-8/
{% endhighlight %}
- set locale in AppController applicationDidFinishLaunchingWithOptions
{% highlight objc %}
NSString* resources = [ [ NSBundle mainBundle ] resourcePath ];
setenv("PATH_LOCALE", [resources cStringUsingEncoding:1], 1);
setlocale( LC_CTYPE, "en_US.UTF-8" );
{% endhighlight %}
- when format string use %ls instead