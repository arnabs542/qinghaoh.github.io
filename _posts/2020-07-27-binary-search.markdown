---
layout: post
title:  "Binary Search"
---
[Binary Search][binary-search]

{% highlight java %}
public int search(int[] nums, int target) {
    int low = 0, high = nums.length - 1;
    while (low <= high) {
        int mid = (high - low) / 2 + low;
        if (nums[mid] == target) {
            return mid;
        }
        if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return -1;
}
{% endhighlight %}

There can be variants of this template. For example [First Bad Version][first-bad-version].
* `low <= high`, `low < high`, ...
* `low = mid + 1`, `low = mid`, ...
* `high = mid - 1`, `high = mid`, ...
* `return -1`, `return low`, ...

Don't try to remember all these. The easiest way is to test your code with these examples: `[0]`, `[0, 1]` and `[0, 1, 2]`. Always make sure:
* The range shrinks
* No dead loop
* Returns the right thing

[binary-search]: https://leetcode.com/problems/binary-search/
[first-bad-version]: https://leetcode.com/problems/first-bad-version/
