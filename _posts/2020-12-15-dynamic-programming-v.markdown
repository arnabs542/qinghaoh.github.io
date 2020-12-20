---
layout: post
title:  "Dynamic Programming V"
tag: dynamic programming
---
![Paint Fence](/assets/paint_fence.png)

[Paint Fence][paint-fence]

{% highlight java %}
public int numWays(int n, int k) {
    if (n == 0) {
        return 0;
    }
    if (n == 1) {
        return k;
    }

    int[] dp = new int[n + 1];
    dp[1] = k;
    dp[2] = k * k;

    for (int i = 3; i <= n; i++) {
        dp[i] = (dp[i - 1] + dp[i - 2]) * (k - 1);
    }
    return dp[n];
}
{% endhighlight %}

Reduced to 0D:

{% highlight java %}
public int numWays(int n, int k) {
    if (n == 0) {
        return 0;
    }
    if (n == 1) {
        return k;
    }

    int prev = k * k, prevPrev = k;
    for (int i = 3; i <= n; i++) {
        int tmp = (prev + prevPrev) * (k - 1);
        prevPrev = prev;
        prev = tmp;
    }
    return prev;
}
{% endhighlight %}

[House Robber][house-robber]

{% highlight java %}
public int rob(int[] nums) {
    int[] dp = new int[nums.length + 1];
    dp[1] = nums[0];
    for (int i = 1; i < nums.length; i++) {
        dp[i + 1] = Math.max(dp[i], dp[i - 1] + nums[i]);
    }       
    return dp[nums.length];
}
{% endhighlight %}

Reduced to 0D:

{% highlight java %}
public int rob(int[] nums) {
    int prev = 0, curr = 0;
    for (int num : nums) {
        int tmp = curr;
        curr = Math.max(curr, prev + num);
        prev = tmp;
    }
    return curr;
}
{% endhighlight %}

[House Robber II][house-robber-ii]

{% highlight java %}
public int rob(int[] nums) {
    if (nums.length == 1) {
        return nums[0];
    }
    return Math.max(rob(nums, 0, nums.length - 1), rob(nums, 1, nums.length));
}

// 198. House Robber
private int rob(int[] nums, int start, int end) {
    int prev = 0, curr = 0;
    for (int i = start; i < end; i++) {
        int tmp = curr;
        curr = Math.max(curr, prev + nums[i]);
        prev = tmp;
    }
    return curr;
}
{% endhighlight %}

[Dice Roll Simulation][dice-roll-simulation]

![Reduction](/assets/dice_roll_simulation.png)

{% highlight java %}
private final int MOD = (int)1e9 + 7;
private final int SIDES = 6;

public int dieSimulator(int n, int[] rollMax) {
    // dp[i][j]: number of distinct sequences at i-th roll and the last number is j
    // if j == SIDES, it's the total number of distinct sequences at i-th roll
    int[][] dp = new int[n + 1][SIDES + 1];

    // initialization
    dp[0][SIDES] = 1;

    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < SIDES; j++) {
            // if there's no constraint
            dp[i][j] = dp[i - 1][SIDES];

            if (i - rollMax[j] > 0) {
                // e.g. rollMax[1] = 2, and the rolls so far are: a, x, x, b
                // if b == 1, then we should exclude all possible cases of a, 1, 1
                // where a != 1
                int reduction = dp[i - rollMax[j] - 1][SIDES] - dp[i - rollMax[j] - 1][j];
                dp[i][j] = ((dp[i][j] - reduction) % MOD + MOD) % MOD;
            }

            dp[i][SIDES] = (dp[i][SIDES] + dp[i][j]) % MOD;              
        }
    }

    return dp[n][SIDES];
}
{% endhighlight %}

[dice-roll-simulation]: https://leetcode.com/problems/dice-roll-simulation/
[house-robber]: https://leetcode.com/problems/house-robber/
[house-robber-ii]: https://leetcode.com/problems/house-robber-ii/
[paint-fence]: https://leetcode.com/problems/paint-fence/
