---
layout: post
title:  "Dynamic Programming V"
tag: dynamic programming
---
[Paint Fence][paint-fence]

![Paint Fence](/assets/paint_fence.png)

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
    int n = nums.length;
    int[] dp = new int[n + 1];
    dp[1] = nums[0];
    for (int i = 1; i < n; i++) {
        dp[i + 1] = Math.max(dp[i], dp[i - 1] + nums[i]);
    }       
    return dp[n];
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

[Delete and Earn][delete-and-earn]

{% highlight java %}
public int deleteAndEarn(int[] nums) {
    int[] sum = new int[10001];
    for (int num : nums) {
        sum[num] += num;
    }

    int take = 0, skip = 0;
    for (int s : sum) {
        int tmp = skip;
        skip = Math.max(skip, take);
        take = tmp + s;
    }
    return Math.max(take, skip);
}
{% endhighlight %}

[Wiggle Subsequence][wiggle-subsequence]

{% highlight java %}
// max wiggle sequence length so far at index i
    int up = 1, down = 1;
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] > nums[i - 1]) {
            up = down + 1;
        } else if (nums[i] < nums[i - 1]) {
            down = up + 1;
        }
    }
    return Math.max(up, down);
{% endhighlight %}

[Number of Sets of K Non-Overlapping Line Segments][number-of-sets-of-k-non-overlapping-line-segments]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int numberOfSets(int n, int k) {
    // dp[][i][j]
    // 0: lines don't start from i
    // 1: lines start from i
    int[][][] dp = new int[2][n][k + 1];
    for (int j = 0; j < n; j++) {
        dp[0][j][0] = 1;
        dp[1][j][0] = 1;
    }

    for (int i = n - 2; i >= 0; i--) {
        for (int j = 1; j <= k; j++) {
            dp[0][i][j] = (dp[0][i + 1][j] + dp[1][i + 1][j]) % MOD;
            dp[1][i][j] = (dp[1][i + 1][j] + dp[0][i][j - 1]) % MOD;
        }
    }

    return (dp[0][0][k] + dp[1][0][k]) % MOD;
}
{% endhighlight %}

[Decode Ways][decode-ways]

{% highlight java %}
public int numDecodings(String s) {
    int[] dp = new int[s.length() + 1];
    dp[0] = 1;
    dp[1] = s.charAt(0) == '0' ? 0 : 1;

    for (int i = 2; i < dp.length; i++) {
        // one digit
        if (s.charAt(i - 1) != '0') {
            dp[i] += dp[i - 1];
        }

        // two digits
        int twoDigits = Integer.valueOf(s.substring(i - 2, i));
        if (twoDigits >= 10 && twoDigits <= 26) {
            dp[i] += dp[i - 2];
        }
    }

    return dp[s.length()];
}
{% endhighlight %}

Reduced to 1D:

{% highlight java %}
public int numDecodings(String s) {
    if (s.charAt(0) == '0') {
        return 0;
    }

    int oneBack = 1, twoBack = 1;
    for (int i = 1; i < s.length(); i++) {
        int current = 0;
        if (s.charAt(i) != '0') {
            current = oneBack;
        }

        int twoDigits = Integer.parseInt(s.substring(i - 1, i + 1));
        if (twoDigits >= 10 && twoDigits <= 26) {
            current += twoBack;
        }

        twoBack = oneBack;
        oneBack = current;
    }
    return oneBack;
}
{% endhighlight %}

[decode-ways]: https://leetcode.com/problems/decode-ways/
[delete-and-earn]: https://leetcode.com/problems/delete-and-earn/
[house-robber]: https://leetcode.com/problems/house-robber/
[house-robber-ii]: https://leetcode.com/problems/house-robber-ii/
[number-of-sets-of-k-non-overlapping-line-segments]: https://leetcode.com/problems/number-of-sets-of-k-non-overlapping-line-segments/
[paint-fence]: https://leetcode.com/problems/paint-fence/
[wiggle-subsequence]: https://leetcode.com/problems/wiggle-subsequence/
