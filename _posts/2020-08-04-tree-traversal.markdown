---
layout: post
title:  "Tree Traversal"
tags: tree
---
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
    List<Integer> list = new ArrayList<Integer>();
    Stack<TreeNode> stack = new Stack<TreeNode>();
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
    Stack<TreeNode> stack = new Stack<>();
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
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<Integer>();
    traverse(root, list);
    return list;
}

private void traverse(TreeNode root, List<Integer> list) {
    if (root != null) {
        list.add(root.val);
        traverse(root.left, list);
        traverse(root.right, list);
    }
}
{% endhighlight %}

### Stack
{% highlight java %}
public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    TreeNode node = root;
    while (node != null || !stack.isEmpty()) {
        if (node != null) {
            list.add(node.val);
            stack.push(node);
            node = node.right;  // right -> left
        } else {
            node = stack.pop();
            node = node.left;
        }
    }
    Collections.reverse(list);  // reverse
    return list;
}
{% endhighlight %}

[binary-tree-preorder-traversal]: https://leetcode.com/problems/binary-tree-preorder-traversal
[binary-tree-inorder-traversal]: https://leetcode.com/problems/binary-tree-inorder-traversal
[binary-tree-postorder-traversal]: https://leetcode.com/problems/binary-tree-postorder-traversal
