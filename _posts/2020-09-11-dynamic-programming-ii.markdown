---
layout: post
title:  "Dynamic Programming II"
tag: dynamic programming
---
[Largest Sum of Averages][largest-sum-of-averages]

### Top-down DP: divide and conquer + memorization

{% highlight java %}
private double[][] memo;
private double[] sum;

public double largestSumOfAverages(int[] A, int K) {
    memo = new double[A.length + 1][K + 1];
    sum = new double[A.length + 1];

    for (int i = 0; i < A.length; i++) {
        sum[i + 1] = sum[i] + A[i];
    }
    return largestSumOfAverages(A, A.length, K);
}

private double largestSumOfAverages(int[] A, int end, int K) {
    if (memo[end][K] != 0) {
        return memo[end][K];
    }

    if (K == 1) {
        memo[end][1] = sum[end] / end;
        return memo[end][1];
    }

    double max = 0;
    for (int i = end - 1; i >= K - 1; i--) {
        max = Math.max(max, (sum[end] - sum[i]) / (end - i) + largestSumOfAverages(A, i, K - 1));
    }
    memo[end][K] = max;
    return max;
}
{% endhighlight %}

The result of each loop can be written to `memo` directly:

{% highlight java %}
    for (int i = end - 1; i >= K - 1; i--) {
        memo[end][K] = Math.max(memo[end][K], (sum[end] - sum[i]) / (end - i) + largestSumOfAverages(A, i, K - 1));
    }
    return memo[end][K];
{% endhighlight %}

### Bottom-up DP

{% highlight java %}
public double largestSumOfAverages(int[] A, int K) {
    double[][] dp = new double[A.length + 1][K + 1];

    double[] sum = new double[A.length + 1];
    for (int i = 0; i < A.length; i++) {
        sum[i + 1] = sum[i] + A[i];
    }

    for (int i = 1; i < dp.length; i++) {
        dp[i][1] = sum[i] / i;
    }

    for (int k = 2; k <= K; k++) {
        for (int i = 1; i <= A.length; i++) {
            for (int j = 0; j < i; j++) {
                dp[i][k] = Math.max(dp[i][k], (sum[i] - sum[j]) / (i - j) + dp[j][k - 1]);
            }
        }
    }

    return dp[A.length][K];
}
{% endhighlight %}

```
[0.0,0.0,0.0,0.0]
[0.0,9.0,9.0,9.0]
[0.0,5.0,10.0,10.0]
[0.0,4.0,10.5,12.0]
[0.0,3.75,11.0,13.5]
[0.0,4.8,12.75,20.0]
```

1D:

{% highlight java %}
public double largestSumOfAverages(int[] A, int K) {
    double[] dp = new double[A.length + 1];

    double[] sum = new double[A.length + 1];
    for (int i = 0; i < A.length; i++) {
        sum[i + 1] = sum[i] + A[i];
    }

    for (int i = 1; i < dp.length; i++) {
        dp[i] = sum[i] / i;
    }

    for (int k = 2; k <= K; k++) {
        for (int i = A.length; i > 0; i--) {
            for (int j = 0; j < i; j++) {
                dp[i] = Math.max(dp[i], (sum[i] - sum[j]) / (i - j) + dp[j]);
            }
        }
    }

    return dp[A.length];
}
{% endhighlight %}

[largest-sum-of-averages]: https://leetcode.com/problems/largest-sum-of-averages/
