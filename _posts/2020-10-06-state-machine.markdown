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

[best-time-to-buy-and-sell-stock-with-cooldown]: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/
[best-time-to-buy-and-sell-stock-with-transaction-fee]: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/
