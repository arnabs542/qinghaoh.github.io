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

[Longest Well-Performing Interval][longest-well-performing-interval]

{% highlight java %}
public int longestWPI(int[] hours) {
    int max = 0, score = 0, n = hours.length;
    Map<Integer, Integer> seen = new HashMap<>();
    for (int i = 0; i < n; i++) {
        // finds the longest subarray with positive sum
        score += hours[i] > 8 ? 1 : -1;
        if (score > 0) {
            max = i + 1;
        } else {
            seen.putIfAbsent(score, i);
            if (seen.containsKey(score - 1)) {
                max = Math.max(max, i - seen.get(score - 1));
            }
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

[Minimum Absolute Difference Queries][minimum-absolute-difference-queries]

{% highlight java %}
private static final int MAX_NUM = 100;

public int[] minDifference(int[] nums, int[][] queries) {
    int n = nums.length, m = queries.length;
    int[][] p = new int[n + 1][MAX_NUM + 1];
    int[] count = new int[MAX_NUM + 1];
    for (int i = 0; i < n; i++) {
        count[nums[i]]++;
        p[i + 1] = Arrays.copyOf(count, count.length);
    }

    int[] ans = new int[m];
    for (int i = 0; i < m; i++) {
        ans[i] = Integer.MAX_VALUE;
        for (int j = 0, prev = 0; j < p[0].length; j++) {
            if (p[queries[i][1] + 1][j] != p[queries[i][0]][j]) {
                if (prev != 0) {
                    ans[i] = Math.min(ans[i], j - prev);
                }
                prev = j;
            }
        }
        if (ans[i] == Integer.MAX_VALUE) {
            ans[i] = -1;
        }
    }
    return ans;
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
        answer.add(odd - 1 <= 2 * q[2]);
    }
    return answer;
}
{% endhighlight %}

[Number of Wonderful Substrings][number-of-wonderful-substrings]

{% highlight java %}
private static final int NUM_CHARS = 10;

public long wonderfulSubstrings(String word) {
    // map[i]: count of bit mask i
    long[] map = new long[1 << NUM_CHARS];
    map[0] = 1;

    // bits to represent prefix xor
    long count = 0;
    int p = 0;
    for (int i = 0; i < word.length(); i++) {
        p ^= 1 << (word.charAt(i) - 'a');
        count += map[p];
        for (int j = 0; j < NUM_CHARS; j++) {
            count += map[p ^ (1 << j)];
        }
        map[p]++;
    }
    return count;
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

[Product of Array Except Self][product-of-array-except-self]

{% highlight java %}
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] answer = new int[n];

    // prefix product, no last element
    answer[0] = 1;
    for (int i = 0; i < n - 1; i++) {
        answer[i + 1] = nums[i] * answer[i];
    }

    int product = 1;
    for (int i = n - 1; i >= 0; i--) {
        answer[i] *= product;
        product *= nums[i];
    }
    return answer;
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

## Frequency

[Sum of Floored Pairs][sum-of-floored-pairs]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int sumOfFlooredPairs(int[] nums) {
    int max = Arrays.stream(nums).max().getAsInt();

    int[] freq = new int[max + 1];
    for (int num : nums) {
        freq[num]++;
    }

    for (int i = 1; i < freq.length; i++) {
        freq[i] += freq[i - 1];
    }

    // if floor(nums[i] / nums[j]) = k,
    // then k * nums[j] <= nums[i] < ((k + 1) * nums[j])
    Integer[] dp = new Integer[max + 1];
    int count = 0;
    for (int num : nums) {
        if (dp[num] != null) {
            count = (count + dp[num]) % MOD;
            continue;
        }

        // initial interval: [low, high]
        int curr = 0, k = 1, low = num, high = Math.min(2 * num - 1, max);
        while (low <= max) {
            curr = (int)(curr + ((freq[high] - freq[low - 1]) * (long)k) % MOD) % MOD;
            low += num;
            high = Math.min(high + num, max);
            k++;
        }
        count = (count + (dp[num] = curr)) % MOD;
    }
    return count;
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

## Range Sum

[Number of Ways of Cutting a Pizza][number-of-ways-of-cutting-a-pizza]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;
private Integer[][][] memo;

public int ways(String[] pizza, int k) {
    int m = pizza.length, n = pizza[0].length();
    this.memo = new Integer[k][m][n];

    // p[i][j] is the total number of apples in pizza[i:][j:]
    int[][] p = new int[m + 1][n + 1];
    for (int i = m - 1; i >= 0; i--) {
        for (int j = n - 1; j >= 0; j--) {
            p[i][j] = p[i][j + 1] + p[i + 1][j] - p[i + 1][j + 1] + (pizza[i].charAt(j) == 'A' ? 1 : 0);
        }
    }
    return dfs(m, n, k - 1, 0, 0, p);
}

private int dfs(int m, int n, int cuts, int i, int j, int[][] p) {
    // if the remain piece has no apple
    if (p[i][j] == 0) {
        return 0;
    }

    // found valid way after using k - 1 cuts
    if (cuts == 0) {
        return 1;
    }

    if (memo[cuts][i][j] != null) {
        return memo[cuts][i][j];
    }

    int count = 0;
    // cuts in horizontal
    for (int r = i + 1; r < m; r++)  {
        // cuts if the upper piece contains at least one apple
        if (p[i][j] - p[r][j] > 0) {
            count = (count + dfs(m, n, cuts - 1, r, j, p)) % MOD;
        }
    }

    // cuts in vertical
    for (int c = j + 1; c < n; c++) {
        // cuts if the left piece contains at least one apple
        if (p[i][j] - p[i][c] > 0) {
            count = (count + dfs(m, n, cuts - 1, i, c, p)) % MOD;
        }
    }
    return memo[cuts][i][j] = count;
}
{% endhighlight %}

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

[Count Subarrays With More Ones Than Zeros][count-subarrays-with-more-ones-than-zeros]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int subarraysWithMoreZerosThanOnes(int[] nums) {
    int n = nums.length;
    // dp[i]: count of subarrays ending at i
    int[] dp = new int[n];

    // {sum : last index}
    Map<Integer, Integer> map = new HashMap<>();
    map.put(0, -1);

    // like sin(x)
    int sum = 0, count = 0;
    for (int i = 0; i < n; i++) {
        // prefix sum of #1 - #0
        sum += nums[i] == 0 ? -1 : 1;
        if (map.containsKey(sum)) {
            // between curr and prev, #0 == #1
            int prev = map.get(sum);
            dp[i] = prev < 0 ? 0 : dp[prev];

            // valley between curr and prev
            if (nums[i] == 1) {
                dp[i] += i - prev - 1;
            }
        } else if (sum > 0) {
            // the subarray ending at i has more ones than zeros
            dp[i] = i + 1;
        }
        count = (count + dp[i]) % MOD;
        map.put(sum, i);
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

# Prefix + Suffix Sum

[Maximum Number of Ways to Partition an Array][maximum-number-of-ways-to-partition-an-array]

{% highlight java %}
public int waysToPartition(int[] nums, int k) {
    int n = nums.length;
    long sum = Arrays.stream(nums).	asLongStream().sum();

    // sum of left part minus sum of right part
    // diff[i] = (nums[0] + ... + nums[i - 1]) - (nums[i] + ... + nums[n - 1])
    // where 1 <= i < n

    // prefix and suffix
    // frequency map of diff[1..i] and diff[(i + 1)..(n - 1)] respectively
    Map<Long, Integer> p = new HashMap<>(), s = new HashMap<>();
    // running sum
    long left = 0, right = 0;
    for (int i = 0; i < n - 1; i++) {
        left += nums[i];
        right = sum - left;
        s.compute(left - right, (key, v) -> v == null ? 1 : v + 1);
    }

    // no replacement
    int ways = s.getOrDefault(0l, 0);

    // if we replace nums[i] with k,
    // then diff[1...i] decrease by d, and diff[(i + 1)...(n - 1)] increase by d
    // where d = k - nums[i]
    // the ways is the number of 0s in this new diff array.
    left = 0;
    for (int i = 0; i < n; i++) {
        left += nums[i];
        right = sum - left;
        long d = k - nums[i];

        // replaces nums[i] with k
        // we don't actually modify the diff arrays

        // diff[1...i] decrease by d, which means we need to find p[d] before the replacement
        // so that after the replacement, these diff elements with value d will become 0
        // similarly, we need to find s[-d]
        ways = Math.max(ways, p.getOrDefault(d, 0) + s.getOrDefault(-d, 0));

        // transfers the frequency from suffix map to prefix map
        s.compute(left - right, (key, v) -> v == null ? 1 : v - 1);
        p.compute(left - right, (key, v) -> v == null ? 1 : v + 1);
    }
    return ways;
}
{% endhighlight %}

[Super Washing Machines][super-washing-machines]

{% highlight java %}
public int findMinMoves(int[] machines) {
    int n = machines.length;
    int sum = Arrays.stream(machines).sum();
    if (sum % n != 0) {
        return -1;
    }

    // prefix and suffix sum
    int[] p = new int[n], s = new int[n];
    for (int i = 1; i < n; i++) {
        p[i] = p[i - 1] + machines[i - 1];
    }
    for (int i = n - 2; i >= 0; i--) {
        s[i] = s[i + 1] + machines[i + 1];
    }

    // minimum moves is the maximum dresses that pass through for each single machine
    int move = 0, avg = sum / n, expLeft = 0, expRight = sum - avg;
    for (int i = 0; i < n; i++) {
        move = Math.max(move, Math.max(expLeft - p[i], 0) + Math.max(expRight - s[i], 0));
        expLeft += avg;
        expRight -= avg;
    }
    return move;
}
{% endhighlight %}

Optimization:

{% highlight java %}
public int findMinMoves(int[] machines) {
    int n = machines.length;
    int sum = Arrays.stream(machines).sum();
    if (sum % n != 0) {
        return -1;
    }

    // minimum moves is the maximum dresses that pass through for each single machine
    int target = sum / n, move = 0, toRight = 0;
    for (int m : machines) {
        // for each machines, toRight = right - left
        // if toRight > 0, left -> right
        // if toRight < 0, right -> left
        toRight += m - target;
        move = Math.max(move, Math.max(Math.abs(toRight), m - target));
    }
    return move;
}
{% endhighlight %}

[can-make-palindrome-from-substring]: https://leetcode.com/problems/can-make-palindrome-from-substring/
[change-minimum-characters-to-satisfy-one-of-three-conditions]: https://leetcode.com/problems/change-minimum-characters-to-satisfy-one-of-three-conditions/
[contiguous-array]: https://leetcode.com/problems/contiguous-array/
[count-subarrays-with-more-ones-than-zeros]: https://leetcode.com/problems/count-subarrays-with-more-ones-than-zeros/
[count-triplets-that-can-form-two-arrays-of-equal-xor]: https://leetcode.com/problems/count-triplets-that-can-form-two-arrays-of-equal-xor/
[longest-well-performing-interval]: https://leetcode.com/problems/longest-well-performing-interval/
[make-sum-divisible-by-p]: https://leetcode.com/problems/make-sum-divisible-by-p/
[max-sum-of-rectangle-no-larger-than-k]: https://leetcode.com/problems/max-sum-of-rectangle-no-larger-than-k/
[maximize-the-beauty-of-the-garden]: https://leetcode.com/problems/maximize-the-beauty-of-the-garden/
[maximum-absolute-sum-of-any-subarray]: https://leetcode.com/problems/maximum-absolute-sum-of-any-subarray/
[maximum-number-of-non-overlapping-subarrays-with-sum-equals-target]: https://leetcode.com/problems/maximum-number-of-non-overlapping-subarrays-with-sum-equals-target/
[maximum-number-of-ways-to-partition-an-array]: https://leetcode.com/problems/maximum-number-of-ways-to-partition-an-array/
[maximum-size-subarray-sum-equals-k]: https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/
[minimum-absolute-difference-queries]: https://leetcode.com/problems/minimum-absolute-difference-queries/
[number-of-ways-of-cutting-a-pizza]: https://leetcode.com/problems/number-of-ways-of-cutting-a-pizza/
[number-of-ways-to-separate-numbers]: https://leetcode.com/problems/number-of-ways-to-separate-numbers/
[number-of-wonderful-substrings]: https://leetcode.com/problems/number-of-wonderful-substrings/
[product-of-array-except-self]: https://leetcode.com/problems/product-of-array-except-self/
[product-of-the-last-k-numbers]: https://leetcode.com/problems/product-of-the-last-k-numbers/
[remove-zero-sum-consecutive-nodes-from-linked-list]: https://leetcode.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/
[subarray-sum-equals-k]: https://leetcode.com/problems/subarray-sum-equals-k/
[sum-of-floored-pairs]: https://leetcode.com/problems/sum-of-floored-pairs/
[sum-of-special-evenly-spaced-elements-in-array]: https://leetcode.com/problems/sum-of-special-evenly-spaced-elements-in-array/
[sum-of-beauty-of-all-substrings]: https://leetcode.com/problems/sum-of-beauty-of-all-substrings/
[super-washing-machines]: https://leetcode.com/problems/super-washing-machines/
