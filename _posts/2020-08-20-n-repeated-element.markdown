---
layout: post
title:  " N-Repeated Element"
tags: array
---
[N-Repeated Element in Size 2N Array][n-repeated-element-in-size-2n-array]

Find the minimum size of a subarray which must have a repeated element. In this example, the size is `4`.

{% highlight java %}
public int repeatedNTimes(int[] A) {
    for (int i = 0; i < A.length; i++) {
        for (int j = 1; j <= 3 && i + j < A.length; j++) {
            if (A[i] == A[i + j]) {
                return A[i];
            }
        }
    }
    return 0;
}
{% endhighlight %}

[n-repeated-element-in-size-2n-array]: https://leetcode.com/problems/n-repeated-element-in-size-2n-array/
