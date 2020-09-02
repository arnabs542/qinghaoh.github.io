---
layout: post
title:  "Knapsack Problem"
tag: dynamic programming
usemathjax: true
---
[Knapsack problem](https://en.wikipedia.org/wiki/Knapsack_problem)

## 0-1 Knapsack Problem

maximize $$ \sum _{i=1}^{n}v_{i}x_{i} $$

subject to $$ \sum _{i=1}^{n}w_{i}x_{i}\leq W $$ and $$ x_{i}\in \{0,1\} $$

### Backtracking
Backtracking takes `O(2^n)` time, so it's less preferred.

### Dynamic Programming
[Partition Equal Subset Sum][partition-equal-subset-sum]

{% highlight java %}
public boolean canPartition(int[] nums) {
    int sum = 0;
    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];
    }

    if (sum % 2 == 1) {
        return false;
    }

    // dp[i][j]: whether the first i elements can sum up to j
    boolean[][] dp = new boolean[nums.length + 1][sum / 2 + 1];
    dp[0][0] = true;

    for (int i = 0; i < nums.length; i++) {
        for (int j = 0; j <= sum / 2; j++) {
            if (j < nums[i]) {
                dp[i + 1][j] = dp[i][j];
            } else {
                dp[i + 1][j] = dp[i][j] || dp[i][j - nums[i]];
            }
        }
    }
    return dp[nums.length][sum / 2];
}
{% endhighlight %}

For example, `nums = [1,2,5,1]`, then `dp` is:

```
[true,false,false,false,false,false]
[true,true,false,false,false,false]
[true,true,true,true,false,false]
[true,true,true,true,false,true]
[true,true,true,true,true,true]
```

![2D](/assets/knapsack_partition_equal_subset_sum_2d.png)

{% highlight java %}
public boolean canPartition(int[] nums) {
    int sum = 0;
    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];
    }

    if (sum % 2 == 1) {
        return false;
    }

    boolean[] dp = new boolean[sum / 2 + 1];
    dp[0] = true;

    for (int num : nums) {
        // dp[k + 1][i] depends on dp[k][i - num]
        //  so the iteration order is reversed
        for (int i = sum / 2; i >= num; i--) {
            dp[i] = dp[i] || dp[i - num];
        }
    }
    return dp[sum / 2];
}
{% endhighlight %}

In 2D, `dp[i + 1][j] = dp[i][j] || dp[i][j - nums[i]]`. The reverse iteration ensures `dp[i][j - nums[i]]` is not updated to `dp[i + 1][j - nums[i]]` before we update `dp[i][j]` to `dp[i + 1][j]`.

![1D](/assets/knapsack_partition_equal_subset_sum_1d.png)

[Target Sum][target-sum]

{% highlight java %}
public int findTargetSumWays(int[] nums, int S) {
    int sum = 0;
    for (int num : nums) {
        sum += num;
    }

    // d[i][j]: ways to sum up to j with the first i elements
    int[][] dp = new int[nums.length][2 * sum + 1];

    // + sum ensures the index >= 0
    dp[0][nums[0] + sum] = 1;
    dp[0][-nums[0] + sum] += 1;  // +0, -0

    for (int i = 1; i < nums.length; i++) {
        for (int j = -sum; j <= sum; j++) {
            if (dp[i - 1][j + sum] > 0) {
                dp[i][j + nums[i] + sum] += dp[i - 1][j + sum];
                dp[i][j - nums[i] + sum] += dp[i - 1][j + sum];
            }
        }
    }

    return S > sum ? 0 : dp[nums.length - 1][S + sum];
}
{% endhighlight %}

Refer to subset sum problem:

{% highlight java %}
public int findTargetSumWays(int[] nums, int S) {
    int sum = 0;
    for (int num : nums) {
        sum += num;
    }

    // sum(P) - sum(N) == S
    // sum(P) - (sum(nums) - sum(P)) == S
    // 2 * sum(P) == S + sum(nums)
    // sum(P) == (S + sum(nums)) / 2
    return sum < S || (S + sum) % 2 > 0 ? 0 : subsetSum(nums, (S + sum) >>> 1); 
}   

private int subsetSum(int[] nums, int S) {
    int[] dp = new int[S + 1]; 
    dp[0] = 1;
    for (int num : nums) {
        for (int i = S; i >= num; i--) {
            dp[i] += dp[i - num]; 
        }
    }
    return dp[S];
}
{% endhighlight %}

## Unbounded Knapsack Problem (UKP)

maximize $$ \sum _{i=1}^{n}v_{i}x_{i} $$
subject to $$ \sum _{i=1}^{n}w_{i}x_{i}\leq W $$ and $$ x_{i}\geq 0,\ x_{i}\in \mathbb {Z} $$

[Coin Change 2][coin-change-2]

{% highlight java %}
public int change(int amount, int[] coins) {
    // dp[i][j]: combinations to make up amount j by using the first i kinds of coins
    int[][] dp = new int[coins.length + 1][amount + 1];
    dp[0][0] = 1;

    for (int i = 0; i < coins.length; i++) {  
        for (int j = 0; j <= amount; j++) {
            if (j < coins[i]) {
                dp[i + 1][j] = dp[i][j];
            } else {
                dp[i + 1][j] = dp[i][j] + dp[i + 1][j - coins[i]];
            }
        }
    }
    return dp[coins.length][amount];
}
{% endhighlight %}

For example, `amount = 5, coins = [1, 2, 5]`, then `dp` is:

```
[1,0,0,0,0,0]
[1,1,1,1,1,1]
[1,1,2,2,3,3]
[1,1,2,2,3,4]
```

![2D](/assets/knapsack_coin_change_2_2d.png)

{% highlight java %}
public int change(int amount, int[] coins) {
    int[] dp = new int[amount + 1];
    dp[0] = 1;

    for (int coin : coins) {  
        for (int i = 0; i <= amount; i++) {
            if (i >= coin) {
                dp[i] += dp[i - coin];
            }
        }
    }
    return dp[amount];
}
{% endhighlight %}

In 2D, `dp[i + 1][j] = dp[i][j] + dp[i + 1][j - nums[i]]`. The natural iteration ensures `dp[i][j - nums[i]]` is updated to `dp[i + 1][j - nums[i]]` before we update `dp[i][j]` to `dp[i + 1][j]`.

![1D](/assets/knapsack_coin_change_2_1d.png)

### Change-making Problem
[Change-making problem](https://en.wikipedia.org/wiki/Change-making_problem)

minimize $$ f(W)=\sum _{j=1}^{n}x_{j} $$

subject to $$ \sum _{j=1}^{n}w_{j}x_{j}=W $$

{% highlight java %}
public int coinChange(int[] coins, int amount) {
    int[][] dp = new int[coins.length + 1][amount + 1];
    int max = amount + 1;
    for (int i = 0; i < dp.length; i++) {
        Arrays.fill(dp[i], max);
    }
    for (int i = 0; i < dp.length; i++) {
        dp[i][0] = 0;
    }

    for (int i = 0; i < coins.length; i++) {
        for (int j = 0; j <= amount; j++) {
            if (j < coins[i]) {
                dp[i + 1][j] = dp[i][j];
            } else {
                dp[i + 1][j] = Math.min(dp[i][j], dp[i + 1][j - coins[i]] + 1);
            }
        }
    }
    return dp[coins.length][amount] == max ? -1 : dp[coins.length][amount];
}
{% endhighlight %}

{% highlight java %}
public int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1);
    dp[0] = 0;

    for (int coin : coins) {
        for (int i = 0; i <= amount; i++) {
            if (i >= coin) {
                dp[i] = Math.min(dp[i], dp[i - coin] + 1);
            }
        }
    }
    return dp[amount] > amount ? -1 : dp[amount];
}
{% endhighlight %}

[coin-change-2]: https://leetcode.com/problems/coin-change-2/
[partition-equal-subset-sum]: https://leetcode.com/problems/partition-equal-subset-sum/
[target-sum]: https://leetcode.com/problems/target-sum/
