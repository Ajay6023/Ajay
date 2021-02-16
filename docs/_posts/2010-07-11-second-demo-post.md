---
layout: post
title: Second Demo Post
date: 2020-07-11 13:32:20 +0300
description: You'll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
image: /assets/images/posts/1.jpg
fig-caption: # Add figcaption (optional)
tags: [Holidays, Hawaii]
---

Watermark determines the scan rate policy of the memory management.

Linux kernel as described in my previous writing has 3 major watermarks, High, Min and Low.  This blog is a brief view of the watermark calculation in the Linux system.

1. Use variable from admin window /proc/sys/vm/min_free_kbytes

Converts kbytes unit to page unit.

pages_min = min_free_kbytes >> (PAGE_SHIFT – 10);

2. Calculate total managed pages in each zone except highmem  zone( if it exists )

1
2
3
4
for_each_zone(zone) {
       if(!is_highmem(zone))
            lowmem_pages += zone->managed_pages;
}
 For Each zone calculate the following
3. Calculate fraction for each zone’s pages_min with respect to lowmem_pages.
tmp= ( pages_min / lowmem_pages ) * zone->managed_pages.

Managed_pages are total available page count in a zone that could be used for allocation.
4. Calculate WMARK_MIN for HIGHMEM using following calculation:
min_pages = zone->managed_pages  /  1024
min_pages = MIN( MAX(min_pages, SWAP_CLUSTER_MAX), 128)

5. Else, simply use the tmp variable as min_pages watermark count.

6. Calculate the distance for the rest of the watermarks.
Its the fraction of managed_pages by 10000, scaled by watermark_scale_factor = 10
fraction = ( zone->managed_pages / 10000 )  * watermark_scale_factor
distance = MAX( tmp >> 2,  fraction);

7. Assign watermarks
zone->watermark[WMARK_MIN] = min_pages;
zone->watermark[WMARK_LOW] = min_pages + distance;
zone->watermark[WMARK_HIGH] = min_pages + distance * 2;

Posted onApril 15, 2017
Categorieslinux, Memory Management, ScanRate, Uncategorized
Leave a commenton Empathizing Mammoths Brain: Determining Watermark in Linux


Your new function 
f
d
i
s
c
r
e
t
i
z
e
d
 now could be seen as a vector composed of mostly zeros, except for a small region:

f
d
i
s
c
r
e
t
i
z
e
d
=
[
…
0
,
0
,
1
,
1
,
1
,
1
,
0
,
0
,
…
]
  
(1)
 
Because this is an infinite array, it is hard to know exactly where it “starts” (or where it “ends”). In the introduction to this post I said this was a “problem”, and we had solved it by dropping the two regions composed exclusively by zeroes:

f
d
i
s
c
r
e
t
i
z
e
d
=
[
1
,
1
,
1
,
1
]
  
(2)
 
Of course, we could have retained some of the zeros, if it was for any reason convenient to us. It doesn’t matter much. The main idea here is that we now have a convenient way to represent functions compactly through vectors. This also means that anything that works for vectors (dot products, angles, norms) also should have some interpretation for discrete functions. Think about it!
