---
layout: post
title:  "State Machine"
---
[Best Time to Buy and Sell Stock with Transaction Fee][best-time-to-buy-and-sell-stock-with-transaction-fee]

![state machine](/assets/best_time_to_buy_and_sell_stock_with_transaction_fee.png)

{% highlight java %}
public int maxProfit(int[] prices, int fee) {
    int[] s0 = new int[prices.length];  // cash
    int[] s1 = new int[prices.length];  // hold

    s0[0] = 0;
    s1[0] = -prices[0];

    for (int i = 1; i < prices.length; i++) {
        s0[i] = Math.max(s0[i - 1], s1[i - 1] + prices[i] - fee);
        s1[i] = Math.max(s1[i - 1], s0[i - 1] - prices[i]);
    }

    return s0[prices.length - 1];
}
{% endhighlight %}

Reduced to 0D:

{% highlight java %}
public int maxProfit(int[] prices, int fee) {
    int hold = -prices[0], cash = 0;
    for (int price : prices) {
        int prevCash = cash;
        cash = Math.max(cash, hold + price - fee);
        hold = Math.max(hold, prevCash - price);
    }
    return Math.max(cash, hold);
}
{% endhighlight %}

It can be simplified as:

{% highlight java %}
public int maxProfit(int[] prices, int fee) {
    int hold = -prices[0], cash = 0;
    for (int price : prices) {
        cash = Math.max(cash, hold + price - fee);

        // hold2 = max(hold1, cash2 - prices[i]);
        //   = max(hold1, max(cash1, hold1 + prices[i] - fee) - prices[i])
        //   = max(hold1, max(cash1 - prices[i], hold1 - fee))
        //   = max(hold1, cash1 - prices[i], hold1 - fee)
        //   = max(hold1, cash1 - prices[i])
        hold = Math.max(hold, cash - price);
    }

    // max(hold2, cash2)
    //   = max(hold1, cash1 - prices[i], cash1, hold1 + prices[i] - fee)
    //   = max(hold1, cash1, hold1 + prices[i] - fee)
    //   = max(hold1, cash2)
    //   ...
    //   = Math.max(-prices[0], cash2)
    // -prices[0] < 0 = cash0 <= cash2
    //
    // The profit after selling is higher than holding
    return cash;
}
{% endhighlight %}

[Best Time to Buy and Sell Stock with Cooldown][best-time-to-buy-and-sell-stock-with-cooldown]

![state machine](/assets/best_time_to_buy_and_sell_stock_with_cooldown.png)

{% highlight java %}
public int maxProfit(int[] prices) {
    if (prices.length == 0) {
        return 0;
    }

    int[] s0 = new int[prices.length];  // cash, not immediate after selling
    int[] s1 = new int[prices.length];  // hold
    int[] s2 = new int[prices.length];  // sold, immediate after selling

    s0[0] = 0;
    s1[0] = -prices[0];
    s2[0] = Integer.MIN_VALUE;

    for (int i = 1; i < prices.length; i++) {
        s0[i] = Math.max(s0[i - 1], s2[i - 1]);
        s1[i] = Math.max(s1[i - 1], s0[i - 1] - prices[i]);
        s2[i] = s1[i - 1] + prices[i];
    }

    return Math.max(s0[prices.length - 1], s2[prices.length - 1]);
}
{% endhighlight %}

Reduced to 0D:

{% highlight java %}
public int maxProfit(int[] prices) {
    int cash = 0, hold = Integer.MIN_VALUE, sold = 0;
    for (int price : prices) {
        int prevSold = sold;
        sold = hold + price;
        hold = Math.max(hold, cash - price);
        cash = Math.max(cash, prevSold);
    }
    return Math.max(sold, cash);
}
{% endhighlight %}

[Minimum Swaps To Make Sequences Increasing][minimum-swaps-to-make-sequences-increasing]

{% highlight java %}
public int minSwap(int[] A, int[] B) {
    int s1 = 0, s2 = 1;  // same, swap
    for (int i = 1; i < A.length; i++) {
        int tmp = s1;
        if (A[i - 1] < A[i] && B[i - 1] < B[i]) {
            if (A[i - 1] < B[i] && B[i - 1] < A[i]) {
                s1 = Math.min(s1, s2);
                s2 = Math.min(tmp, s2) + 1;
            } else {
                s2++;
            }
        } else {
            s1 = s2;
            s2 = tmp + 1;
        }
    }
    return Math.min(s1, s2);
}
{% endhighlight %}

[Dice Roll Simulation][dice-roll-simulation]

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

[best-time-to-buy-and-sell-stock-with-cooldown]: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/
[best-time-to-buy-and-sell-stock-with-transaction-fee]: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/
[dice-roll-simulation]: https://leetcode.com/problems/dice-roll-simulation/
[minimum-swaps-to-make-sequences-increasing]: https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing/
