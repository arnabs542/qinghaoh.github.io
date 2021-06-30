---
layout: post
title:  "Prefix Sum"
---
## Template

{% highlight java %}
int[] p = new int[n + 1];
for (int i = 0: i < n; i++) {
    p[i + 1] = p[i] + nums[i];
}
{% endhighlight %}

## Basic

[Subarray Sum Equals K][subarray-sum-equals-k]

{% highlight java %}
public int subarraySum(int[] nums, int k) {
    Map<Integer, Integer> map = new HashMap<>();  // prefix sum : count
    map.put(0, 1);  // p[0]

    int sum = 0, count = 0;
    for (int num : nums) {
        // calculates p[i]
        sum += num;

        // p[i] - k exists in the map
        if (map.containsKey(sum - k)) {
            count += map.get(sum - k);
        }

        // increments counter of p[i]
        map.put(sum, map.getOrDefault(sum, 0) + 1);
    }

    return count;
}
{% endhighlight %}

[Maximum Size Subarray Sum Equals k][maximum-size-subarray-sum-equals-k]

{% highlight java %}
public int maxSubArrayLen(int[] nums, int k) {
    Map<Integer, Integer> map = new HashMap<>();
    map.put(0, -1);

    int max = 0, sum = 0;;
    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];
        if (map.containsKey(sum - k)) {
            max = Math.max(max, i - map.get(sum - k));
        }

        // stores the index of the first occurrence of sum
        map.putIfAbsent(sum, i);
    }
    return max;
}
{% endhighlight %}

[Contiguous Array][contiguous-array]

{% highlight java %}
public int findMaxLength(int[] nums) {
    Map<Integer, Integer> map = new HashMap<>();
    map.put(0, -1);

    // count(ones) - count(zeros)
    int diff = 0, max = 0;
    for (int i = 0; i < nums.length; i++) {
        diff += nums[i] == 0 ? -1 : 1;
        if (map.containsKey(diff)) {
            max = Math.max(max, i - map.get(diff));
        } else {
            map.put(diff, i);
        }
    }
    return max;
}
{% endhighlight %}

## Variants
### Multi-dimension

[Sum of Beauty of All Substrings][sum-of-beauty-of-all-substrings]

{% highlight java %}
int[][] p = new int[26][n + 1];
for (int i = 0; i < n; i++) {
    for (int k = 0; k < p.length; k++) {
        p[k][i + 1] = p[k][i] + (k == s.charAt(i) - 'a' ? 1 : 0);
    }
}
{% endhighlight %}

### Exclusive Or

[Count Triplets That Can Form Two Arrays of Equal XOR][count-triplets-that-can-form-two-arrays-of-equal-xor]

### Product

[Product of the Last K Numbers][product-of-the-last-k-numbers]

{% highlight java %}
private List<Integer> p;

public ProductOfNumbers() {
    p = new ArrayList<>();
    p.add(1);
}

public void add(int num) {
    if (num > 0) {
        p.add(p.get(p.size() - 1) * num);
    } else {
        p = new ArrayList();
        p.add(1);
    }
}

public int getProduct(int k) {
    int n = p.size();
    return k < n ? p.get(n - 1) / p.get(n - k - 1) : 0;
}
{% endhighlight %}

### Linked List

[Remove Zero Sum Consecutive Nodes from Linked List][remove-zero-sum-consecutive-nodes-from-linked-list]

{% highlight java %}
public ListNode removeZeroSumSublists(ListNode head) {
    ListNode dummy = new ListNode();
    dummy.next = head;

    Map<Integer, ListNode> map = new HashMap<>();
    map.put(0, dummy);

    // stores the last node with the prefix sum
    int sum = 0;
    for (ListNode curr = dummy; curr != null; curr = curr.next) {
        sum += curr.val;
        map.put(sum, curr);
    }

    // links to the last node that has the same prefix sum
    sum = 0;
    for (ListNode curr = dummy; curr != null; curr = curr.next) {
        sum += curr.val;
        curr.next = map.get(sum).next;
    }
    return dummy.next;
}
{% endhighlight %}

[contiguous-array]: https://leetcode.com/problems/contiguous-array/
[count-triplets-that-can-form-two-arrays-of-equal-xor]: https://leetcode.com/problems/count-triplets-that-can-form-two-arrays-of-equal-xor/
[maximum-size-subarray-sum-equals-k]: https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/
[product-of-the-last-k-numbers]: https://leetcode.com/problems/product-of-the-last-k-numbers/
[remove-zero-sum-consecutive-nodes-from-linked-list]: https://leetcode.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/
[subarray-sum-equals-k]: https://leetcode.com/problems/subarray-sum-equals-k/
[sum-of-beauty-of-all-substrings]: https://leetcode.com/problems/sum-of-beauty-of-all-substrings/
