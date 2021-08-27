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

[Maximum Absolute Sum of Any Subarray][maximum-absolute-sum-of-any-subarray]

```
abs(subarray) = p[i] - p[j] <= max(p) - min(p)
```

## Rolling Prefix Sum

[Maximize the Beauty of the Garden][maximize-the-beauty-of-the-garden]

{% highlight java %}
public int maximumBeauty(int[] flowers) {
    // flower : first prefix sum of this flower
    Map<Integer, Integer> map = new HashMap<>();
    int sum = 0, max = Integer.MIN_VALUE;
    for (int f : flowers) {
        if (map.containsKey(f)) {
            max = Math.max(max, sum - map.get(f) + 2 * f);
        }

        // counts positive beauty only
        if (f > 0) {
            sum += f;
        }

        if (!map.containsKey(f)) {
            map.put(f, sum);
        }
    }
    return max;
}
{% endhighlight %}

[Change Minimum Characters to Satisfy One of Three Conditions][change-minimum-characters-to-satisfy-one-of-three-conditions]

{% highlight java %}
public int minCharacters(String a, String b) {
    int m = a.length(), n = b.length(), min = m + n;
    int[] c1 = new int[26], c2 = new int[26];
    for (char c : a.toCharArray()) {
        c1[c - 'a']++;
    }
    for (char c : b.toCharArray()) {
        c2[c - 'a']++;
    }

    for (int i = 0; i < 26; ++i) {
        // Condition 3
        min = Math.min(min, m + n - c1[i] - c2[i]);

        // rolling prefix sum
        if (i > 0) {
            c1[i] += c1[i - 1];
            c2[i] += c2[i - 1];
        }

        // exludes 'z'
        if (i < 25) {
            // Condition 1
            min = Math.min(min, m - c1[i] + c2[i]);
            // Condition 2
            min = Math.min(min, n - c2[i] + c1[i]);
        }
    }
    return min;
}
{% endhighlight %}

## Dynamic Programming

[Maximum Number of Non-Overlapping Subarrays With Sum Equals Target][maximum-number-of-non-overlapping-subarrays-with-sum-equals-target]

{% highlight java %}
public int maxNonOverlapping(int[] nums, int target) {
    // prefix sum : max number of non-empty non-overlapping subarrays
    Map<Integer, Integer> map = new HashMap<>();
    map.put(0, 0);

    int sum = 0, count = 0;
    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];
        if (map.containsKey(sum - target)) {
            count = Math.max(count, map.get(sum - target) + 1);
        }

        // later sum can always overwrite, because `count` is guaranteed >=
        map.put(sum, count);
    }
    return count;
}
{% endhighlight %}

## Bounded Sum

[Max Sum of Rectangle No Larger Than K][max-sum-of-rectangle-no-larger-than-k]

{% highlight java %}
public int maxSumSubmatrix(int[][] matrix, int k) {
    // 2D Kadane's algorithm
    int m = matrix.length, n = matrix[0].length;
    int max = Integer.MIN_VALUE;

    // finds the rectangle with maxSum <= k in O(logN) time
    TreeSet<Integer> set = new TreeSet<>();

    // assumes the number of rows is larger than columns
    for (int r1 = 0; r1 < n; r1++) {
        // accumulate sums for rows in [r1, r2]
        int[] nums = new int[m];
        for (int r2 = r1; r2 < n; r2++) {
            for (int i = 0; i < m; i++) {
                nums[i] += matrix[i][r2];
            }

            // stores prefix sums
            set.clear();
            set.add(0);  // p[0]

            int prefixSum = 0;
            for (int num : nums) {
                prefixSum += num;
                // finds the max targetSum which satifies
                // sum == prefixSum - targetSum <= k
                // i.e. targetSum >= prefixSum - k
                Integer targetSum = set.ceiling(prefixSum - k);
                if (targetSum != null) {
                    max = Math.max(max, prefixSum - targetSum);
                }
                set.add(prefixSum);
            }
        }
    }

    return max;
}
{% endhighlight %}

[change-minimum-characters-to-satisfy-one-of-three-conditions]: https://leetcode.com/problems/change-minimum-characters-to-satisfy-one-of-three-conditions/
[contiguous-array]: https://leetcode.com/problems/contiguous-array/
[count-triplets-that-can-form-two-arrays-of-equal-xor]: https://leetcode.com/problems/count-triplets-that-can-form-two-arrays-of-equal-xor/
[max-sum-of-rectangle-no-larger-than-k]: https://leetcode.com/problems/max-sum-of-rectangle-no-larger-than-k/
[maximize-the-beauty-of-the-garden]: https://leetcode.com/problems/maximize-the-beauty-of-the-garden/
[maximum-absolute-sum-of-any-subarray]: https://leetcode.com/problems/maximum-absolute-sum-of-any-subarray/
[maximum-number-of-non-overlapping-subarrays-with-sum-equals-target]: https://leetcode.com/problems/maximum-number-of-non-overlapping-subarrays-with-sum-equals-target/
[maximum-size-subarray-sum-equals-k]: https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/
[product-of-the-last-k-numbers]: https://leetcode.com/problems/product-of-the-last-k-numbers/
[remove-zero-sum-consecutive-nodes-from-linked-list]: https://leetcode.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/
[subarray-sum-equals-k]: https://leetcode.com/problems/subarray-sum-equals-k/
[sum-of-beauty-of-all-substrings]: https://leetcode.com/problems/sum-of-beauty-of-all-substrings/
