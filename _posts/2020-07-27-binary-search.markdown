---
layout: post
title:  "Binary Search"
---
# Template
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

There can be variants of this templatekest Rows in a Matrix. For example: [First Bad Version][first-bad-version], [The K Weakest Rows in a Matrix][the-k-weakest-rows-in-a-matrix]

* `low <= high`, `low < high`, ...
* `low = mid + 1`, `low = mid`, ...
* `high = mid - 1`, `high = mid`, ...
* `return -1`, `return low`, `return high`, ...

Don't try to remember all these. The easiest way is to test your code with these examples: `[0]`, `[0, 1]` and `[0, 1, 2]`. Always make sure:
* The range shrinks
* No dead loop
* Returns the right thing

# Variants
[Search Insert Position][search-insert-position]

{% highlight java %}
public int searchInsert(int[] nums, int target) {
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
    return low;
}
{% endhighlight %}

[Find K Closest Elements][find-k-closest-elements]

# Java
## Arrays
[public static \<T\> int binarySearch(T\[\] a, int fromIndex, int toIndex, T key, Comparator\<? super T\> c)](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#binarySearch-T:A-int-int-T-java.util.Comparator-)

The *insertion point* is defined as the point at which the key would be inserted into the array: the index of the first element in the range **greater** than the key, or toIndex if all elements in the range are less than the specified key.

## Collections
[public static \<T\> int binarySearch(List\<? extends T\> list, T key, Comparator\<? super T\> c)](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#binarySearch-java.util.List-T-java.util.Comparator-)

[binary-search]: https://leetcode.com/problems/binary-search/
[find-k-closest-elements]: https://leetcode.com/problems/find-k-closest-elements/
[first-bad-version]: https://leetcode.com/problems/first-bad-version/
[search-insert-position]: https://leetcode.com/problems/search-insert-position/
[the-k-weakest-rows-in-a-matrix]: https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/
