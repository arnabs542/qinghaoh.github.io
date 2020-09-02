---
layout: post
title:  "Dynamic Programming"
tag: dynamic programming
---
[Edit Distance][edit-distance]

{% highlight java %}
public int minDistance(String word1, String word2) {
    // dp[i][j]: word1.substring(0, i) -> word2.substring(0, j)
    int[][] dp = new int[word1.length() + 1][word2.length() + 1];

    // word1.substring(0, i) -> empty string
    for (int i = 1; i <= word1.length(); i++) {
        dp[i][0] = i;
    }

    // empty string -> word2.substring(0, j)
    for (int j = 1; j <= word2.length(); j++) {
        dp[0][j] = j;
    }

    for (int i = 1; i <= word1.length(); i++) {
        for (int j = 1; j <= word2.length(); j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                // replace: dp[i - 1][j - 1] + 1
                // delete: dp[i - 1][j] + 1
                // insert: dp[i][j - 1] + 1
                dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1])) + 1;
            }
        }
    }
    return dp[word1.length()][word2.length()];
}
{% endhighlight %}

For example, `word1 = "newton", word2 = "einstein"`, then `dp` is:
```
[0,1,2,3,4,5,6,7,8]
[1,1,2,2,3,4,5,6,7]
[2,1,2,3,3,4,4,5,6]
[3,2,2,3,4,4,5,5,6]
[4,3,3,3,4,4,5,6,6]
[5,4,4,4,4,5,5,6,7]
[6,5,5,4,5,5,6,6,6]
```

Rolling array optimization:
* `dp[i - 1][j] -> pre[j]`
* `dp[i][j] -> cur[j]`

{% highlight java %}
public int minDistance(String word1, String word2) {
    int[] pre = new int[word2.length() + 1];
    int[] cur = new int[word2.length() + 1];

    for (int j = 1; j <= word2.length(); j++) {
        pre[j] = j;
    }

    for (int i = 1; i <= word1.length(); i++) {
        cur[0] = i;
        for (int j = 1; j <= word2.length(); j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                cur[j] = pre[j - 1];
            } else {
                cur[j] = Math.min(pre[j - 1], Math.min(pre[j], cur[j - 1])) + 1;
            }
        }
        int[] tmp = pre;
        pre = cur;
        cur = tmp;
    }
    return pre[word2.length()];
}
{% endhighlight %}

* `pre[j - 1] -> pre`
* `pre[j] -> cur[j]`
{% highlight java %}
public int minDistance(String word1, String word2) {
    int pre = 0;
    int[] cur = new int[word2.length() + 1];

    for (int j = 1; j <= word2.length(); j++) {
        cur[j] = j;
    }

    for (int i = 1; i <= word1.length(); i++) {
        pre = cur[0];
        cur[0] = i;
        for (int j = 1; j <= word2.length(); j++) {
            int tmp = cur[j];
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                cur[j] = pre;
            } else {
                cur[j] = Math.min(pre, Math.min(cur[j], cur[j - 1])) + 1;
            }
            pre = tmp;
        }
    }
    return cur[word2.length()];
}
{% endhighlight %}

[Min Cost Climbing Stairs][min-cost-climbing-stairs]

{% highlight java %}
public int minCostClimbingStairs(int[] cost) {
    int[] dp = new int[cost.length];
    dp[0] = cost[0];
    dp[1] = cost[1];

    for (int i = 2; i < cost.length; i++) {
        dp[i] = Math.min(dp[i - 1], dp[i - 2]) + cost[i];
    }
    return Math.min(dp[cost.length - 1], dp[cost.length - 2]);
}
{% endhighlight %}

{% highlight java %}
public int minCostClimbingStairs(int[] cost) {
    int dp0 = cost[0], dp1 = cost[1];

    for (int i = 2; i < cost.length; i++) {
        int tmp = Math.min(dp0, dp1) + cost[i];
        dp0 = dp1;
        dp1 = tmp;
    }
    return Math.min(dp0, dp1);
}
{% endhighlight %}

[Greatest Sum Divisible by Three][greatest-sum-divisible-by-three]

-> Divisible by K

{% highlight java %}
public int maxSumDivThree(int[] nums) {
    int k = 3;
    int[] dp = new int[k];
    for (int num : nums) {
        int[] tmp = Arrays.copyOf(dp, k);
        for (int i = 0; i < k; i++) {
            int r = (dp[i] + num) % k;
            tmp[r] = Math.max(tmp[r], dp[i] + num);
        }
        dp = tmp;
    }
    return dp[0];
}
{% endhighlight %}

[edit-distance]: https://leetcode.com/problems/edit-distance/
[greatest-sum-divisible-by-three]: https://leetcode.com/problems/greatest-sum-divisible-by-three/
[min-cost-climbing-stairs]: https://leetcode.com/problems/min-cost-climbing-stairs/
