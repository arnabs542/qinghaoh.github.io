---
layout: post
title:  "Mathematical Induction"
tags: math
usemathjax: true
---
# Arrangement

[Global and Local Inversions][global-and-local-inversions]

If the 0 occurs at index 2 or greater, then `A[0] > A[2] = 0` is a non-local inversion. So 0 can only occur at index 0 or 1.

If `A[1] = 0`, then we must have `A[0] = 1` otherwise `A[0] > A[j] = 1` is a non-local inversion.

Otherwise, `A[0] = 0`.

In summary, the possibilities are:
* A = [0] + (ideal permutation of 1...n-1)
* A = [1, 0] + (ideal permutation of 2...n-1).

A necessary and sufficient condition is: `Math.abs(A[i] - i) <= 1`. So we check this for every i.

{% highlight java %}
public boolean isIdealPermutation(int[] A) {
    for (int i = 0; i < A.length; i++) {
        if (Math.abs(A[i] - i) > 1) {
            return false;
        }
    }
    return true;
}
{% endhighlight %}

[global-and-local-inversions]: https://leetcode.com/problems/global-and-local-inversions/
