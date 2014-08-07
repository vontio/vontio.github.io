---
layout: post
category : ios
tags : [UIRefreshControl]
---
{% include JB/setup %}

iOS UIRefreshControl add margin fix
============================

## why

if set UIRefreshControl attributedTitle before refresh the control there will be a top margin

## fix

#### method 1

assign refreshControl after set attributedTitle

```
var refreshControl = UIRefreshControl()
refreshControl.attributedTitle = NSAttributedString(string: "Last:", attributes: [NSForegroundColorAttributeName: UIColor.whiteColor()])
self.refreshControl = refreshControl
```

#### method 2

set attributedTitle in refresh callback
