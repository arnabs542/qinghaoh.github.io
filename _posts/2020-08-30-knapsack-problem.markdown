---
layout: post
title:  "Knapsack Problem"
tag: dynamic programming
usemathjax: true
---
[Knapsack problem](https://en.wikipedia.org/wiki/Knapsack_problem)

# 0-1 Knapsack Problem

maximize $$ \sum _{i=1}^{n}v_{i}x_{i} $$

subject to $$ \sum _{i=1}^{n}w_{i}x_{i}\leq W $$ and $$ x_{i}\in \{0,1\} $$

## Backtracking

Backtracking takes `O(2^n)` time, so it's less preferable.

## Dynamic Programming

[Partition Equal Subset Sum][partition-equal-subset-sum]

With full dimensionality (no reduction), we can backtrace.

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

[Last Stone Weight II][last-stone-weight-ii]

{% highlight java %}
private static final int MAX = 30 * 100;

public int lastStoneWeightII(int[] stones) {
    // smaller group
    boolean[] dp = new boolean[MAX / 2 + 1];
    dp[0] = true;

    int sum = 0;
    for (int s : stones) {
        sum += s;
        // min to ensure smaller group
        for (int i = Math.min(MAX / 2, sum); i >= s; i--) {
            dp[i] |= dp[i - s];
        }
    }

    for (int i = sum / 2; i >= 0; i--) {
        if (dp[i]) {
            return sum - i - i;
        }
    }
    return 0;
}
{% endhighlight %}

[Toss Strange Coins][toss-strange-coins]

{% highlight java %}
public double probabilityOfHeads(double[] prob, int target) {
    int n = prob.length;

    // dp[i][j]: whether the first i elements can sum up to j
    double[][] dp = new double[n + 1][target + 1];
    dp[0][0] = 1d;

    for (int i = 0; i < n; i++) {
        dp[i + 1][0] = dp[i][0] * (1 - prob[i]);
    }

    for (int i = 0; i < n; i++) {
        for (int j = 1; j <= target; j++) {
            dp[i + 1][j] = dp[i][j] * (1 - prob[i]) + dp[i][j - 1] * prob[i];
        }
    }
    return dp[n][target];
}
{% endhighlight %}

### 3D

[Ones and Zeroes][ones-and-zeroes]

{% highlight java %}
public int findMaxForm(String[] strs, int m, int n) {
    int[][][] dp = new int[strs.length + 1][m + 1][n + 1];

    for (int i = 0; i < strs.length; i++) {
        int zeroes = 0, ones = 0;
        for (char c : strs[i].toCharArray()) {
            if (c == '0') {
                zeroes++;
            } else {
                ones++;
            }
        }

        for (int j = 0; j <= m; j++) {
            for (int k = 0; k <= n; k++) {
                if (j < zeroes || k < ones) {
                    dp[i + 1][j][k] = dp[i][j][k];
                } else {
                    dp[i + 1][j][k] = Math.max(dp[i][j][k], dp[i][j - zeroes][k - ones] + 1);
                }
            }
        }
    }

    return dp[strs.length][m][n];
}
{% endhighlight %}

2D:

{% highlight java %}
public int findMaxForm(String[] strs, int m, int n) {
    int[][] dp = new int[m + 1][n + 1];

    for (String str : strs) {
        int zeroes = 0, ones = 0;
        for (char c : str.toCharArray()) {
            if (c == '0') {
                zeroes++;
            } else {
                ones++;
            }
        }

        for (int i = m; i >= zeroes; i--) {
            for (int j = n; j >= ones; j--) {
                dp[i][j] = Math.max(dp[i][j], dp[i - zeroes][j - ones] + 1);
            }
        }
    }

    return dp[m][n];
}
{% endhighlight %}

[Profitable Schemes][profitable-schemes]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

// Knapsack
public int profitableSchemes(int n, int minProfit, int[] group, int[] profit) {
    // dp[i][j]: count of schemes with profit >= j done by exactly i members
    int[][] dp = new int[n + 1][minProfit + 1];
    dp[0][0] = 1;

    for (int k = 0; k < group.length; k++) {
        for (int i = n; i >= group[k]; i--) {
            for (int j = minProfit; j >= 0; j--) {
                dp[i][j] = (dp[i][j] + dp[i - group[k]][Math.max(0, j - profit[k])]) % MOD;
            }
        }
    }

    int count = 0;
    for (int i = 0; i < dp.length; i++){
        count = (count + dp[i][minProfit]) % MOD;
    }
    return count;
}
{% endhighlight %}

[Split Array With Same Average][split-array-with-same-average]

{% highlight java %}
// NP
public boolean splitArraySameAverage(int[] nums) {
    int n = nums.length, sum = 0;
    for (int num : nums) {
        sum += num;
    }

    // if avg(A) = avg(B) = avg(nums)
    // assumes size(A) <= size(B)
    // sum(A) = avg(nums) * size(A)
    //        = sum * size(A) / n

    // dp[i][j]: whether it's possible to sum to i using j elements
    boolean[][] dp = new boolean[sum + 1][n / 2 + 1];
    dp[0][0] = true;

    for (int num : nums) {
        for (int i = sum; i >= num; i--) {
            for (int j = 1; j <= n / 2; j++) {
                dp[i][j] = dp[i][j] || dp[i - num][j - 1];
            }
        }
    }

    for (int i = 1; i <= n / 2; i++)  {
        if (sum * i % n == 0 && dp[sum * i / n][i]) {
            return true;
        }
    }
    return false;
}
{% endhighlight %}

# Unbounded Knapsack Problem (UKP)

maximize $$ \sum _{i=1}^{n}v_{i}x_{i} $$

subject to $$ \sum _{i=1}^{n}w_{i}x_{i}\leq W $$ and $$ x_{i}\geq 0,\ x_{i}\in \mathbb {Z} $$

[Coin Change 2][coin-change-2]

{% highlight java %}
public int change(int amount, int[] coins) {
    int n = coins.length;

    // dp[i][j]: combinations to make up amount j by using the first i kinds of coins
    int[][] dp = new int[n + 1][amount + 1];
    dp[0][0] = 1;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j <= amount; j++) {
            if (j < coins[i]) {
                dp[i + 1][j] = dp[i][j];
            } else {
                dp[i + 1][j] = dp[i][j] + dp[i + 1][j - coins[i]];
            }
        }
    }
    return dp[n][amount];
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

[Form Largest Integer With Digits That Add up to Target][form-largest-integer-with-digits-that-add-up-to-target]

## Permutation Sum

The below permutation sum (yes it's permutation, ignore the wrong problem name) is not a knapsack problem, but the only difference is the loop order:

[Combination Sum IV][combination-sum-iv]

{% highlight java %}
public int combinationSum4(int[] nums, int target) {
    int[] dp = new int[target + 1];
    dp[0] = 1;

    for (int i = 0; i <= target; i++) {
        for (int num : nums) {
            if (i >= num) {
                dp[i] += dp[i - num];
            }
        }
    }
    return dp[target];
}
{% endhighlight %}

In essence, it's recursion.

## Change-making Problem
[Change-making problem](https://en.wikipedia.org/wiki/Change-making_problem)

minimize $$ f(W)=\sum _{j=1}^{n}x_{j} $$

subject to $$ \sum _{j=1}^{n}w_{j}x_{j}=W $$

[Coin Change][coin-change]

{% highlight java %}
public int coinChange(int[] coins, int amount) {
    int n = coins.length, max = amount + 1;

    int[][] dp = new int[n + 1][amount + 1];
    for (int i = 0; i < dp.length; i++) {
        Arrays.fill(dp[i], max);
    }
    for (int i = 0; i < dp.length; i++) {
        dp[i][0] = 0;
    }

    for (int i = 0; i < n; i++) {
        for (int j = 0; j <= amount; j++) {
            if (j < coins[i]) {
                dp[i + 1][j] = dp[i][j];
            } else {
                dp[i + 1][j] = Math.min(dp[i][j], dp[i + 1][j - coins[i]] + 1);
            }
        }
    }
    return dp[n][amount] == max ? -1 : dp[n][amount];
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

[coin-change]: https://leetcode.com/problems/coin-change/
[coin-change-2]: https://leetcode.com/problems/coin-change-2/
[combination-sum-iv]: https://leetcode.com/problems/combination-sum-iv/
[form-largest-integer-with-digits-that-add-up-to-target]: https://leetcode.com/problems/form-largest-integer-with-digits-that-add-up-to-target/
[last-stone-weight-ii]: https://leetcode.com/problems/last-stone-weight-ii/
[ones-and-zeroes]: https://leetcode.com/problems/ones-and-zeroes/
[partition-equal-subset-sum]: https://leetcode.com/problems/partition-equal-subset-sum/
[profitable-schemes]: https://leetcode.com/problems/profitable-schemes/
[split-array-with-same-average]: https://leetcode.com/problems/split-array-with-same-average/
[target-sum]: https://leetcode.com/problems/target-sum/
[toss-strange-coins]: https://leetcode.com/problems/toss-strange-coins/
