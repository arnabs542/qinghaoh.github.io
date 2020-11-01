---
layout: post
title:  "Two Pointers"
tags: array
---
[Remove All Adjacent Duplicates in String II][remove-all-adjacent-duplicates-in-string-ii]

{% highlight java %}
public String removeDuplicates(String s, int k) {
    char[] c = s.toCharArray();
    int[] count = new int[s.length()];  // DP
    int i = 0, j = 0;
    while (j < c.length) {
        c[i] = c[j];
        count[i] = i > 0 && c[i - 1] == c[i] ? count[i - 1] + 1 : 1;
        if (count[i] == k) {
            i -= k;
        }
        i++;
        j++;
    }
    return new String(c, 0, i);
}
{% endhighlight %}

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

[Container With Most Water][container-with-most-water]

{% highlight java %}
public int maxArea(int[] height) {
    int area = 0, left = 0, right = height.length - 1;
    while (left < right) {
        area = Math.max(area, Math.min(height[left], height[right]) * (right - left));
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    return area;
}
{% endhighlight %}

[backspace-string-compare]: https://leetcode.com/problems/backspace-string-compare/
[container-with-most-water]: https://leetcode.com/problems/container-with-most-water/
[remove-all-adjacent-duplicates-in-string-ii]: https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/
[trapping-rain-water]: https://leetcode.com/problems/trapping-rain-water/
