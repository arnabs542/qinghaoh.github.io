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

[Shift 2D Grid][shift-2d-grid]

[rotate-array]: https://leetcode.com/problems/rotate-array/
[shift-2d-grid]: https://leetcode.com/problems/shift-2d-grid/
