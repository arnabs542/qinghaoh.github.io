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

[Find Permutation][find-permutation]

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

[Candy][candy]

{% highlight java %}
public int candy(int[] ratings) {
    // steps of continuous up and down respectively
    int up = 0, down = 0, peak = 0, count = 1;
    for (int i = 1; i < ratings.length; i++) {
        // each child gets at least one candy
        count++;

        if (ratings[i - 1] < ratings[i]) {
            peak = ++up;
            down = 0;
            count += up;
        } else if (ratings[i - 1] > ratings[i])  {
            up = 0;
            down++;
            // gives peak one more candy if down > peak
            count += down + (peak >= down ? -1 : 0);
        } else {
            peak = up = down = 0;
        }
    }
    return count;
}
{% endhighlight %}

For example, `[0, 1, 10, 9, 8, 7]`

```
i = 1, up = 1, down = 0, peak = 1, count = 3
i = 2, up = 2, down = 0, peak = 2, count = 6
i = 3, up = 0, down = 1, peak = 2, count = 7
i = 4, up = 0, down = 2, peak = 2, count = 9
i = 5, up = 0, down = 3, peak = 2, count = 13

```

[best-time-to-buy-and-sell-stock-ii]: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/
[candy]: https://leetcode.com/problems/candy/
[decrease-elements-to-make-array-zigzag]: https://leetcode.com/problems/decrease-elements-to-make-array-zigzag/
[find-permutation]: https://leetcode.com/problems/find-permutation/
