---
layout: post
title:  "N-Repeated Element"
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

## Sliding window

[Detect Pattern of Length M Repeated K or More Times][detect-pattern-of-length-m-repeated-k-or-more-times]

{% highlight java %}
public boolean containsPattern(int[] arr, int m, int k) {
    for (int i = 0, count = 0; i + m < arr.length; i++) {
        if (arr[i] != arr[i + m]) {
            count = 0;
        } else if (++count == (k - 1) * m) {
            return true;
        }
    }
    return false;
}
{% endhighlight %}

[detect-pattern-of-length-m-repeated-k-or-more-times]: https://leetcode.com/problems/detect-pattern-of-length-m-repeated-k-or-more-times/
[n-repeated-element-in-size-2n-array]: https://leetcode.com/problems/n-repeated-element-in-size-2n-array/
