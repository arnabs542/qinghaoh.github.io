---
layout: post
title:  "Selection"
tags: array
---
[Selection algorithm](https://en.wikipedia.org/wiki/Selection_algorithm)

## Heap Sort

[Kth Largest Element in an Array][kth-largest-element-in-an-array]

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

## Bucket Sort

[Top K Frequent Elements][top-k-frequent-elements]

{% highlight java %}
public int[] topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> count = new HashMap<>();
    for (int num : nums) {
        count.put(num, count.getOrDefault(num, 0) + 1);
    }

    List<Integer>[] bucket = new List[nums.length + 1];
    for (var e : count.entrySet()) {
        if (bucket[e.getValue()] == null) {
            bucket[e.getValue()] = new ArrayList<>();
        }
        bucket[e.getValue()].add(e.getKey());
    }

    int[] result = new int[k];
    int index = 0;
    for (int i = bucket.length - 1; i >= 0; i--) {
        if (bucket[i] != null) {
            for (int num : bucket[i]) {
                result[index++] = num;
            }
            if (index == k) {
                break;
            }
        }
    }
    return result;
}
{% endhighlight %}

## Quickselect

[Quickselect](https://en.wikipedia.org/wiki/Quickselect)
[Partial sorting](https://en.wikipedia.org/wiki/Partial_sorting)

Time complexity: 
* Average: `O(n)`
* Worst: `O(n^2)`

[Kth Largest Element in an Array][kth-largest-element-in-an-array]

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

    return count > k ? quickSelect(nums, p + 1, high, k) : quickSelect(nums, low, p - 1, k - count);
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

## Median of Medians

[Median of medians](https://en.wikipedia.org/wiki/Median_of_medians)

Time complexity:
* Best: `O(n)`
* Worst: `O(n)`

[kth-largest-element-in-a-stream]: https://leetcode.com/problems/kth-largest-element-in-a-stream/
[kth-largest-element-in-an-array]: https://leetcode.com/problems/kth-largest-element-in-an-array/
[top-k-frequent-elements]: https://leetcode.com/problems/top-k-frequent-elements/
