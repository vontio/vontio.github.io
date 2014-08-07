---
layout: post
category : ios
tags : [UICollectionView]
---
{% include JB/setup %}

UICollectionView cellForItemAtIndexPath not called fix,UICollectionView margin fix
============================

## cellForItemAtIndexPath not called for many reasons

for my problem: cell is not in the visible area of UICollectionView,
because the `Adjust scroll view insets` option of parent view controller is on by default,
this cause automatic add margin to the UICollectionView


## fix

toggle off `Adjust Scroll View Insets` option in parent view controller attribute inspector
or add this code to parent view controller viewDidLoad method:

```
self.automaticallyAdjustsScrollViewInsets = NO;
```
