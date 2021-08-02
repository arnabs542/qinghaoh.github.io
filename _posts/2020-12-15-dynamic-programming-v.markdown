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
public int wiggleMaxLength(int[] nums) {
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
}
{% endhighlight %}

[Flip String to Monotone Increasing][flip-string-to-monotone-increasing]

{% highlight java %}
public int minFlipsMonoIncr(String s) {
    // count of '1's and flips so far
    // to make [0...i] monotone increasing
    int one = 0, flip = 0;
    for (int i = 0; i < s.length(); i++) {
        // [0...i] is monotone increasing
        if (s.charAt(i) == '1') {
            one++;
        } else {
            flip++;
        }

        // either flips all '1's
        // or keeps as is
        flip = Math.min(one, flip);
    }
    return flip;
}
{% endhighlight %}

Similar: [Minimum Deletions to Make String Balanced][minimum-deletions-to-make-string-balanced]

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
    int n = s.length();
    // dp[i]: number of ways ending at s[i - 1]
    int[] dp = new int[n + 1];
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

    return dp[n];
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
        int curr = 0;
        if (s.charAt(i) != '0') {
            curr = oneBack;
        }

        int twoDigits = Integer.parseInt(s.substring(i - 1, i + 1));
        if (twoDigits >= 10 && twoDigits <= 26) {
            curr += twoBack;
        }

        twoBack = oneBack;
        oneBack = curr;
    }
    return oneBack;
}
{% endhighlight %}

[Number of Ways to Paint N × 3 Grid][number-of-ways-to-paint-n-3-grid]

{% highlight java %}
public int numDecodings(String s) {
    if (s.charAt(0) == '0') {
        return 0;
    }

    int oneBack = 1, twoBack = 1;
    for (int i = 1; i < s.length(); i++) {
        int curr = 0;
        if (s.charAt(i) != '0') {
            curr = oneBack;
        }

        int twoDigits = Integer.parseInt(s.substring(i - 1, i + 1));
        if (twoDigits >= 10 && twoDigits <= 26) {
            curr += twoBack;
        }

        twoBack = oneBack;
        oneBack = curr;
    }
    return oneBack;
}
{% endhighlight %}

[Painting a Grid With Three Different Colors][painting-a-grid-with-three-different-colors]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;
private int m, n;
// memo[j][mask]: the number of ways in the j-th column,
//   while the mask is for the m rows in the previous column.
//   It only stores result when r == 0
private int[][] memo;

public int colorTheGrid(int m, int n) {
    this.m = m;
    this.n = n;
    // for each row, the color is stored in 2 bits
    this.memo = new int[n][1 << (m * 2)];

    return dfs(0, 0, 0, 0);
}


private int dfs(int r, int c, int prev, int curr) {
    // found a valid way
    if (c == n) {
        return 1;
    }

    if (r == 0 && memo[c][prev] > 0) {
        return memo[c][prev];
    }

    // completes the current column
    // proceeds to the next column
    if (r == m) {
        return dfs(0, c + 1, curr, 0);
    }

    int count = 0;
    // color mask:
    // - r: 1
    // - g: 2
    // - b: 3
    for (int color = 1; color <= 3; color++) {
        // - same row in the previous column
        // - same column in the previous row (or the first row)
        if (getColor(prev, r) != color && (r == 0 || getColor(curr, r - 1) != color)) {
            // current row picks this color
            // then dfs the next row in the same column
            count = (count + dfs(r + 1, c, prev, setColor(curr, r, color))) % MOD;
        }
    }

    if (r == 0) {
        memo[c][prev] = count;
    }
    return count;
}

private int getColor(int mask, int pos) {
    return (mask >> (pos * 2)) & 0b11;
}

private int setColor(int mask, int pos, int color) {
    return mask | (color << (pos * 2));
}
{% endhighlight %}

[decode-ways]: https://leetcode.com/problems/decode-ways/
[delete-and-earn]: https://leetcode.com/problems/delete-and-earn/
[flip-string-to-monotone-increasing]: https://leetcode.com/problems/flip-string-to-monotone-increasing/
[house-robber]: https://leetcode.com/problems/house-robber/
[house-robber-ii]: https://leetcode.com/problems/house-robber-ii/
[minimum-deletions-to-make-string-balanced]: https://leetcode.com/problems/minimum-deletions-to-make-string-balanced/
[number-of-sets-of-k-non-overlapping-line-segments]: https://leetcode.com/problems/number-of-sets-of-k-non-overlapping-line-segments/
[number-of-ways-to-paint-n-3-grid]: https://leetcode.com/problems/number-of-ways-to-paint-n-3-grid/
[paint-fence]: https://leetcode.com/problems/paint-fence/
[painting-a-grid-with-three-different-colors]: https://leetcode.com/problems/painting-a-grid-with-three-different-colors/
[wiggle-subsequence]: https://leetcode.com/problems/wiggle-subsequence/
