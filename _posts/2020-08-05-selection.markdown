---
layout: post
title:  "Selection"
tags: array
---
[Selection algorithm](https://en.wikipedia.org/wiki/Selection_algorithm)

[Kth Largest Element in an Array][kth-largest-element-in-an-array]

## Heap Sort

{% highlight java %}
public int findKthLargest(int[] nums, int k) {
    // min heap
    Queue<Integer> pq = new PriorityQueue<>();
    for (int num : nums) {
        pq.offer(num);

        if (pq.size() > k) {
            pq.poll();    
        }
    }
    return pq.peek();
}
{% endhighlight %}

## Quickselect

[Quickselect](https://en.wikipedia.org/wiki/Quickselect)

Time complexity: 
* Average: `O(n)`
* Worst: `O(n^2)`

{% highlight java %}
public int findKthLargest(int[] nums, int k) {
    return quickSelect(nums, 0, nums.length - 1, k);
}

private int quickSelect(int[] nums, int low, int high, int k) {
    int p = partition(nums, low, high);

    // count of nums greater than or equal to nums[p]
    int count = high - p + 1;
    if (count == k) {
        return nums[p];
    }

    if (count > k) {
        return quickSelect(nums, p + 1, high, k);
    }

    return quickSelect(nums, low, p - 1, k - count);
}

private int partition(int[] nums, int low, int high) {
    int pivot = nums[high];
    int i = low;
    for (int j = low; j < high; j++) {
        if (nums[j] < pivot) {
            swap(nums, i, j);
            i++;
        }
    }
    swap(nums, i, high);
    return i;
}

private void swap(int[] nums, int i, int j) {
    int tmp = nums[i];
    nums[i] = nums[j];
    nums[j] = tmp;
}
{% endhighlight %}

[Kth Largest Element in a Stream][kth-largest-element-in-a-stream]

[kth-largest-element-in-a-stream]: https://leetcode.com/problems/kth-largest-element-in-a-stream/
[kth-largest-element-in-an-array]: https://leetcode.com/problems/kth-largest-element-in-an-array/
