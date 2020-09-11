---
layout: post
title:  "Dynamic Programming II"
tag: dynamic programming
---
[Partition Array for Maximum Sum][partition-array-for-maximum-sum]

Top-down DP: divide and conquer + memorization

{% highlight java %}
private int[] memo;

public int maxSumAfterPartitioning(int[] arr, int k) {
    memo = new int[arr.length];
    Arrays.fill(memo, -1);

    return maxSumAfterPartitioning(arr, 0, k);
}

private int maxSumAfterPartitioning(int[] arr, int start, int k) {
    if (start == arr.length) {
        return 0;
    }

    if (memo[start] >= 0) {
        return memo[start];
    }

    int max = 0, sum = 0;
    for (int i = 0; i < k && start + i < arr.length; i++) {
        max = Math.max(arr[start + i], max);
        sum = Math.max(sum, max * (i + 1) + maxSumAfterPartitioning(arr, start + i + 1, k));
    }
    memo[start] = sum;
    return sum;
}
{% endhighlight %}

Bottom-up DP

{% highlight java %}
public int maxSumAfterPartitioning(int[] arr, int k) {
    int[] dp = new int[arr.length + 1];

    for (int i = arr.length - 1; i >= 0; i--) {
        int max = 0;
        for (int j = 0; j < k && i + j < arr.length; j++) {
            max = Math.max(arr[i + j], max);
            dp[i] = Math.max(dp[i], max * (j + 1) + dp[i + j + 1]);
        }
    }

    return dp[0];
}
{% endhighlight %}

Reverse `dp[]`

{% highlight java %}
public int maxSumAfterPartitioning(int[] arr, int k) {
    int[] dp = new int[arr.length + 1];

    for (int i = 0; i < arr.length; i++) {
        int max = 0;
        for (int j = 0; j < k && i - j >= 0; j++) {
            max = Math.max(arr[i - j], max);
            dp[i + 1] = Math.max(dp[i + 1], max * (j + 1) + dp[i - j]);
        }
    }

    return dp[arr.length];
}
{% endhighlight %}

[partition-array-for-maximum-sum]: https://leetcode.com/problems/partition-array-for-maximum-sum/
