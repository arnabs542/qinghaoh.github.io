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

[design-phone-directory]: https://leetcode.com/problems/design-phone-directory/
[lru-cache]: https://leetcode.com/problems/lru-cache/
[max-stack]: https://leetcode.com/problems/max-stack/
