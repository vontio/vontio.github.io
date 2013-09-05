---
layout: post
tagline: "sublime text manipulate trick"
category : sublime
tags : [sublime,text manipulate]
---
{% include JB/setup %}

sublime text manipulate tricks
============================

**for sublime2 install `Invert Selection` package first**

## line filters remove useless line

- `Ctrl+F` open find window
- enable regex `.*` mark
- find all using `.*your_filter_string.*\n`
- goto menu `Selection` then `Invert Selection`
- delete selections

## delete useless text

- `Ctrl+F` open replace window
- enable regex `.*` mark
- find all using `your_unwant_string_regex`
- delete selections

## sort

- goto menu `Edit` then `Sort Lines`

## uniq lines

- goto menu `Edit` then `Permute Lines` then `Unique`