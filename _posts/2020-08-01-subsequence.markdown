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

[Longest Arithmetic Subsequence][longest-arithmetic-subsequence]

{% highlight java %}
{% endhighlight %}

[longest-arithmetic-subsequence]: https://leetcode.com/problems/longest-arithmetic-subsequence/
[longest-increasing-subsequence]: https://leetcode.com/problems/longest-increasing-subsequence/
[is-subsequence]: https://leetcode.com/problems/is-subsequence/
