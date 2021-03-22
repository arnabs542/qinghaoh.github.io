---
layout: post
title:  "Encoding/Decoding"
tag: string
---

# Chunked Transfer Encoding

[Chunked transfer encoding](https://en.wikipedia.org/wiki/Chunked_transfer_encoding): HTTP/1.1

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

[Serialize and Deserialize Binary Tree][serialize-and-deserialize-binary-tree]

{% highlight java %}
// preorder
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        serialize(root, sb)
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        return deserialize(new LinkedList<>(Arrays.asList(data.split(","))));
    }
    
    private void serialize(TreeNode root, StringBuilder sb) {
        if (root == null) {
            sb.append("#").append(",");
            return;
        }
        
        sb.append(root.val).append(",");
        serialize(root.left, sb);
        serialize(root.right, sb);
    }
    
    private TreeNode deserialize(Queue<String> q) {
        String val = q.poll();
        if ("#".equals(val)) {
            return null;
        }
        
        TreeNode root = new TreeNode(Integer.valueOf(val));
        root.left = deserialize(q);
        root.right = deserialize(q);
        return root;
    }
}
{% endhighlight %}

[Serialize and Deserialize N-ary Tree][serialize-and-deserialize-n-ary-tree]

{% highlight java %}
class Codec {
    // Encodes a tree to a single string.
    public String serialize(Node root) {
        StringBuilder sb = new StringBuilder();
        serialize(root, sb);
        return sb.toString();
    }
	
    // Decodes your encoded data to tree.
    public Node deserialize(String data) {
        return deserialize(new LinkedList<>(Arrays.asList(data.split(","))));
    }
    
    private void serialize(Node root, StringBuilder sb) {
        if (root == null) {
            sb.append("#").append(",");
            return;
        }
        
        sb.append(root.val).append(",");
        if (root.children.isEmpty()) {
            sb.append("#").append(",");
        } else {
            sb.append(root.children.size()).append(",");
            for (Node child : root.children) {
                serialize(child, sb);
            }
        }
    }
    
    private Node deserialize(Queue<String> q) {
        String val = q.poll();
        if ("#".equals(val)) {
            return null;
        }
        
        // count of children
        String count = q.poll();

        Node root = new Node(Integer.valueOf(val), new ArrayList<>());
        if (!count.equals("#")) {
            for (int i = 0; i < Integer.valueOf(count); i++) {
                root.children.add(deserialize(q));
            }
        }
        return root;
    }
}
{% endhighlight %}

[Encode N-ary Tree to Binary Tree][encode-n-ary-tree-to-binary-tree]

[Convert a m-ary tree to binary tree](https://en.wikipedia.org/wiki/M-ary_tree#Convert_a_m-ary_tree_to_binary_tree)

[Construct Binary Tree from String][construct-binary-tree-from-string]

{% highlight java %}
public TreeNode str2tree(String s) {
    Deque<TreeNode> st = new ArrayDeque<>();
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (c == ')') {
            st.pop();
        } else if (c != '(') {
            int j = i;
            while (i + 1 < s.length() && Character.isDigit(s.charAt(i + 1))) {
                i++;
            }

            TreeNode node = new TreeNode(Integer.valueOf(s.substring(j, i + 1)));
            if (!st.isEmpty()) {
                TreeNode parent = st.peek();
                if (parent.left != null) {
                    parent.right = node;
                } else {
                    parent.left = node;
                }
            }
            st.push(node);
        }
    }
    return st.isEmpty() ? null : st.peek();
}
{% endhighlight %}

[Encode and Decode TinyURL][encode-and-decode-tinyurl]

{% highlight java %}
public class Codec {
    // 62 chars
    private static final String ALPHABET = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    private static final String PATH = "http://tinyurl.com/";

    private Map<String, String> map = new HashMap<>();
    private Random rand = new Random();
    private String key = getRand();

    private String getRand() {
        StringBuilder sb = new StringBuilder();
        // short url has 6 random chars
        for (int i = 0; i < 6; i++) {
            sb.append(ALPHABET.charAt(rand.nextInt(ALPHABET.length())));
        }
        return sb.toString();
    }

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        while (map.containsKey(key)) {
            key = getRand();
        }
        map.put(key, longUrl);
        return PATH + key;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        return map.get(shortUrl.replace(PATH, ""));
    }
}
{% endhighlight %}

[construct-binary-tree-from-string]: https://leetcode.com/problems/construct-binary-tree-from-string/
[encode-and-decode-strings]: https://leetcode.com/problems/encode-and-decode-strings/
[encode-and-decode-tinyurl]: https://leetcode.com/problems/encode-and-decode-tinyurl/
[encode-n-ary-tree-to-binary-tree]: https://leetcode.com/problems/encode-n-ary-tree-to-binary-tree/
[serialize-and-deserialize-binary-tree]: https://leetcode.com/problems/serialize-and-deserialize-binary-tree/
[serialize-and-deserialize-n-ary-tree]: https://leetcode.com/problems/serialize-and-deserialize-n-ary-tree/
