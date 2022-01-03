---
layout: post
title:  "Linked List"
tags: array
---
# Cycle Detection

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
 
[Add Two Numbers II][add-two-numbers-ii]

{% highlight java %}
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    int s1 = size(l1), s2 = size(l2);
    return s1 > s2 ? addTwoNumbers(l1, l2, s1, s2) : addTwoNumbers(l2, l1, s2, s1);
}

private ListNode addTwoNumbers(ListNode l1, ListNode l2, int s1, int s2) {
    ListNode head = null, curr = null;
    while (l1 != null) {
        int v1 = l1.val, v2 = s1 == s2 ? l2.val : 0;
        l1 = l1.next;
        if (s1 == s2) {
            l2 = l2.next;
        } else {
            s1--;
        }

        // creates the result list in reverse order
        curr = new ListNode(v1 + v2);
        curr.next = head;
        head = curr;
    }

    // normalization
    head = null;
    int carry = 0;
    while (curr != null) {
        curr.val += carry;
        carry = curr.val > 9 ? 1 : 0;
        curr.val %= 10;

        // reverses the result list
        ListNode tmp = curr.next;
        curr.next = head;
        head = curr;
        curr = tmp;
    }

    // adds a new head if carry exists
    if (carry > 0) {
        curr = new ListNode(1);
        curr.next = head;
        head = curr;
    }
    return head;
}

private int size(ListNode l) {
    int s = 0;
    while (l != null) {
        l = l.next;
        s++;
    }
    return s;
}
{% endhighlight %}

# Reverse

[Palindrome Linked List][palindrome-linked-list]

{% highlight java %}
public boolean isPalindrome(ListNode head) {
    ListNode fast = head, slow = head;
    while (fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;
    }

    // if the number of nodes is even, fast == null
    // else (odd) fast != null
    if (fast != null) {
        slow = slow.next;
    }

    // slow is the head of the second half
    // if the number of nodes is odd, the second half is shorter
    slow = reverse(slow);
    fast = head;

    while (slow != null) {
        if (fast.val != slow.val) {
            return false;
        }
        fast = fast.next;
        slow = slow.next;
    }
    return true;
}

private ListNode reverse(ListNode head) {
    ListNode prev = null;
    while (head != null) {
        ListNode next = head.next;
        head.next = prev;
        prev = head;
        head = next;
    }
    return prev;
}
{% endhighlight %}

# Clone

[Copy List with Random Pointer][copy-list-with-random-pointer]

{% highlight java %}
public Node copyRandomList(Node head) {
    Node curr = head, next = null;

    // links each copy node to its original node
    while (curr != null) {
        next = curr.next;
        Node copy = new Node(curr.val);
        curr.next = copy;
        copy.next = next;
        curr = next;
    }

    // assigns random pointers for the copy nodes
    curr = head;
    while (curr != null) {
        if (curr.random != null) {
            curr.next.random = curr.random.next;
        }
        curr = curr.next.next;
    }

    curr = head;
    Node dummyCopyHead = new Node(0);
    Node copyCurr = dummyCopyHead, copyNext = null;

    while (curr != null) {
        // extracts the copy list
        copyNext = curr.next;
        copyCurr.next = copyNext;
        copyCurr = copyNext;

        // restores the original list
        next = curr.next.next;
        curr.next = next;
        curr = next;
    }

    return dummyCopyHead.next;
}
{% endhighlight %}

# Tree

[Populating Next Right Pointers in Each Node][populating-next-right-pointers-in-each-node]

{% highlight java %}
public Node connect(Node root) {
    if (root == null) {
        return root;
    }

    Node leftmost = root;
    while (leftmost.left != null) {
        // starts from leftmost node in each level
        Node head = leftmost;

        while (head != null) {
            // inner connection
            head.left.next = head.right;

            // inter connection
            if (head.next != null) {
                head.right.next = head.next.left;
            }

            head = head.next;
        }

        // moves to the next level
        leftmost = leftmost.left;
    }
    return root;
}
{% endhighlight %}

[add-two-numbers-ii]: https://leetcode.com/problems/add-two-numbers-ii/
[copy-list-with-random-pointer]: https://leetcode.com/problems/copy-list-with-random-pointer/
[find-the-duplicate-number]: https://leetcode.com/problems/find-the-duplicate-number/
[linked-list-cycle-ii]: https://leetcode.com/problems/linked-list-cycle-ii/
[palindrome-linked-list]: https://leetcode.com/problems/palindrome-linked-list/
[populating-next-right-pointers-in-each-node]: https://leetcode.com/problems/populating-next-right-pointers-in-each-node/
