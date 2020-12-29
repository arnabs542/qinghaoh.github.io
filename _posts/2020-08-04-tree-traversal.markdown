---
layout: post
title:  "Tree Traversal"
tags: tree
---
# Traversal
## Preorder
[Binary Tree Preorder Traveral][binary-tree-preorder-traversal]

### Recursion
{% highlight java %}
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<Integer>();
    helper(root, list);
    return list;
}

private void helper(TreeNode root, List<Integer> list) {
    if (root != null) {
        list.add(root.val);  // preorder
        helper(root.left, list);
        helper(root.right, list);
    }
}
{% endhighlight %}

### Stack
{% highlight java %}
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<>();
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode node = root;
    while (node != null) {
        list.add(node.val);  // preorder
        if (node.right != null) {
            stack.push(node.right);
        }             
        node = node.left;
        if (node == null && !stack.isEmpty()) {
            node = stack.pop();
        }
    }
    return list;
}
{% endhighlight %}

### Morris

## Inorder
[Binary Tree Inorder Traveral][binary-tree-inorder-traversal]

### Recursion
{% highlight java %}
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<>();
    helper(root, list);
    return list;
}

private void helper(TreeNode root, List<Integer> list) {
    if (root != null) {
        helper(root.left, list);
        list.add(root.val);  // inorder
        helper(root.right, list);
    }
}
{% endhighlight %}

### Stack
{% highlight java %}
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<>();
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode node = root;
    while (node != null || !stack.isEmpty()) {
        if (node != null) {
            stack.push(node);
            node = node.left;
        } else {
            node = stack.pop();
            list.add(node.val);  // inorder
            node = node.right;
        }
    }
    return list;  
}
{% endhighlight %}

## Postorder
[Binary Tree Postorder Traveral][binary-tree-postorder-traversal]

### Recursion
{% highlight java %}
public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<Integer>();
    traverse(root, list);
    return list;
}

private void traverse(TreeNode root, List<Integer> list) {
    if (root != null) {
        traverse(root.left, list);
        traverse(root.right, list);
        list.add(root.val);
    }
}
{% endhighlight %}

### Stack
{% highlight java %}
public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<>();
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode node = root;
    while (node != null) {
        list.add(node.val);
        if (node.left != null) {
            stack.push(node.left);
        }
        node = node.right;  // right -> left
        if (node == null && !stack.isEmpty()) {
            node = stack.pop();
        }
    }
    Collections.reverse(list);  // reverse
    return list;
}
{% endhighlight %}

## Depth First Search

[Find Largest Value in Each Tree Row][find-largest-value-in-each-tree-row]

{% highlight java %}
public List<Integer> largestValues(TreeNode root) {
    List<Integer> list = new ArrayList<>();
    if (root == null) {
        return list;
    }

    dps(root, list, 0);
    return list;
}

private void dps(TreeNode node, List<Integer> list, int depth) {
    if (node == null) {
        return;
    }

    if (depth == list.size()) {
        list.add(node.val);
    } else {
        list.set(depth, Math.max(list.get(depth), node.val));
    }

    dps(node.left, list, depth + 1);
    dps(node.right, list, depth + 1);
}
{% endhighlight %}

# Construction

[Construct Binary Tree from Preorder and Inorder Traversal][construct-binary-tree-from-preorder-and-inorder-traversal]

The key is to find the root in inorder. We can iterate to find it, or keep track of it in a map. Or optimally:

{% highlight java %}
private int pre = 0, in = 0;

public TreeNode buildTree(int[] preorder, int[] inorder) {
    // min int is a virtual parent of root
    return build(preorder, inorder, Integer.MIN_VALUE);
}

private TreeNode build(int[] preorder, int[] inorder, int prevRoot) {
    if (pre >= preorder.length) {
        return null;
    }

    // stops at previous root in inorder
    if (inorder[in] == prevRoot) {
        in++;
        return null;
    }

    TreeNode node = new TreeNode(preorder[pre++]);
    node.left = build(preorder, inorder, node.val);
    node.right = build(preorder, inorder, prevRoot);
    return node;
}
{% endhighlight %}

For example:

```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```
```
pre	in	prevRoot
0	0	-2147483648
1	0	3
2	0	9
2	1	3
2	2	-2147483648
3	2	20
4	2	15
4	3	20
4	4	-2147483648
5	4	7
5	4	-2147483648
```

[Construct Binary Tree from Inorder and Postorder Traversal][construct-binary-tree-from-inorder-and-postorder-traversal]

{% highlight java %}
private int in, post;

public TreeNode buildTree(int[] inorder, int[] postorder) {
    this.in = inorder.length - 1;
    this.post = postorder.length - 1;

    // min int is a virtual parent of root
    return build(inorder, postorder, Integer.MIN_VALUE);
}

private TreeNode build(int[] inorder, int[] postorder, int prevRoot) {
    if (post < 0) {
        return null;
    }

    // stops at previous root in inorder
    if (inorder[in] == prevRoot) {
        in--;
        return null;
    }

    TreeNode node = new TreeNode(postorder[post--]);
    node.right = build(inorder, postorder, node.val);
    node.left = build(inorder, postorder, prevRoot);
    return node;
}
{% endhighlight %}

[Construct Binary Tree from Preorder and Postorder Traversal][construct-binary-tree-from-preorder-and-postorder-traversal]

{% highlight java %}
private int preIndex = 0, postIndex = 0;

public TreeNode constructFromPrePost(int[] pre, int[] post) {
    TreeNode root = new TreeNode(pre[preIndex++]);

    // if root.val == post[postIndex]
    // the entire (sub)tree at root is constructed
    if (root.val != post[postIndex]) {
        root.left = constructFromPrePost(pre, post);
    }

    if (root.val != post[postIndex]) {
        root.right = constructFromPrePost(pre, post);
    }

    postIndex++;
    return root;
}
{% endhighlight %}

For example:

```
pre = [1,2,4,5,3,6,7]
post = [4,5,2,6,7,3,1]
```
```
preIndex	postIndex
0		0
1		0
2		0
3		1
4		3
5		3
6		4
```

{% highlight java %}
public TreeNode constructFromPrePost(int[] pre, int[] post) {
    Deque<TreeNode> dq = new ArrayDeque<>();
    dq.offer(new TreeNode(pre[0]));
    for (int i = 1, j = 0; i < pre.length; i++) {
        TreeNode node = new TreeNode(pre[i]);
        while (dq.getLast().val == post[j]) {
            dq.pollLast();
            j++;
        }
        if (dq.getLast().left == null) {
            dq.getLast().left = node;
        } else {
            dq.getLast().right = node;
        }
        dq.offer(node);
    }
    return dq.getFirst();
}
{% endhighlight %}

[Construct Binary Search Tree from Preorder Traversal][construct-binary-search-tree-from-preorder-traversal]

{% highlight java %}
private int index = 0;

public TreeNode bstFromPreorder(int[] preorder) {
    return build(preorder, Integer.MAX_VALUE);
}

public TreeNode build(int[] preorder, int high) {
    if (index == preorder.length || preorder[index] > high) {
        return null;
    }

    TreeNode root = new TreeNode(preorder[index++]);
    root.left = build(preorder, root.val);
    root.right = build(preorder, high);
    return root;
}
{% endhighlight %}

# Verification

[Verify Preorder Sequence in Binary Search Tree][verify-preorder-sequence-in-binary-search-tree]

{% highlight java %}
public boolean verifyPreorder(int[] preorder) {
    int low = Integer.MIN_VALUE;
    Deque<Integer> st = new ArrayDeque<>();
    for (int p : preorder) {
        if (p < low) {
            return false;
        }

        // monotonically decreasing stack
        // pops left subtree
        while (!st.isEmpty() && p > st.peek()) {
            // the sequence of low is the inorder traversal
            low = st.pop();
        }
        st.push(p);
    }
    return true;
}
{% endhighlight %}

In-place:

{% highlight java %}
public boolean verifyPreorder(int[] preorder) {
    int low = Integer.MIN_VALUE, i = -1;
    for (int p : preorder) {
        if (p < low) {
            return false;
        }

        // pops left subtree
        while (i >= 0 && p > preorder[i]) {
            // the sequence of low is the inorder traversal
            low = preorder[i--];
        }
        preorder[++i] = p;
    }
    return true;
}
{% endhighlight %}

For example:

```
[5,2,1,3,6]
```
```
		low
[5,2,1,3,6]	-2147483648
[5,2,1,3,6]	-2147483648
[5,2,1,3,6]	-2147483648
[5,3,1,3,6]	2
[6,3,1,3,6]	5
```

In-place, no overwriting:

{% highlight java %}
public boolean verifyPreorder(int[] preorder) {
    int low = Integer.MIN_VALUE;
    for (int i = 0; i < preorder.length; i++) {
        if (preorder[i] < low) {
            return false;
        }

        for (int j = i - 1; j >= 0 && preorder[j] < preorder[i]; j--) {
            low = Math.max(low, preorder[j]);
        }
    }
    return true;
}
{% endhighlight %}

# Complexity

# Binary Search Tree

[Binary Search Tree Iterator][binary-search-tree-iterator]

{% highlight java %}
private Deque<TreeNode> stack;

public BSTIterator(TreeNode root) {
    this.stack = new ArrayDeque<>();
    leftmostInorder(root);
}

/** @return the next smallest number */
public int next() {
    TreeNode tmpNode = stack.pop();
    leftmostInorder(tmpNode.right);
    return tmpNode.val;
}

/** @return whether we have a next smallest number */
public boolean hasNext() {
    return !stack.isEmpty();
}

private void leftmostInorder(TreeNode node) {
    while (node != null) {
        stack.push(node);
        node = node.left;
    }
}
{% endhighlight %}

[All Elements in Two Binary Search Trees][all-elements-in-two-binary-search-trees/submissions]

{% highlight java %}
public List<Integer> getAllElements(TreeNode root1, TreeNode root2) {
    List<Integer> list = new ArrayList<>();
    Deque<TreeNode> st1 = new ArrayDeque<>(), st2 = new ArrayDeque<>();
    TreeNode node1 = root1, node2 = root2;
    while (true) {
        while (node1 != null) {
            st1.push(node1);
            node1 = node1.left;
        }
        while (node2 != null) {
            st2.push(node2);
            node2 = node2.left;
        }

        if (st1.isEmpty() && st2.isEmpty()) {
            return list;
        }

        if (st2.isEmpty() || (!st1.isEmpty() && st1.peek().val <= st2.peek().val)) {
            node1 = st1.pop();
            list.add(node1.val);
            node1 = node1.right;
        } else {
            node2 = st2.pop();
            list.add(node2.val);
            node2 = node2.right;
        }
    }
    return list;
}
{% endhighlight %}

[all-elements-in-two-binary-search-trees/submissions]: https://leetcode.com/problems/all-elements-in-two-binary-search-trees/submissions/
[binary-search-tree-iterator]: https://leetcode.com/problems/binary-search-tree-iterator/
[binary-tree-preorder-traversal]: https://leetcode.com/problems/binary-tree-preorder-traversal
[binary-tree-inorder-traversal]: https://leetcode.com/problems/binary-tree-inorder-traversal
[binary-tree-postorder-traversal]: https://leetcode.com/problems/binary-tree-postorder-traversal
[construct-binary-search-tree-from-preorder-traversal]: https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/
[construct-binary-tree-from-inorder-and-postorder-traversal]: https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/
[construct-binary-tree-from-preorder-and-inorder-traversal]: https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/
[construct-binary-tree-from-preorder-and-postorder-traversal]: https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/
[find-largest-value-in-each-tree-row]: https://leetcode.com/problems/find-largest-value-in-each-tree-row/
[verify-preorder-sequence-in-binary-search-tree]: https://leetcode.com/problems/verify-preorder-sequence-in-binary-search-tree/
