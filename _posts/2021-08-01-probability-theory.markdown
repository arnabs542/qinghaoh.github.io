---
layout: post
title:  "Probability Theory"
tags: math
usemathjax: true
---
# Dynamic Programming 

[New 21 Game][new-21-game]

{% highlight java %}
private static final double MAX_ERROR = 1e-5;

public double new21Game(int n, int k, int maxPts) {
    // k <= score <= n
    if (k == 0 || k + maxPts <= n) {
        return 1d;
    }

    // dp[i]: the probability to get score i
    double[] dp = new double[n + 1];
    dp[0] = 1;

    // sum is the sum of dp[j] where j + maxPts >= i
    double sum = 1, p = 0;
    for (int i = 1; i <= n; i++) {
        // uniform distribution
        dp[i] = sum / maxPts;

        if (i < k) {
            sum += dp[i];
        } else {
            p += dp[i];
        }

        // sliding window size is maxPts
        if (i - maxPts >= 0) {
            sum -= dp[i - maxPts];
        }
    }
    return p;
}
{% endhighlight %}

[new-21-game]: https://leetcode.com/problems/new-21-game/
