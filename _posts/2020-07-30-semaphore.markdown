---
layout: post
title:  "Semaphore"
tags: java
---
[Semaphore][semaphore]

Conceptually, a semaphore maintains a set of permits.
* [Semaphore(int)](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Semaphore.html#Semaphore-int-)
* [acquire()](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Semaphore.html#acquire--)
* [release()](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Semaphore.html#release--)

Semaphores are often used to restrict the number of threads than can access some (physical or logical) resource.

{% highlight java %}
class Pool {
   private static final int MAX_AVAILABLE = 100;
   private final Semaphore available = new Semaphore(MAX_AVAILABLE, true);

   public Object getItem() throws InterruptedException {
     available.acquire();
     return getNextAvailableItem();
   }

   public void putItem(Object x) {
     if (markAsUnused(x))
       available.release();
   }

   // Not a particularly efficient data structure; just for demo

   protected Object[] items = ... whatever kinds of items being managed
   protected boolean[] used = new boolean[MAX_AVAILABLE];

   protected synchronized Object getNextAvailableItem() {
     for (int i = 0; i < MAX_AVAILABLE; ++i) {
       if (!used[i]) {
          used[i] = true;
          return items[i];
       }
     }
     return null; // not reached
   }

   protected synchronized boolean markAsUnused(Object item) {
     for (int i = 0; i < MAX_AVAILABLE; ++i) {
       if (item == items[i]) {
          if (used[i]) {
            used[i] = false;
            return true;
          } else
            return false;
       }
     }
     return false;
   }
 }
{% endhighlight %}

Example:
[Print in order][print_in_order]

[semaphore]: https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Semaphore.html
[print_in_order]: https://leetcode.com/problems/print-in-order/
