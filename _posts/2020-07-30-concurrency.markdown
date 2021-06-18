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

# Mutex

[Mutex/Lock](https://en.wikipedia.org/wiki/Lock_(computer_science))

* [void lock()](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/concurrent/locks/Lock.html#lock())
* [void unlock()](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/concurrent/locks/Lock.html#unlock())

[ReentrantLock](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/concurrent/locks/ReentrantLock.html)

A reentrant mutual exclusion Lock with the same basic behavior and semantics as the implicit monitor lock accessed using **synchronized** methods and statements, but with extended capabilities.

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

## Intrinsic Lock

* Enforces exclusive access to an object's state
* Establishes *happens-before* relationships that are essential to visibility.

Every object has an intrinsic lock associated with it.

A thread can acquire a lock that it already owns. Allowing a thread to acquire the same lock more than once enables *reentrant synchronization*. This describes a situation where synchronized code, directly or indirectly, invokes a method that also contains synchronized code, and both sets of code use the same lock.

* [Object wait()](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/lang/Object.html#wait()): Causes the current thread to wait until it is awakened, typically by being notified or interrupted.
* [Object notify()](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/lang/Object.html#notify()): Wakes up a single thread that is waiting on this object's monitor. If any threads are waiting on this object, one of them is chosen to be awakened. The choice is arbitrary and occurs at the discretion of the implementation.
* [Thread interrupt()](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/lang/Thread.html#interrupt()): Interrupts this thread. If this thread is blocked in an invocation of the wait(), join() or sleep(long), or sleep(long, int), then its interrupt status will be cleared and it will receive an InterruptedException.

![Thread Life Cycle](http://www.btechsmartclass.com/java/java_images/java-thread-life-cycle.png)

### Synchronized Methods

[Fizz Buzz Multithreaded][fizz-buzz-multithreaded]

{% highlight java %}
private int n, curr = 1;

public FizzBuzz(int n) {
    this.n = n;
}

// printFizz.run() outputs "fizz".
public synchronized void fizz(Runnable printFizz) throws InterruptedException {
    while (curr <= n) {
        if (curr % 3 != 0 || curr % 5 == 0) {
            wait();
            continue;
        }
        printFizz.run();
        curr++;
        notifyAll();
    }
}

// printBuzz.run() outputs "buzz".
public synchronized void buzz(Runnable printBuzz) throws InterruptedException {
    while (curr <= n) {
        if (curr % 5 != 0 || curr % 3 == 0) {
            wait();
            continue;
        }
        printBuzz.run();
        curr++;
        notifyAll();
    }
}

// printFizzBuzz.run() outputs "fizzbuzz".
public synchronized void fizzbuzz(Runnable printFizzBuzz) throws InterruptedException {
    while (curr <= n) {
        if (curr % 15 != 0) {
            wait();
            continue;
        }
        printFizzBuzz.run();
        curr++;
        notifyAll();
    }
}

// printNumber.accept(x) outputs "x", where x is an integer.
public synchronized void number(IntConsumer printNumber) throws InterruptedException {
    while (curr <= n) {
        if (curr % 3 == 0 || curr % 5 == 0) {
            wait();
            continue;
        }
        printNumber.accept(curr);
        curr++;
        notifyAll();
    }
}
{% endhighlight %}

### Synchronized Statements

[Traffic Light Controlled Intersection][traffic-light-controlled-intersection]

{% highlight java %}
class TrafficLight {
    private final Signal signal;

    public TrafficLight() {
        signal = new Signal();
    }

    public void carArrived(
        int carId,           // ID of the car
        int roadId,          // ID of the road the car travels on. Can be 1 (road A) or 2 (road B)
        int direction,       // Direction of the car
        Runnable turnGreen,  // Use turnGreen.run() to turn light to green on current road
        Runnable crossCar    // Use crossCar.run() to make car cross the intersection
    ) {
        // allows only one car to pass at a time
        synchronized(signal) {
            if (signal.greenRoad != roadId) {
                turnGreen.run();
                signal.greenRoad = roadId;
            }
            crossCar.run();
        }
    }

    class Signal {
        int greenRoad = 1;  // road ID that's green
    }
}
{% endhighlight %}

# Parallel Stream

[Web Crawler Multithreaded][web-crawler-multithreaded]

{% highlight java %}
public List<String> crawl(String startUrl, HtmlParser htmlParser) {
    int index = startUrl.indexOf('/', 7);  // skips http://
    String hostname = index < 0 ? startUrl : startUrl.substring(0, index);  // hostname with protocol

    Set<String> visited = ConcurrentHashMap.newKeySet();
    visited.add(startUrl);

    return crawl(startUrl, htmlParser, visited, hostname)
        .collect(Collectors.toList());
}

private Stream<String> crawl(String startUrl, HtmlParser parser, Set<String> visited, String hostname) {
    Stream<String> stream = parser.getUrls(startUrl)
        .parallelStream()
        .filter(u -> u.startsWith(hostname))
        .filter(u -> visited.add(u))
        .flatMap(u -> crawl(u, parser, visited, hostname));

    return Stream.concat(Stream.of(startUrl), stream);
}
{% endhighlight %}

[fizz-buzz-multithreaded]: https://leetcode.com/problems/fizz-buzz-multithreaded/
[print_in_order]: https://leetcode.com/problems/print-in-order/
[semaphore]: https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Semaphore.html
[the-dining-philosophers]: https://leetcode.com/problems/the-dining-philosophers/
[traffic-light-controlled-intersection]: https://leetcode.com/problems/traffic-light-controlled-intersection/
[web-crawler-multithreaded]: https://leetcode.com/problems/web-crawler-multithreaded/
