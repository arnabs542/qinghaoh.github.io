---
layout: post
title:  "Java Cheatsheet"
tags: java
---
# Arrays
* [public static \<T\> T\[\] copyOf(T\[\] original, int newLength)](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#copyOf-T:A-int-)
* [public static void fill(Objet\[\] a, Object val)](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#fill-java.lang.Object:A-java.lang.Object-)
* [public static \<T\> void sort(T\[\] a, int fromIndex, int toIndex, Comparator\<? super T\> c)](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#sort-T:A-int-int-java.util.Comparator-)

# List
[List\<E\> subList(int fromIndex, int toIndex)](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#subList-int-int-)

[\<T\> T\[\] toArray(T\[\] a)](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#toArray-T:A-)

Convert String List to String array:
{% highlight java %}
List<String> list = new ArrayList<String>();
list.add(0);
String[] array = list.toArray(new String[0])
{% endhighlight %}

# Collections
* [public static final \<T\> List\<T\> emptyList()](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#emptyList--)

# Comparator
[Comparator](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)

Compare Strings by length in descending order, then by alphabetical order:
{% highlight java %}
Comparator.comparing(String::length)
    .reversed()
    .thenComparing(Comparator.<String>naturalOrder())
{% endhighlight %}

# Object
* [protected Object clone() throws CloneNotSupportedException](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#clone--)

# String
* [public String(char\[\]  value, int offset, int count)](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#String-char:A-int-int-)
* [public boolean contains(CharSequence s)](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#contains-java.lang.CharSequence-)
* [public String repeat(int count)](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html#repeat(int))
* [public boolean startsWith(String prefix, int toffset)](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#startsWith-java.lang.String-int-)

# StringBuilder
* [public StringBuilder replace(int start, int end, String str)](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#replace-int-int-java.lang.String-)
* [public StringBuilder reverse()](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#reverse--)
