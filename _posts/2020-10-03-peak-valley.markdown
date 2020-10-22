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

[Decrease Elements To Make Array Zigzag][decrease-elements-to-make-array-zigzag]

{% highlight java %}
private int MAX = 1001;

public int movesToMakeZigzag(int[] nums) {       
    int[] result = new int[2];
    int left = 0, right = 0;
    for (int i = 0; i < nums.length; ++i) {
        left = i > 0 ? nums[i - 1] : MAX;
        right = i < nums.length - 1 ? nums[i + 1] : MAX;

        // decreases nums[odd] or nums[even]
        result[i % 2] += Math.max(0, nums[i] - Math.min(left, right) + 1);
    }
    return Math.min(result[0], result[1]);
}
{% endhighlight %}

[best-time-to-buy-and-sell-stock-ii]: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/
[decrease-elements-to-make-array-zigzag]: https://leetcode.com/problems/decrease-elements-to-make-array-zigzag/
