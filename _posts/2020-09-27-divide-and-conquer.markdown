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

[Super Ugly Number][super-ugly-number]

{% highlight java %}
public int nthSuperUglyNumber(int n, int[] primes) {
    int[] ugly = new int[n];
    ugly[0] = 1;

    // prime factorization
    int m = primes.length;
    int[] power = new int[m];

    for (int i = 1; i < n; i++) {
        ugly[i] = Integer.MAX_VALUE;
        for (int j = 0; j < m; j++) {
            if (ugly[power[j]] * primes[j] == ugly[i - 1]) {
                power[j]++;
            }
            ugly[i] = Math.min(ugly[i], ugly[power[j]] * primes[j]);
        }
    }

    return ugly[n - 1];
}
{% endhighlight %}

[Longest Nice Substring][longest-nice-substring]

{% highlight java %}
public String longestNiceSubstring(String s) {
    if (s.length() < 2) {
        return "";
    }

    Set<Character> set = new HashSet<>();
    for (char c: s.toCharArray()) {
        set.add(c);
    }
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (set.contains(Character.toUpperCase(c)) && set.contains(Character.toLowerCase(c))) {
            continue;
        }
        String s1 = longestNiceSubstring(s.substring(0, i));
        String s2 = longestNiceSubstring(s.substring(i + 1));
        return s1.length() >= s2.length() ? s1 : s2;
    }
    return s; 
}
{% endhighlight %}

[Beautiful Array][beautiful-array]

{% highlight java %}
public int[] beautifulArray(int N) {
    List<Integer> list = new ArrayList<>();
    list.add(1);

    // properties:
    // if A is a beautiful array, then
    //  * A + c is a beautiful array
    //  * c * A is a beautiful array
    //  * deletion: A is still a beautiful array after deleting some elements in it
    while (list.size() < N) {
        List<Integer> tmp = new ArrayList<>();
        // divide and conquer
        // divides the numbers into two parts: odd + even, because odd + even = odd != 2 * x
        for (int i : list) {
            if (i * 2 - 1 <= N) {
                tmp.add(i * 2 - 1);
            }
        }
        for (int i : list) {
            if (i * 2 <= N) {
                tmp.add(i * 2);
            }
        }
        list = tmp;
    }
    return list.stream().mapToInt(i -> i).toArray();
}
{% endhighlight %}

It's easy to come up with a recursive version.

[Bit reversal permutation](https://en.wikipedia.org/wiki/Bit-reversal_permutation)

Another solution is bit reverse (br) sort. It can be proven that if `i + k = j * 2`, then `br(j)` is either less than or greater than both `br(i)` and `br(k)`. Thus the algorithm is to sort the numbers based on their bit reverse. Credit to @Aging.

[beautiful-array]: https://leetcode.com/problems/beautiful-array/
[longest-nice-substring]: https://leetcode.com/problems/longest-nice-substring/
[merge-k-sorted-lists]: https://leetcode.com/problems/merge-k-sorted-lists/
[super-ugly-number]: https://leetcode.com/problems/super-ugly-number/
