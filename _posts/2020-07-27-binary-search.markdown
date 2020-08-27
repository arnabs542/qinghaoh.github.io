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
        int mid = (low + high) >>> 1;
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

There can be variants of this template. For example: [First Bad Version][first-bad-version], [The K Weakest Rows in a Matrix][the-k-weakest-rows-in-a-matrix]

* `low <= high`, `low < high`, ...
* `low = mid + 1`, `low = mid`, ...
* `high = mid - 1`, `high = mid`, ...
* `return -1`, `return low`, `return high`, ...

## Boundary
The initial boundary `[low, high]` should include ***all*** possible answers. When each loop begins, any value within the range `[low, high]` could be the answer.

Don't try to remember all these. The easiest way is to test your code with these examples: `[0]`, `[0, 1]` and `[0, 1, 2]`. Always make sure:
* The range shrinks
* No infinite loop
* Returns the right thing

# Variants
[Search Insert Position][search-insert-position]

{% highlight java %}
public int searchInsert(int[] nums, int target) {
    int low = 0, high = nums.length - 1;
    while (low <= high) {
        int mid = (low + high) >>> 1;
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

Search for the first index from which the `k`-element sliding window starts.

{% highlight java %}
public List<Integer> findClosestElements(int[] arr, int k, int x) {
    int low = 0, high = arr.length - k;
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (x - arr[mid] > arr[mid + k] - x) {
            low = mid + 1;
        } else {
            high = mid;
        }
    }
    return Arrays.stream(arr, low, low + k).boxed().collect(Collectors.toList());
}
{% endhighlight %}

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
