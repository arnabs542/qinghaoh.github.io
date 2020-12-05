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
[find-largest-value-in-each-tree-row]: https://leetcode.com/problems/find-largest-value-in-each-tree-row/
