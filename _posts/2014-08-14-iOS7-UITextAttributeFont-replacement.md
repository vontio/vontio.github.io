---
layout: post
category : ios
---
{% include JB/setup %}

iOS7 UITextAttributeFont replacement
============================

## REMARK

use `NSFontAttributeName`

for example:
change navigation bar title color and font

```
navigationBar.titleTextAttributes = [
    NSForegroundColorAttributeName: UIColor(red: 90 / 255.0, green: 103 / 255.0, blue: 145 / 255.0, alpha: 1.0),
    NSFontAttributeName: UIFont(name: "Heiti SC", size: 24),
]
```
