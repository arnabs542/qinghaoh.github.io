---
layout: post
title:  "Array Rotation"
tags: array
---
[Rotate Array][rotate-array]

![Example](/assets/rotate_array.png)

{% highlight java %}
public void rotate(int[] nums, int k) {
    k %= nums.length;
    int start = 0, count = 0;
    while (count != nums.length) {
        int index = start;
        int curr = nums[index];
        do {
            index = (index + k) % nums.length;
            int tmp = nums[index];
            nums[index] = curr;
            curr = tmp;
            count++;
        } while (index != start);
        start++;
    }
    return;
}
{% endhighlight %}

[rotate-array]: https://leetcode.com/problems/rotate-array/
