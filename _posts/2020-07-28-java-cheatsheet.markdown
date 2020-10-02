---
layout: post
title:  "Java Cheatsheet"
tags: java
---
# Arrays
* [public static \<T\> List\<T\> asList(T... a)](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#asList-T...-): fixed-size, mutable
* [public static \<T\> T\[\] copyOf(T\[\] original, int newLength)](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#copyOf-T:A-int-)
* [public static void fill(Objet\[\] a, Object val)](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#fill-java.lang.Object:A-java.lang.Object-)
* [static \<E\> List\<E\> of(E... elements)](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html#of%28E...%29): immutable
* [public static \<T\> void sort(T\[\] a, int fromIndex, int toIndex, Comparator\<? super T\> c)](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#sort-T:A-int-int-java.util.Comparator-): stable

# Collection
* [void clear()](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#clear--)
* [boolean contains(Object o)](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#contains-java.lang.Object-)

# Collections
* [public static boolean disjoint(Collection\<?\> c1, Collection\<?\> c2)](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#disjoint-java.util.Collection-java.util.Collection-)
* [public static final \<T\> List\<T\> emptyList()](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#emptyList--): immutable
* [public static \<T\> void fill(List\<? super T\> list, T obj)](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#fill-java.util.List-T-)
* [public static int frequency(Collection\<?\> c, Object o)](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#frequency-java.util.Collection-java.lang.Object-)
* [public static \<T\> T max(Collection\<? extends T\> coll, Comparator\<? super T\> comp)](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#max-java.util.Collection-java.util.Comparator-)
* [public static \<T\> List\<T\> nCopies(int n, T o)](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#nCopies-int-T-): immutable
* [public static void reverse(List\<?\> list)](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Collections.html#reverse(java.util.List))
* [public static \<T\> Comparator\<T\> reverseOrder()](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#reverseOrder--)

{% highlight java %}
Integer[] arr = {1, 2, 3};
  
// Sorts arr[] in descending order 
Arrays.sort(arr, Collections.reverseOrder()); 
{% endhighlight %}

* [public static \<T\> void sort(List\<T\> list, Comparator\<? super T\> c)](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#sort-java.util.List-java.util.Comparator-): stable
* [public static void swap(List\<?\> list, int i, int j)](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#swap-java.util.List-int-int-)

# Comparable
* [int compareTo(T o)](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html#compareTo-T-)

# Comparator
[Comparator](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)

Compare Strings by length in descending order, then by alphabetical order:
{% highlight java %}
Comparator.comparing(String::length)
    .reversed()
    .thenComparing(Comparator.<String>naturalOrder())
{% endhighlight %}

* [static \<T\> Comparator\<T\> comparingInt(ToIntFunction\<? super T\> keyExtractor)](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#comparingInt-java.util.function.ToIntFunction-)
* [static \<T extends Comparable\<? super T\>\> Comparator\<T\> naturalOrder()](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#naturalOrder--)
* [default Comparator\<T\> reversed()](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/Comparator.html#reversed())
* [static \<T extends Comparable\<? super T\>\> Comparator\<T\> reverseOrder()](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/Comparator.html#reverseOrder())

# Deque
[Deque](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Deque.html)
Deques can also be used as LIFO (Last-In-First-Out) stacks. This interface should be used in preference to the legacy `Stack` class. When a deque is used as a stack, elements are pushed and popped from the beginning of the deque.

{% highlight java %}
Deque<Integer> stack = new ArrayDeque<Integer>();
{% endhighlight %}

* [Iterator\<E\> descendingIterator()](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Deque.html#descendingIterator())

# Integer
* [MAX_VALUE](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html#MAX_VALUE): 2^31-1 = 2147483647
* [MIN_VALUE](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html#MIN_VALUE): -2^31 = -2147483648
* [public static int bitCount(int i)](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html#bitCount-int-)
* [public int intValue()](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html#intValue--)

{% highlight java %}
MAX_VALUE + 1 == MIN_VALUE
{% endhighlight %}

# IntStream
* [int sum()](https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html#sum--)

# Iterator
* [default void forEachRemaining(Consumer\<? super E\> action)](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Iterator.html#forEachRemaining(java.util.function.Consumer))

# LinkedList
* [LinkedList](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html)
Doubly-linked list implementation of the `List` and `Deque` interfaces. Implements all optional list operations, and permits all elements (including `null`).

# List
* [void add(int index, E element)](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#add-int-E-)
* [List\<E\> subList(int fromIndex, int toIndex)](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#subList-int-int-)

[\<T\> T\[\] toArray(T\[\] a)](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#toArray-T:A-)

Convert String List to String array:
{% highlight java %}
List<String> list = new ArrayList<String>();
list.add(0);
String[] array = list.toArray(new String[0])
{% endhighlight %}

# Map.Entry
* [static \<K,V extends Comparable\<? super V\>\> Comparator\<Map.Entry\<K,V\>\> comparingByValue()](https://docs.oracle.com/javase/8/docs/api/java/util/Map.Entry.html#comparingByValue--)

# Object
* [protected Object clone() throws CloneNotSupportedException](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#clone--)

# Set
* [boolean containsAll(Collection\<?\> c)](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#containsAll-java.util.Collection-)
* [boolean retainAll(Collection\<?\> c)](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#retainAll-java.util.Collection-)

# Stream
* [\<A\> A\[\] toArray(IntFunction\<A\[\]\> generator)](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#toArray-java.util.function.IntFunction-)

# String
* [public String(char\[\]  value, int offset, int count)](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#String-char:A-int-int-)
* [public boolean contains(CharSequence s)](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#contains-java.lang.CharSequence-)
* [public String repeat(int count)](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html#repeat(int))
* [public String replaceAll(String regex, String replacement)](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#replaceAll-java.lang.String-java.lang.String-)
* [public boolean startsWith(String prefix, int toffset)](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#startsWith-java.lang.String-int-)

# StringBuilder
* [public StringBuilder replace(int start, int end, String str)](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#replace-int-int-java.lang.String-)
* [public StringBuilder reverse()](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#reverse--)

# System
* [public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#arraycopy-java.lang.Object-int-java.lang.Object-int-int-)

# TreeSet
* [public NavigableSet\<E\> subSet(E fromElement, boolean fromInclusive, E toElement, boolean toInclusive)](https://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html#subSet-E-boolean-E-boolean-)
* [public SortedSet\<E\> subSet(E fromElement, E toElement)](https://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html#subSet-E-E-)
