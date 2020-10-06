---
layout: post
title:  "State Machine"
---
[Best Time to Buy and Sell Stock with Cooldown][best-time-to-buy-and-sell-stock-with-cooldown]

{% highlight java %}
public int maxProfit(int[] prices) {
    int sold = 0, hold = Integer.MIN_VALUE, rest = 0;
    for (int price : prices) {
        int prevSold = sold;
        sold = hold + price;
        hold = Math.max(hold, rest - price);
        rest = Math.max(rest, prevSold);
    }
    return Math.max(sold, rest);
}
{% endhighlight %}

[best-time-to-buy-and-sell-stock-with-cooldown]: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/
