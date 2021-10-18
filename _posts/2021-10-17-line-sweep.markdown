---
layout: post
title:  "Bit Mask"
tags: math
usemathjax: true
---
[Describe the Painting][describe-the-painting]

{% highlight java %}
public List<List<Long>> splitPainting(int[][] segments) { 
    Map<Integer, Long> map = new TreeMap<>();
    for (int[] s : segments) {
        map.put(s[0], map.getOrDefault(s[0], 0l) + s[2]);
        map.put(s[1], map.getOrDefault(s[1], 0l) - s[2]);
    }

    List<List<Long>> painting = new ArrayList<>();
    int prev = 0;
    long color = 0;
    for (int curr : map.keySet()) {
        // color == 0 means this segment is not painted
        if (prev > 0 && color > 0) {
            painting.add(Arrays.asList((long)prev, (long)curr, color));
        }

        color += map.get(curr);
        prev = curr;
    }
    return painting;
}
{% endhighlight %}

[describe-the-painting]: https://leetcode.com/problems/describe-the-painting/
