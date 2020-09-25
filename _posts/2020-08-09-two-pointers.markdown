---
layout: post
title:  "Two Pointers"
tags: array
---
[Remove All Adjacent Duplicates in String][remove-all-adjacent-duplicates-in-string]

[Backspace String Compare][backspace-string-compare]

Scan from the end of the Strings.

{% highlight java %}
public boolean backspaceCompare(String S, String T) {
    int i = S.length() - 1, j = T.length() - 1;
    int back = 0;
    while (true) {
        back = 0;
        while (i >= 0 && (back > 0 || S.charAt(i) == '#')) {
            back += S.charAt(i) == '#' ? 1 : -1;
            i--;
        }
        back = 0;
        while (j >= 0 && (back > 0 || T.charAt(j) == '#')) {
            back += T.charAt(j) == '#' ? 1 : -1;
            j--;
        }
        if (i >= 0 && j >= 0 && S.charAt(i) == T.charAt(j)) {
            i--;
            j--;
        } else {
            break;
        }
    }
    return i == -1 && j == -1;
}
{% endhighlight %}

[Trapping Rain Water][trapping-rain-water]

{% highlight java %}
public int trap(int[] height) {
    int water = 0, leftMax = 0, rightMax = 0;
    int left = 0, right = height.length - 1;
    while (left < right) {
        // moves pointer of one side if its height is lower
        // this ensures left and right meet at max height
        // in this way, we avoid one pass to find the max height
        if (height[left] < height[right]) {
            leftMax = Math.max(leftMax, height[left]);
            water += leftMax - height[left++];
        } else {
            rightMax = Math.max(rightMax, height[right]);
            water += rightMax - height[right--];
        }
    }

    return water;
}
{% endhighlight %}

[backspace-string-compare]: https://leetcode.com/problems/backspace-string-compare/
[remove-all-adjacent-duplicates-in-string]: https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/
[trapping-rain-water]: https://leetcode.com/problems/trapping-rain-water/
