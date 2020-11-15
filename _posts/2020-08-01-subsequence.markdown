---
layout: post
title:  "Subsequence"
tags: array
---
# Definition
```
a[i_0], a[i_1], ..., a[i_k]
```
Where `0 <= i_0 < i_1 < ... < i_k <= a.length`

# Algorithm

## Sort

[Smallest Range II][smallest-range-ii]

{% highlight java %}
public int smallestRangeII(int[] A, int K) {
    Arrays.sort(A);

    int max = A[A.length - 1], min = A[0], diff = max - min;
    for (int i = 0; i < A.length - 1; i++) {
        max = Math.max(A[A.length - 1], A[i] + 2 * K);
        min = Math.min(A[0] + 2 * K, A[i + 1]);
        diff = Math.min(diff, max - min);
    }
    return diff;
}
{% endhighlight %}

## Binary Search

[Is Subsequence][is-subsequence]

It's easy to come up with a `O(n)` solution with two poiners.

Follow up:

If there are lots of incoming `s` (e.g. more than one billion), how to check one by one to see if `t` has its subsequence?

Binary Search:

{% highlight java %}
public boolean isSubsequence(String s, String t) {
    Map<Integer, List<Integer>> map = new HashMap<>();
    for (int i = 0; i < t.length(); i++) {
        map.computeIfAbsent(t.charAt(i) - 'a', k -> new ArrayList<>()).add(i);
    }

    int index = 0;
    for (char c : s.toCharArray()) {
        if (!map.containsKey(c - 'a')) {
            return false;
        }

        int i = Collections.binarySearch(map.get(c - 'a'), index);
        if (i < 0) {
            i = ~i;
        }
        if (i == map.get(c - 'a').size()) {
            return false;
        }
        index = map.get(c - 'a').get(i) + 1;
    }
    return true;
}
{% endhighlight %}

## Dynamic Programming

[Longest Increasing][longest-increasing-subsequence]

{% highlight java %}
public int lengthOfLIS(int[] nums) {
    // dp[i]: LIS ends at index i
    int[] dp = new int[nums.length];

    int max = 0;
    for (int i = 0; i < nums.length; i++) {
        dp[i] = 1;
        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
        max = Math.max(max, dp[i]);
    }

    return max;
}
{% endhighlight %}

Similar problem: [Largest Divisible Subset][largest-divisible-subset]

A quicker solution is [Patience sorting](https://en.wikipedia.org/wiki/Patience_sorting). [This](https://www.cs.princeton.edu/courses/archive/spring13/cos423/lectures/LongestIncreasingSubsequence.pdf) is a Princeton lecture for it.

{% highlight java %}
public int lengthOfLIS(int[] nums) {
    List<Integer> piles = new ArrayList<>(nums.length);
    for (int num : nums) {
        int pile = Collections.binarySearch(piles, num);
        if (pile < 0) {
            pile = ~pile;
        }

        if (pile == piles.size()) {
            piles.add(num);
        } else {
            piles.set(pile, num);
        }
    }
    return piles.size();
}
{% endhighlight %}

Not as intuitive, we can use array instead:

{% highlight java %}
public int lengthOfLIS(int[] nums) {
    int[] piles = new int[nums.length];
    int count = 0;
    for (int num : nums) {
        int i = Arrays.binarySearch(piles, 0, count, num);
        if (i < 0) {
            i = ~i;
        }

        piles[i] = num;
        if (i == count) {
            count++;
        }
    }

    return count;
}
{% endhighlight %}

[Shortest Common Supersequence][shortest-common-supersequence]

{% highlight java %}
public String shortestCommonSupersequence(String str1, String str2) {
    // longest common subsequence
    int[][] dp = new int[str1.length() + 1][str2.length() + 1];

    for (int i = 1; i < dp.length; i++) {
        for (int j = 1; j < dp[i].length; j++) {
            if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j]);
            }
        }
    }

    StringBuilder sb = new StringBuilder();
    int len = dp[str1.length()][str2.length()];
    int i = str1.length(), j = str2.length(), pi = str1.length(), pj = str2.length();
    while (i > 0 || j > 0) {
        while (j > 0 && dp[i][j - 1] == dp[i][j]) {
            j--;
        }
        if (j < pj) {
            sb.insert(0, str2.substring(j, pj));
        }

        while (i > 0 && dp[i - 1][j] == dp[i][j]) {
            i--;
        }
        if (i < pi) {
            sb.insert(0, str1.substring(i, pi));
        }

        pi = --i;
        pj = --j;
        if (pj >= 0) {
            sb.insert(0, str2.charAt(pj));
        }
    }

    return sb.toString();
}
{% endhighlight %}

[Longest Arithmetic Subsequence][longest-arithmetic-subsequence]

{% highlight java %}
public int longestArithSeqLength(int[] A) {
    // diff : max length
    List<Map<Integer, Integer>> dp = new ArrayList<>();
    int max = 2;
    for (int j = 0; j < A.length; j++) {
        dp.add(new HashMap<>());
        for (int i = 0; i < j; i++) {
            int d = A[j] - A[i];
            dp.get(j).put(d, dp.get(i).getOrDefault(d, 1) + 1);
            max = Math.max(max, dp.get(j).get(d));
        }
    }
    return max;
}
{% endhighlight %}

[largest-divisible-subset]: https://leetcode.com/problems/largest-divisible-subset/
[longest-arithmetic-subsequence]: https://leetcode.com/problems/longest-arithmetic-subsequence/
[longest-increasing-subsequence]: https://leetcode.com/problems/longest-increasing-subsequence/
[is-subsequence]: https://leetcode.com/problems/is-subsequence/
[shortest-common-subsequence]: https://leetcode.com/problems/shortest-common-subsequence/
[smallest-range-ii]: https://leetcode.com/problems/smallest-range-ii/
