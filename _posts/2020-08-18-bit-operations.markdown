---
layout: post
title:  "Bit Operations"
tags: bit
---
[Binary Number with Alternating Bits][binary-number-with-alternating-bits]

{% highlight java %}
public boolean hasAlternatingBits(int n) {
    return (n & (n >> 1)) == 0 && (n | (n >> 2)) == n;
}
{% endhighlight %}

[binary-number-with-alternating-bits]: https://leetcode.com/problems/binary-number-with-alternating-bits/
