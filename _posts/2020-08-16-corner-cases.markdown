---
layout: post
title:  "Corner Cases"
---
[Reverse Integer][reverse-integer]

{% highlight java %}
public int reverse(int x) {
    int res = 0;
    while (x != 0) {
        int d = x % 10;
        int tmpRes = 10 * res + d;
        // avoid overflow
        if ((tmpRes - d) / 10 != res) {
            return 0;
        }
        res = tmpRes;
        x /= 10;
    }
    return res;
}
{% endhighlight %}

[reverse-integer]: https://leetcode.com/problems/reverse-integer/
