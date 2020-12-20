---
layout: post
title:  "Encoding/Decoding"
tag: string
---

# Chunked Transfer Encoding

[Chunked transfer encoding](https://en.wikipedia.org/wiki/Chunked_transfer_encoding): HTTPi/1.1

[Encode and Decode Strings][encode-and-decode-strings]

{% highlight java %}
private String toCount(String s) {
    int length = 4;  // 4 chunks
    char[] bytes = new char[length];
    for (int i = length - 1; i >= 0; i--) {
        bytes[length - 1 - i] = (char) (s.length() >> (i * 8) & 0xFF);
    }
    return new String(bytes);
}
{% endhighlight %}

[encode-and-decode-strings]: https://leetcode.com/problems/encode-and-decode-strings/
[paint-fence]: https://leetcode.com/problems/paint-fence/
