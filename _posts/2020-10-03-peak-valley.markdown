---
layout: post
title:  "Peak Valley"
tag: array
---
[Best Time to Buy and Sell Stock II][best-time-to-buy-and-sell-stock-ii]

```
[1,7,2,3,6,7,6,7]
```

![array](/assets/best_time_to_buy_and_sell_stock_2.png)

{% highlight java %}
public int maxProfit(int[] prices) {
    int profit = 0;
    for (int i = 1; i < prices.length; i++) {
        profit += Math.max(0, prices[i] - prices[i - 1]);
    }
    return profit;
}
{% endhighlight %}

[best-time-to-buy-and-sell-stock-ii]: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/