---
layout: post
title:  "Linked List"
tags: array
---
## Cycle Detection

[Floyd's Tortoise and Hare](https://en.wikipedia.org/wiki/Cycle_detection#Floyd's_Tortoise_and_Hare)

[Linked List Cycle II][linked-list-cycle-ii]

{% highlight java %}
public ListNode detectCycle(ListNode head) {
    ListNode tortoise = head, hare = head;

    // Find the intersection point of the two runners
    while (hare != null && hare.next != null) {
        tortoise = tortoise.next;
        hare = hare.next.next;

        if (tortoise == hare) {
            // Find the "entrance" to the cycle
            tortoise = head;
            while (tortoise != hare) {
                tortoise = tortoise.next;
                hare = hare.next;
            }
            return hare;
        }
    }

    return null;
}
{% endhighlight %}

This algorithm can be used to detect duplicate elements in an array, too.

[Find the Duplicate Number][find-the-duplicate-number]

{% highlight java %}
public int findDuplicate(int[] nums) {
    // Find the intersection point of the two runners
    int tortoise = nums[0], hare = nums[0];
    do {
        tortoise = nums[tortoise];
        hare = nums[nums[hare]];
    } while (tortoise != hare);

    // Find the "entrance" to the cycle
    tortoise = nums[0];
    while (tortoise != hare) {
        tortoise = nums[tortoise];
        hare = nums[hare];
    }

    return hare;
}
{% endhighlight %}

A hidden condition is `nums[0] != 0`, otherwise the tortoise and hare will stay at `0` forever.
 
[find-the-duplicate-number]: https://leetcode.com/problems/find-the-duplicate-number/
[linked-list-cycle-ii]: https://leetcode.com/problems/linked-list-cycle-ii/
