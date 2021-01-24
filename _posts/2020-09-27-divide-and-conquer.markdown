---
layout: post
title:  "Divide and Conquer"
tags: divide-and-conquer
---
[Merge K Sorted Lists][merge-k-sorted-lists]

{% highlight java %}
public ListNode mergeKLists(ListNode[] lists) {
    if (lists.length == 0) {
        return null;
    }

    int interval = 1;
    while (interval < lists.length) {
        for (int i = 0; i + interval < lists.length; i += interval * 2) {
            lists[i] = mergeTwoLists(lists[i], lists[i + interval]);            
        }
        interval *= 2;
    }

    return lists[0];
}

private ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode head = new ListNode(0), curr = head;
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            curr.next = l1;
            l1 = l1.next;
        } else {
            curr.next = l2;
            l2 = l2.next;
        }
        curr = curr.next;
    }
    curr.next = l1 == null ? l2 : l1;
    return head.next;
}
{% endhighlight %}

Another solution is to add all nodes to a min heap.

[Ugly Number II][ugly-number-ii]

{% highlight java %}
public int nthUglyNumber(int n) {
    int[] ugly = new int[n];
    ugly[0] = 1;

    int i2 = 0, i3 = 0, i5 = 0;
    for (int i = 1; i < n; i++) {
        ugly[i] = Math.min(Math.min(ugly[i2] * 2, ugly[i3] * 3), ugly[i5] * 5);

        if (ugly[i2] * 2 == ugly[i]) {
            i2++;
        }
        if (ugly[i3] * 3 == ugly[i]) {
            i3++;
        }   
        if (ugly[i5] * 5 == ugly[i]) {
            i5++;
        }
    }

    return ugly[n - 1];
}
{% endhighlight %}

[merge-k-sorted-lists]: https://leetcode.com/problems/merge-k-sorted-lists/
[ugly-number-ii]: https://leetcode.com/problems/ugly-number-ii/
