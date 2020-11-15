---
layout: post
title:  "Concurrency"
tags: concurrency
---
# Semaphore

[Semaphore][semaphore]

Conceptually, a semaphore maintains a set of permits.
* [Semaphore(int)](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Semaphore.html#Semaphore-int-): a bowl of marbles
* [acquire()](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Semaphore.html#acquire--): takes one marble from the bowl; waits if there are none
* [release()](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Semaphore.html#release--): adds one marble to the bowl

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

[Print in order][print_in_order]

{% highlight java %}
private Semaphore run2, run3;

public Foo() {
    run2 = new Semaphore(0);
    run3 = new Semaphore(0);
}

public void first(Runnable printFirst) throws InterruptedException {

    // printFirst.run() outputs "first". Do not change or remove this line.
    printFirst.run();

    run2.release();
}

public void second(Runnable printSecond) throws InterruptedException {
    run2.acquire();

    // printSecond.run() outputs "second". Do not change or remove this line.
    printSecond.run();

    run3.release();
}

public void third(Runnable printThird) throws InterruptedException {
    run3.acquire();

    // printThird.run() outputs "third". Do not change or remove this line.
    printThird.run();
}
{% endhighlight %}

# Mutex

[Mutex/Lock](https://en.wikipedia.org/wiki/Lock_(computer_science))

* [void lock()](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/concurrent/locks/Lock.html#lock())
* [void unlock()](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/concurrent/locks/Lock.html#unlock())

[ReentrantLock](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/concurrent/locks/ReentrantLock.html)

A reentrant mutual exclusion Lock with the same basic behavior and semantics as the implicit monitor lock accessed using synchronized methods and statements, but with extended capabilities.

In computing, a computer program or subroutine is called reentrant if multiple invocations can safely run concurrently on a single processor system, where a reentrant procedure can be interrupted in the middle of its execution and then safely be called again ("re-entered") before its previous invocations complete execution.

{% highlight java %}
Lock l = ...;
l.lock();
try {
    // access the resource protected by this lock
} finally {
    l.unlock();
}
{% endhighlight %}

[The Dining Philosophers][the-dining-philosophers]

[Dining philosophers problem](https://en.wikipedia.org/wiki/Dining_philosophers_problem)

{% highlight java %}
private int NUM = 5;
private Lock forks[] = new Lock[NUM];
// the number of philosophers who can take action is at most (NUM_FORKS - 1)
private Semaphore semaphore = new Semaphore(NUM - 1);

public DiningPhilosophers() {
    Arrays.fill(forks, new ReentrantLock());
}

void pickFork(int id, Runnable pick) {
    forks[id].lock();
    pick.run();
}

void putFork(int id, Runnable put) {
    put.run();
    forks[id].unlock();
}

// call the run() method of any runnable to execute its code
public void wantsToEat(int philosopher,
                       Runnable pickLeftFork,
                       Runnable pickRightFork,
                       Runnable eat,
                       Runnable putLeftFork,
                       Runnable putRightFork) throws InterruptedException {
    int left = philosopher, right = (philosopher + 4) % 5;

    semaphore.acquire();

    pickFork(left, pickLeftFork);
    pickFork(right, pickRightFork);

    eat.run();

    putFork(right, putRightFork);
    putFork(left, putLeftFork);

    semaphore.release();
}
{% endhighlight %}

[print_in_order]: https://leetcode.com/problems/print-in-order/
[semaphore]: https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Semaphore.html
[the-dining-philosophers]: https://leetcode.com/problems/the-dining-philosophers/
