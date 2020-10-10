---
layout: post
title:  "State Machine"
---
[Best Time to Buy and Sell Stock with Cooldown][best-time-to-buy-and-sell-stock-with-cooldown]

![state machine](/assets/best_time_to_buy_and_sell_stock_with-cooldown.png)

{% highlight java %}
public int maxProfit(int[] prices) {
    if (prices.length == 0) {
        return 0;
    }

    int[] s0 = new int[prices.length];  // cash, not immediate after selling
    int[] s1 = new int[prices.length];  // hold
    int[] s2 = new int[prices.length];  // cash, immediate after selling

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

Reduce to 0D:

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
