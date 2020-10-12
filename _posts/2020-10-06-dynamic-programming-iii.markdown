---
layout: post
title:  "Dynamic Programming III"
tag: dynamic programming
---
[Best Time to Buy and Sell Stock IV][best-time-to-buy-and-sell-stock-iv]

```
dp[k][i] = max(dp[k][i - 1], prices[i] - prices[j] + dp[k - 1][j - 1]), j = [0, 1, ..., i - 1]
```

{% highlight java %}
public int maxProfit(int k, int[] prices) {
    if (k >= prices.length / 2) {
        // Best Time to Buy and Sell Stock II
        int profit = 0;
        for (int i = 1; i < prices.length; i++) {
            profit += Math.max(0, prices[i] - prices[i - 1]);
        }
        return profit;
    }

    // dp[i][j]: max profit on j-th day with i transactions
    int[][] dp = new int[k + 1][prices.length];
    for (int m = 1; m <= k; m++) {
        for (int i = 1; i < prices.length; i++) {
            int min = prices[0];
            for (int j = 1; j <= i; j++) {
                min = Math.min(min, prices[j] - dp[m - 1][j - 1]);
            }
            dp[m][i] = Math.max(dp[m][i - 1], prices[i] - min);
        }
    }
    return dp[k][prices.length - 1];
}
{% endhighlight %}

Reduces the repetitive calculation of min:

{% highlight java %}
for (int m = 1; m <= k; m++) {
    int min = prices[0];
    for (int i = 1; i < prices.length; i++) {
	min = Math.min(min, prices[i] - dp[m - 1][i - 1]);
	dp[m][i] = Math.max(dp[m][i - 1], prices[i] - min);
    }
}
{% endhighlight %}

Swaps the two for-loops. `min` becomes an array to store min of each transaction.

{% highlight java %}
int[] min = new int[k + 1];
Arrays.fill(min, prices[0]);      
for (int i = 1; i < prices.length; i++) {
    for (int j = 1; j <= k; j++) {
	min[j] = Math.min(min[j], prices[i] - dp[j - 1][i - 1]);
	dp[j][i] = Math.max(dp[j][i - 1], prices[i] - min[j]);
    }
}
{% endhighlight %}

Reduces to 1D:

{% highlight java %}
int[] dp = new int[k + 1];
int[] min = new int[k + 1];
Arrays.fill(min, prices[0]);      
for (int i = 1; i < prices.length; i++) {
    for (int j = 1; j <= k; j++) {
	min[j] = Math.min(min[j], prices[i] - dp[j - 1]);
	dp[j] = Math.max(dp[j], prices[i] - min[j]);
    }
}        
return dp[k];
{% endhighlight %}

[Best Time to Buy and Sell Stock III][best-time-to-buy-and-sell-stock-iii]

In the last solution of [Best Time to Buy and Sell Stock IV][best-time-to-buy-and-sell-stock-iv], replaces `k` with `2`:

{% highlight java %}
public int maxProfit(int[] prices) {
    int buy1 = Integer.MAX_VALUE, buy2 = Integer.MAX_VALUE;
    int sell1 = 0, sell2 = 0;
    for (int price : prices) {
        buy1 = Math.min(buy1, price);
        sell1 = Math.max(sell1, price - buy1);
        buy2 = Math.min(buy2, price - sell1);
        sell2 = Math.max(sell2, price - buy2);
    }
    return sell2;
}
{% endhighlight %}

[Best Time to Buy and Sell Stock with Cooldown][best-time-to-buy-and-sell-stock-with-cooldown]

```
dp[i] = max(dp[i - 1], prices[i] - prices[j] + dp[j - 2]), j = [0, 1, ..., i - 1]
```

{% highlight java %}
public int maxProfit(int[] prices) {
    if (prices.length < 2) {
        return 0;
    }

    int[] dp = new int[prices.length + 1];
    int min = prices[0];
    for (int i = 1; i < prices.length; i++) {
        min = Math.min(min, prices[i] - dp[i - 1]);
        dp[i + 1] = Math.max(dp[i], prices[i] - min);
    }
    return dp[prices.length];
}
{% endhighlight %}

Reduced to 0D:

{% highlight java %}
int prev = 0, curr = 0;
int min = prices[0];
for (int i = 1; i < prices.length; i++) {
    min = Math.min(min, prices[i] - prev);
    prev = curr;
    curr = Math.max(curr, prices[i] - min);
}
return curr;
{% endhighlight %}

[best-time-to-buy-and-sell-stock-iii]: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/
[best-time-to-buy-and-sell-stock-iv]: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/
[best-time-to-buy-and-sell-stock-with-cooldown]: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/
