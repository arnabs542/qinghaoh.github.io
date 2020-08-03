---
layout: post
title:  "Java Cheatsheet"
tags: java
---
# Array
[public static \<T\> void sort(T\[\] a, int fromIndex, int toIndex, Comparator\<? super T\> c)](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#sort-T:A-int-int-java.util.Comparator-)

# ArrayList
[public \<T\> T\[\] toArray(T\[\] a)](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#toArray-T:A-)

Convert String ArrayList to String array:
{% highlight java %}
List<String> list = new ArrayList<String>();
list.add(0);
String[] array = list.toArray(new String[0])
{% endhighlight %}

# Comparator
[Comparator](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)

Compare Strings by length in descending order, then by alphabetical order:
{% highlight java %}
Comparator.comparing(String::length)
    .reversed()
    .thenComparing(Comparator.<String>naturalOrder())
{% endhighlight %}

# String
* [public boolean contains(CharSequence s)](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#contains-java.lang.CharSequence-)
* [public String repeat(int count)](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html#repeat(int))

# StringBuilder
* [public StringBuilder replace(int start, int end, String str)](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#replace-int-int-java.lang.String-)
* [public StringBuilder reverse()](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#reverse--)
