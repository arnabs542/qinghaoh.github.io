---
layout: post
title:  "Prefix Sum"
---
# Template

{% highlight java %}
int[] p = new int[n + 1];
for (int i = 0: i < n; i++) {
    p[i + 1] = p[i] + nums[i];
}
{% endhighlight %}

# Basic

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

# Variants
## Multi-dimension

[Sum of Beauty of All Substrings][sum-of-beauty-of-all-substrings]

{% highlight java %}
int[][] p = new int[26][n + 1];
for (int i = 0; i < n; i++) {
    for (int k = 0; k < p.length; k++) {
        p[k][i + 1] = p[k][i] + (k == s.charAt(i) - 'a' ? 1 : 0);
    }
}
{% endhighlight %}

## Exclusive Or

[Count Triplets That Can Form Two Arrays of Equal XOR][count-triplets-that-can-form-two-arrays-of-equal-xor]

[Can Make Palindrome from Substring][can-make-palindrome-from-substring]

{% highlight java %}
public List<Boolean> canMakePaliQueries(String s, int[][] queries) {
    int n = s.length();
    // 26 bits to represent prefix xor
    int[] p = new int[n + 1];
    for (int i = 0; i < n; i++) {
        p[i + 1] = p[i] ^ (1 << (s.charAt(i) - 'a'));
    }

    List<Boolean> answer = new ArrayList<>();
    for (int[] q : queries) {
        int odd = Integer.bitCount(p[q[1] + 1] ^ p[q[0]]);
        answer.add((odd - 1) <= 2 * q[2]);
    }
    return answer;
}
{% endhighlight %}

## Product

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

## Mod

[Make Sum Divisible by P][make-sum-divisible-by-p]

{% highlight java %}
public int minSubarray(int[] nums, int p) {
    int n = nums.length;

    int r = 0;
    for (int num : nums) {
        r = (r + num) % p;
    }
    if (r == 0) {
        return 0;
    }

    // index
    Map<Integer, Integer> map = new HashMap<>();
    map.put(0, -1);

    int min = n, m = 0;
    for (int i = 0; i < n; i++) {
        m = (m + nums[i] % p) % p;
        int d = (m - r + p) % p;
        if (map.containsKey(d)) {
            min = Math.min(min, i - map.get(d));
        }
        map.put(m, i);
    }
    return min == n ? -1 : min;
}
{% endhighlight %}

## Linked List

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

# Rolling Prefix Sum

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

# Pre-computation

[Sum Of Special Evenly-Spaced Elements In Array][sum-of-special-evenly-spaced-elements-in-array]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int[] solve(int[] nums, int[][] queries) {
    int n = nums.length;

    // pre-computation (y ^ 2 <= n)
    int sqrtn = (int)Math.sqrt(n) + 1;
    // map[i][j]: when y == i, the answer from j to the end
    int[][] map = new int[sqrtn][n];
    for (int i = 1; i * i <= n; i++) {
        for (int j = n - 1; j >= 0; j--) {
            if (i + j >= n) {
                map[i][j] = nums[j];
            } else {
                // sums backwards
                map[i][j] = (map[i][i + j] + nums[j]) % MOD;
            }
        }
    }

    int m = queries.length;
    int[] answer = new int[m];
    for(int i = 0; i < m; i++) {
        int x = queries[i][0], y = queries[i][1];
        if ((long)y * (long)y <= n) {
            answer[i] = map[y][x];
        } else {
            // y is large enough, no pre-computation
            for (int j = x; j < n; j += y) {
                answer[i] = (answer[i] + nums[j]) % MOD;
            }
        }
    }
    return answer;
}
{% endhighlight %}

# Dynamic Programming

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

[Number of Ways to Separate Numbers][number-of-ways-to-separate-numbers]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int numberOfCombinations(String num) {
    int n = num.length();
    if (num.charAt(0) == '0') {
        return 0;
    }

    // lcp[i][j]: length of the long common prefix of s.substring(i) and s.substring(j)
    int[][] lcp = new int[n + 1][n + 1];
    for (int i = n - 1; i >= 0 ; i--) {
        for (int j = n - 1; j >= 0; j--) {
            if (num.charAt(i) == num.charAt(j)) {
                lcp[i][j] = lcp[i + 1][j + 1] + 1;
            }
        }
    }

    // dp[i][j]: number of ways when the last number is num[i, j]
    // len = j - i + 1, length of the last number
    // - Case 1:
    //   second last number has less digits than last number
    //   dp[i][j] += dp[i - len + 1][i - 1] + ... + dp[i - 1][i - 1]
    // - Case 2:
    //   second last number has the same number of digits as last number
    //   if num[i - length : i - 1] <= num[i : j],
    //   dp[i][j] += dp[i - length][i - 1]

    // prefix sum:
    // p[i + 1][j] = dp[0][j] + dp[1][j] ... + dp[i][j]
    long[][] p = new long[n + 1][n];

    // p[1][j] = dp[0][j] = 1
    Arrays.fill(p[1], 1);

    // last number is num[i:j]
    for (int i = 1; i < n; i++) {
        // no leading zero
        if (num.charAt(i) == '0') {
            p[i + 1] = p[i];
            continue;
        }

        for (int j = i; j < n; j++) {
            int len = j - i + 1, prevStart = i - len;

            // number of ways introduced by the current digit
            long count = 0;

            // start of second last number must be >= 0
            if (prevStart < 0) {
                // Case 1 only:
                // dp[0][i - 1] + dp[1][i - 1] + ... + dp[i - 1][i - 1]
                // == p[i][i - 1]
                count += p[i][i - 1];
            } else {
                // Case 1:
                // dp[prevStart][i - 1] + ... + dp[i - 1][i - 1]
                count += (p[i][i - 1] - p[prevStart + 1][i - 1] + MOD) % MOD;

                // Case 2:
                if (lcp[prevStart][i] >= len ||  // second last number == last number
                    // second last number < last number
                    num.charAt(prevStart + lcp[prevStart][i]) - num.charAt(i + lcp[prevStart][i]) < 0) {
                    // dp[i - length][i - 1]
                    long tmp = (p[prevStart + 1][i - 1] - p[prevStart][i - 1] + MOD) % MOD;
                    count = (count + tmp + MOD) % MOD;
                }
            }
            p[i + 1][j] = (p[i][j] + count) % MOD;
        }
    }
    return (int)p[n][n - 1];
}
{% endhighlight %}

# Bounded Sum

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

[can-make-palindrome-from-substring]: https://leetcode.com/problems/can-make-palindrome-from-substring/
[change-minimum-characters-to-satisfy-one-of-three-conditions]: https://leetcode.com/problems/change-minimum-characters-to-satisfy-one-of-three-conditions/
[contiguous-array]: https://leetcode.com/problems/contiguous-array/
[count-triplets-that-can-form-two-arrays-of-equal-xor]: https://leetcode.com/problems/count-triplets-that-can-form-two-arrays-of-equal-xor/
[make-sum-divisible-by-p]: https://leetcode.com/problems/make-sum-divisible-by-p/
[max-sum-of-rectangle-no-larger-than-k]: https://leetcode.com/problems/max-sum-of-rectangle-no-larger-than-k/
[maximize-the-beauty-of-the-garden]: https://leetcode.com/problems/maximize-the-beauty-of-the-garden/
[maximum-absolute-sum-of-any-subarray]: https://leetcode.com/problems/maximum-absolute-sum-of-any-subarray/
[maximum-number-of-non-overlapping-subarrays-with-sum-equals-target]: https://leetcode.com/problems/maximum-number-of-non-overlapping-subarrays-with-sum-equals-target/
[maximum-size-subarray-sum-equals-k]: https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/
[number-of-ways-to-separate-numbers]: https://leetcode.com/problems/number-of-ways-to-separate-numbers/
[product-of-the-last-k-numbers]: https://leetcode.com/problems/product-of-the-last-k-numbers/
[remove-zero-sum-consecutive-nodes-from-linked-list]: https://leetcode.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/
[subarray-sum-equals-k]: https://leetcode.com/problems/subarray-sum-equals-k/
[sum-of-special-evenly-spaced-elements-in-array]: https://leetcode.com/problems/sum-of-special-evenly-spaced-elements-in-array/
[sum-of-beauty-of-all-substrings]: https://leetcode.com/problems/sum-of-beauty-of-all-substrings/
