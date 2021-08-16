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

    // "at most K groups" is equivalent to "exact K groups"
    // so we don't need to consider largestSumOfAverages(A, end, K - 1)
    //  
    // see https://en.wikipedia.org/wiki/Mediant_(mathematics)#Properties
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

[Maximum Score from Performing Multiplication Operations][maximum-score-from-performing-multiplication-operations]

{% highlight java %}
private int n, m;
private int[] nums, multipliers;
private int[][] memo;

public int maximumScore(int[] nums, int[] multipliers) {
    this.n = nums.length;
    this.m = multipliers.length;
    this.nums= nums;
    this.multipliers = multipliers;
    this.memo = new int[m][m];  // m is much smaller than n, avoid MLE
    for (int i = 0; i < m; i++) {
        Arrays.fill(memo[i], Integer.MIN_VALUE);
    }
    return helper(0, 0);
}

private int helper(int start, int index) {
    if (index == m) {
        return 0;
    }

    if (memo[start][index] != Integer.MIN_VALUE) {
        return memo[start][index];
    }

    int left = helper(start + 1, index + 1) + nums[start] * multipliers[index];
    int right = helper(start, index + 1) + nums[start + n - index - 1] * multipliers[index];
    return memo[start][index] = Math.max(left, right);
}
{% endhighlight %}

[Minimum Score Triangulation of Polygon][minimum-score-triangulation-of-polygon]

{% highlight java %}
private int[][] memo;

public int minScoreTriangulation(int[] values) {
    int n = values.length;
    memo = new int[n][n];
    return minScore(values, 0, n - 1);
}

private int minScore(int[] values, int start, int end) {
    if (start + 1 == end) {
        return 0;
    }

    if (memo[start][end] > 0) {
        return memo[start][end];
    }

    int min = Integer.MAX_VALUE;
    for (int i = start + 1; i < end; i++) {
        int sum = values[start] * values[end] * values[i];
        sum += minScore(values, start, i);
        sum += minScore(values, i, end);
        min = Math.min(min, sum);
    }
    return memo[start][end] = min;
}
{% endhighlight %}

{% highlight java %}
public int minScoreTriangulation(int[] values) {
    int n = values.length;        
    int[][] dp = new int[n][n];
    for (int i = n - 1; i >= 0; i--) {
        for (int j = i + 2; j < n; j++) {
            dp[i][j] = Integer.MAX_VALUE;
            for (int k = i + 1; k < j; k++) {
                dp[i][j] = Math.min(dp[i][j], dp[i][k] + dp[k][j] + values[i] * values[j] * values[k]);
            }
        }
    }
    return dp[0][n - 1];
}
{% endhighlight %}

{% highlight java %}
    for (int j = 2; j < n; j++) {
        for (int i = j - 2; i >= 0; i--) {

        }
    }
{% endhighlight %}

[Burst Balloons][burst-balloons]

{% highlight java %}
public int maxCoins(int[] nums) {
    int n = nums.length;

    // dp[i][j]: max coins after bursting all ballons in the range nums[i...j]
    //   and the rest balloons are NOT bursted. (reverse thinking)
    int[][] dp = new int[n][n];

    for (int len = 1; len <= n; len++) {
        for (int left = 0; left + len - 1 < n; left++) {
            int right = left + len - 1;
            // reverse thinking: finds the balloon that's last to burst
            // every element of the range [left, right] could be the last balloon to burst
            for (int i = left; i <= right; i++) {
                int leftNum = (left == 0) ? 1 : nums[left - 1];
                int rightNum = (right == n - 1) ? 1 : nums[right + 1];

                int leftSum = (i == left) ? 0 : dp[left][i - 1];
                int rightSum = (i == right) ? 0 : dp[i + 1][right];
                dp[left][right] = Math.max(dp[left][right], leftNum * nums[i] * rightNum + leftSum + rightSum);
            }
        }
    }

    return dp[0][n - 1];
}
{% endhighlight %}

[Remove Boxes][remove-boxes]

{% highlight java %}
{% endhighlight %}

## State Transition

[Partition to K Equal Sum Subsets][partition-to-k-equal-sum-subsets]

{% highlight java %}
public boolean canPartitionKSubsets(int[] nums, int k) {
    int sum = 0, max = 0;
    for (int num : nums) {
        sum += num;
        max = Math.max(max, num);
    }

    int target = sum / k;
    if (sum % k != 0 || max > target) {
        return false;
    }

    // searches in reverse order, so that subset sizes decrease faster
    Arrays.sort(nums);

    // dp[state] indicates whether state is a possible candidate
    boolean[] dp = new boolean[1 << nums.length];
    dp[0] = true;
    int[] total = new int[1 << nums.length];

    // bit mask
    // the i-th bit in state represents the presence of nums[i]
    for (int state = 0; state < (1 << nums.length); state++) {
        if (!dp[state]) {
            continue;
        }

        for (int i = 0; i < nums.length; i++) {
            int next = state | (1 << i);  // next state
            // state doesn't set the i-th position
            // next state is not a possible candidate yet
            // nums[i] fits into the current partial subset
            if (state != next && !dp[next] && nums[i] + total[state] % target <= target) {
                dp[next] = true;
                total[next] = total[state] + nums[i];
            }
        }
    }

    return dp[(1 << nums.length) - 1];
}
{% endhighlight %}

[burst-balloons]: https://leetcode.com/problems/burst-balloons/
[largest-sum-of-averages]: https://leetcode.com/problems/largest-sum-of-averages/
[maximum-score-from-performing-multiplication-operations]: https://leetcode.com/problems/maximum-score-from-performing-multiplication-operations/
[minimum-score-triangulation-of-polygon]: https://leetcode.com/problems/minimum-score-triangulation-of-polygon/
[partition-to-k-equal-sum-subsets]: https://leetcode.com/problems/partition-to-k-equal-sum-subsets/
[remove-boxes]: https://leetcode.com/problems/remove-boxes/
