---
layout: post
title:  "Repeated Element"
tags: array
---
[N-Repeated Element in Size 2N Array][n-repeated-element-in-size-2n-array]

`k` repeated element in size `n` array:

Find the minimum size `m`, so that there exists at least one subarray with size `m`, and it is guaranteed to contain more than one repeated element.

In this example, the size is `2N / N + 1 = 3`.

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

Or equivalently,

{% highlight java %}
public int repeatedNTimes(int[] A) {
    for (int i = 2; i < A.length; ++i) {
        if (A[i] == A[i - 1] || A[i] == A[i - 2]) {
            return A[i];
        }  
    }
    return A[0];
}
{% endhighlight %}

We can of course expand this subarray window to, for example, 4.

[Guess the Majority in a Hidden Array][guess-the-majority-in-a-hidden-array]

{% highlight java %}
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
[guess-the-majority-in-a-hidden-array]: https://leetcode.com/problems/guess-the-majority-in-a-hidden-array/
[n-repeated-element-in-size-2n-array]: https://leetcode.com/problems/n-repeated-element-in-size-2n-array/
