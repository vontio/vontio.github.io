---
layout: post
category : ios
tags : [UIStoryboardSegue]
---
{% include JB/setup %}

iOS unwind segue not working fix
============================

## REMARK

- `IBAction func unwindToHome(segue :UIStoryboardSegue)` this function is added to the `Destination` view controller
- if `Destination` is UINavigationController rootViewController then add this function to to the UINavigationController instead of the rootViewController
- button is link the the exit icon of the parent view controller with `unwindToHome`

