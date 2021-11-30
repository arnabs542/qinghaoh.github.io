---
layout: post
title:  "Data Structure"
tag: data structure
usemathjax: true
---
[Min Stack][min-stack]

stack and minStack

[Max Stack][max-stack]

* Two stacks
* Double linked list + TreeMap<Node>

[LRU Cache][lru-cache]

* LinkedHashMap
* Double linked list + TreeMap<Node>

[Design A Leaderboard][design-a-leaderboard]

Map + TreeMap

[Implement Magic Dictionary][implement-magic-dictionary]

{% highlight java %}
// {length : words}
private Map<Integer, List<String>> map = new HashMap<>();
{% endhighlight %}

[All O`one Data Structure][all-oone-data-structure]

Doubly linked list (buckets)
+ Map (key -> count)
+ Map (count -> bucket)

[Dinner Plate Stacks][dinner-plate-stacks]

* List<Stack> + PriorityQueue to find the leftmost available stack
* Map<Integer, Stack> + Set of available stacks

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

[Find Servers That Handled Most Number of Requests][find-servers-that-handled-most-number-of-requests]

{% highlight java %}
TreeSet<Integer> availableServers = new TreeSet<>();
// {busy server, available time}
Queue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] == b[1] ? a[0] - b[0] : a[1] - b[1]);
{% endhighlight %}

[Sliding Window Median][sliding-window-median]

{% highlight java %}
// two TreeSets
// stores index
Comparator<Integer> comparator = (a, b) -> nums[a] == nums[b] ? a - b : Integer.compare(nums[a], nums[b]);
TreeSet<Integer> left = new TreeSet<>(comparator), right = new TreeSet<>(comparator);
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

[LFU Cache][lfu-cache]

{% highlight java %}
private HashMap<Integer, Node> keyMap = new HashMap<>();
private HashMap<Integer, DoubleLinkedList> freqMap = new HashMap<>();
private int capacity;
private int size = 0;
// gets least frequency in O(1)
private int minFreq = 0;

public LFUCache(int capacity) {
    this.capacity = capacity;

    freqMap.put(0, new DoubleLinkedList(0));
}

public int get(int key) {
    return keyMap.containsKey(key) ? updateFreq(keyMap.get(key)) : -1;
}

public void put(int key, int value) {
    if (capacity == 0) {
        return;
    }

    if (keyMap.containsKey(key)) {
        Node node = keyMap.get(key);
        node.value = value;
        updateFreq(node);
        return;
    }

    if (size >= capacity) {
        keyMap.remove(freqMap.get(minFreq).removeLast().key);
        size--;
    }

    Node node = new Node(key, value);
    freqMap.get(0).add(node);
    keyMap.put(key, node);
    minFreq = 0;
    size++;
}

private int updateFreq(Node node) {
    int freq = node.freq;
    freqMap.get(node.freq++).remove(node);
    freqMap.computeIfAbsent(node.freq, k -> new DoubleLinkedList(node.freq)).add(node);

    if (minFreq == freq && freqMap.get(minFreq).isEmpty()) {
        minFreq++;
    }
    return node.value;
}

class Node {
    private int key = 0;
    private int value = 0;
    private int freq = 0;
    private Node prev = null;
    private Node next = null;

    Node() {
    }

    Node(int key,int value) {
        this.key = key;
        this.value = value;
    }
}

class DoubleLinkedList {
    private int freq;
    private Node head = null;
    private Node tail = null;

    DoubleLinkedList(int freq) {
        this.freq = freq;
        head = new Node();
        tail = new Node();
        head.next = tail;
        tail.prev = head;
    }

    // inserts to the head of the list
    void add(Node node) {
        Node tmp = head.next;
        head.next = node;
        node.prev = head;
        node.next = tmp;
        tmp.prev = node;
    }

    void remove(Node node) {
        node.next.prev = node.prev;
        node.prev.next = node.next;
    }

    boolean isEmpty() {
        return head.next == tail;
    }

    // gets the least recently used node
    Node removeLast() {
        if (isEmpty()) {
            return null;
        }

        Node node = tail.prev;
        remove(node);
        return node;
    }
}
{% endhighlight %}

# Skip List

[Skip List](https://en.wikipedia.org/wiki/Skip_list): a probabilistic data structure that allows \\({\mathcal {O}}(\log n)\\) search complexity as well as \\({\mathcal {O}}(\log n)\\) insertion complexity within an ordered sequence of \\(n\\) elements.

[Design Skiplist][design-skiplist]

{% highlight java %}
class Skiplist {
    class Node {
        private int val;
        private Node next = null, down = null;

        public Node(int val) {
            this.val = val;
        }

        public Node(int val, Node next, Node down) {
            this.val = val;
            this.next = next;
            this.down = down;
        }
    }

    private Node head = new Node(-1);

    public Skiplist() {

    }

    public boolean search(int target) {
        Node curr = head;
        while (curr != null) {
            // searches to right on the same level
            while (curr.next != null && curr.next.val < target) {
                curr = curr.next;
            }

            // found the target
            if (curr.next != null && curr.next.val == target) {
                return true;
            }

            // goes down one level
            curr = curr.down;
        }
        return false;
    }

    public void add(int num) {
        Deque<Node> stack = new ArrayDeque<>();
        Node curr = head;
        while (curr != null) {
            // searches to right on the same level
            while (curr.next != null && curr.next.val < num) {
                curr = curr.next;
            }

            // pushes the right most node (< num) on the level to stack
            stack.push(curr);

            // goes down one level
            curr = curr.down;
        }

        // now we are at the bottom
        Node down = null;
        do {
            curr = stack.pop();
            curr.next = new Node(num, curr.next, down);
            down = curr.next;
        // if coin tails up, stops appending new node to the level
        } while(flipCoin() && !stack.isEmpty());

        // if coin heads up, creates a new list head
        if (flipCoin()) {
            head = new Node(-1, null, head);
        }
    }

    public boolean erase(int num) {
        Node curr = head;
        boolean found = false;
        while (curr != null) {
            // searches to right on the same level
            while (curr.next != null && curr.next.val < num) {
                curr = curr.next;
            }

            // the node could be on multiple levels
            if (curr.next != null && curr.next.val == num) {
                found = true;
                // removes the node
                curr.next = curr.next.next;
            }

            // goes one level down
            curr = curr.down;
        }
        return found;
    }

    private boolean flipCoin() {
        return Math.random() < 0.5;
    }
}
{% endhighlight %}

[all-oone-data-structure]: https://leetcode.com/problems/all-oone-data-structure/
[design-a-leaderboard]: https://leetcode.com/problems/design-a-leaderboard/
[design-a-stack-with-increment-operation]: https://leetcode.com/problems/design-a-stack-with-increment-operation/
[design-hashmap]: https://leetcode.com/problems/design-phone-hashmap/
[design-movie-rental-system]: https://leetcode.com/problems/design-movie-rental-system/
[design-phone-directory]: https://leetcode.com/problems/design-phone-directory/
[design-skiplist]: https://leetcode.com/problems/design-skiplist/
[dinner-plate-stacks]: https://leetcode.com/problems/dinner-plate-stacks/
[find-median-from-data-stream]: https://leetcode.com/problems/find-median-from-data-stream/
[find-servers-that-handled-most-number-of-requests]: https://leetcode.com/problems/find-servers-that-handled-most-number-of-requests/
[implement-magic-dictionary]: https://leetcode.com/problems/implement-magic-dictionary/
[lfu-cache]: https://leetcode.com/problems/lfu-cache/
[lru-cache]: https://leetcode.com/problems/lru-cache/
[max-stack]: https://leetcode.com/problems/max-stack/
[maximum-frequency-stack]: https://leetcode.com/problems/maximum-frequency-stack/
[min-stack]: https://leetcode.com/problems/min-stack/
[sliding-window-median]: https://leetcode.com/problems/sliding-window-median/
