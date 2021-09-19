---
layout: post
title:  "Data Structure"
tag: data structure
---

[Max Stack][max-stack]

* Two stacks
* Double linked list + TreeMap<Node>

[LRU Cache][lru-cache]

* LinkedHashMap
* Double linked list + TreeMap<Node>

[Design A Leaderboard][design-a-leaderboard]

* Map
* TreeMap

[All O`one Data Structure][all-oone-data-structure]

* Doubly linked list (buckets)
* Map (key -> count)
* Map (count -> bucket)

[Find Median from Data Stream][find-median-from-data-stream]

{% highlight java %}
// two heaps
if (left.size() == right.size()) {
    right.offer(num);
    left.offer(right.poll());
} else {
    left.offer(num);
    right.offer(left.poll());
}
{% endhighlight %}

[Design Phone Directory][design-phone-directory]

{% highlight java %}
class PhoneDirectory {
    private int max;
    private Set<Integer> used;
    private LinkedList<Integer> released;

    /** Initialize your data structure here
        @param maxNumbers - The maximum numbers that can be stored in the phone directory. */
    public PhoneDirectory(int maxNumbers) {
        this.max = maxNumbers;
        this.used = new HashSet<>();
        this.released = new LinkedList<>();
    }
    
    /** Provide a number which is not assigned to anyone.
        @return - Return an available number. Return -1 if none is available. */
    public int get() {
        if (used.size() == max) {
            return -1;
        }
        
        // always gets released number first
        // if the release pool is empty, the used pool must contain consective numbers [0, size)
        int number = released.isEmpty() ? used.size() : released.remove();
        used.add(number);
        return number;
    }
    
    /** Check if a number is available or not. */
    public boolean check(int number) {
        return !used.contains(number);
    }
    
    /** Recycle or release a number. */
    public void release(int number) {
        if (used.remove(number)) {
            released.add(number);
        }
    }
}
{% endhighlight %}

[Maximum Frequency Stack][maximum-frequency-stack]

{% highlight java %}
class FreqStack {
    Map<Integer, Integer> freq = new HashMap<>();
    Map<Integer, Deque<Integer>> map = new HashMap<>();  // f : stack
    int maxfreq = 0;

    public FreqStack() {

    }

    public void push(int x) {
        int f = freq.getOrDefault(x, 0) + 1;
        freq.put(x, f);
        maxfreq = Math.max(maxfreq, f);
        map.computeIfAbsent(f, k -> new ArrayDeque<Integer>()).push(x);
    }

    public int pop() {
        int x = map.get(maxfreq).pop();
        freq.put(x, maxfreq - 1);
        if (map.get(maxfreq).isEmpty()) {
            maxfreq--;
        }
        return x;
    }
}
{% endhighlight %}

[Design HashMap][design-hashmap]

{% highlight java %}
private static final int MAX_OPS = 10000;
private static final double LOAD_FACTOR = 0.75;
private List<List<int[]>> map = new ArrayList<>();
private int size = 0;

/** Initialize your data structure here. */
public MyHashMap() {
    this.size = (int)(MAX_OPS / LOAD_FACTOR);
    for (int i = 0; i < size; i++) {
        map.add(new ArrayList<>());
    }
}

/** value will always be non-negative. */
public void put(int key, int value) {
    int index = hashCode(key);
    for (int[] node : map.get(index)) {
        if (node[0] == key) {
            node[1] = value;
            return;
        }
    }
    map.get(index).add(new int[]{key, value});
}

/** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
public int get(int key) {
    int index = hashCode(key);
    for (int[] node : map.get(index)) {
        if (node[0] == key) {
            return node[1];
        }
    }
    return -1;
}

/** Removes the mapping of the specified value key if this map contains a mapping for the key */
public void remove(int key) {
    int index = hashCode(key);
    for (int[] node : map.get(index)) {
        if (node[0] == key) {
            map.get(index).remove(node);
            return;
        }
    }
}

private int hashCode(int key) {
    return Integer.hashCode(key) % size;
}
{% endhighlight %}

[Design a Stack With Increment Operation][design-a-stack-with-increment-operation]

{% highlight java %}
private Deque<Integer> stack = new ArrayDeque<>();
private int[] inc;
private int maxSize = 0;

public CustomStack(int maxSize) {
    this.maxSize = maxSize;
    this.inc = new int[maxSize];
}

public void push(int x) {
    if (stack.size() < maxSize) {
        stack.push(x);
    }
}

public int pop() {
    int i = stack.size() - 1;
    if (i < 0) {
        return -1;
    }

    if (i > 0) {
        inc[i - 1] += inc[i];
    }

    int num = stack.pop() + inc[i];
    inc[i] = 0;
    return num;
}

// lazy increment
public void increment(int k, int val) {
    // 0-indexed inc
    int i = Math.min(k, stack.size()) - 1;
    if (i >= 0) {
        inc[i] += val;
    }
}
{% endhighlight %}

[Design Movie Rental System][design-movie-rental-system]

Map/Set of array, with self-define comparator:

{% highlight java %}
class MovieRentingSystem {
    // {price, shop, movie}
    private static final Comparator<int[]> CMP = (a, b) -> a[0] == b[0] ? (a[1] == b[1] ? a[2] - b[2] : a[1] - b[1]) : a[0] - b[0];
    // movie : {price, shop, movie}
    private Map<Integer, Set<int[]>> unrented = new HashMap<>();
    // {price, shop, movie}
    private Set<int[]> rented = new TreeSet<>(CMP);
    // movie : {shop : price}
    private Map<Integer, Map<Integer, Integer>> prices = new HashMap<>();

    public MovieRentingSystem(int n, int[][] entries) {
        for (int[] e : entries) {
            prices.computeIfAbsent(e[0], k -> new HashMap<>()).put(e[1], e[2]);
            unrented.computeIfAbsent(e[1], k -> new TreeSet<>(CMP)).add(new int[]{e[2], e[0], e[1]});
        }
    }

    public List<Integer> search(int movie) {
        return unrented.getOrDefault(movie, Collections.emptySet()).stream()
            .limit(5)
            .map(e -> e[1])
            .collect(Collectors.toList());
    }

    public void rent(int shop, int movie) {
        int[] e = {prices.get(shop).get(movie), shop, movie};
        unrented.get(movie).remove(e);
        rented.add(e);
    }

    public void drop(int shop, int movie) {
        int[] e = {prices.get(shop).get(movie), shop, movie};
        unrented.get(movie).add(e);
        rented.remove(e);
    }

    public List<List<Integer>> report() {
        return rented.stream()
            .limit(5)
            .map(e -> List.of(e[1], e[2]))
            .collect(Collectors.toList());
    }
}
{% endhighlight %}

[all-oone-data-structure]: https://leetcode.com/problems/all-oone-data-structure/
[design-a-leaderboard]: https://leetcode.com/problems/design-a-leaderboard/
[design-a-stack-with-increment-operation]: https://leetcode.com/problems/design-a-stack-with-increment-operation/
[design-hashmap]: https://leetcode.com/problems/design-phone-hashmap/
[design-movie-rental-system]: https://leetcode.com/problems/design-movie-rental-system/
[design-phone-directory]: https://leetcode.com/problems/design-phone-directory/
[find-median-from-data-stream]: https://leetcode.com/problems/find-median-from-data-stream/
[lru-cache]: https://leetcode.com/problems/lru-cache/
[max-stack]: https://leetcode.com/problems/max-stack/
[maximum-frequency-stack]: https://leetcode.com/problems/maximum-frequency-stack/
