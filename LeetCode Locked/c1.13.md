## Binary Tree Upside Down

>**Question**

Given a binary tree where all the right nodes are either leaf nodes with a sibling (a left node that shares the same parent node) or empty, flip it upside down and turn it into a tree where the original right nodes turned into left leaf nodes. Return the new root.

For example:

Given a binary tree `{1,2,3,4,5}`,
```
    1
   / \
  2   3
 / \
4   5
```

return the root of the binary tree `[4,5,2,#,#,3,1]`.
```
   4
  / \
 5   2
    / \
   3   1
```

>**Solution**

At each node you want to assign:
p.left = parent.right;
p.right = parent;

>**Top down approach**

We need to be very careful when reassigning current node’s left and right children. Besides making a copy of the parent node, you would also need to make a copy of the parent’s right child too. The reason is the current node becomes the parent node in the next iteration.


[here](http://blog.csdn.net/whuwangyi/article/details/43186045)
recursion
```java
public TreeNode UpsideDownBinaryTree(TreeNode root) {
  if (root == null)
    return null;
  TreeNode parent = root, left = root.left, right = root.right;
  if (left != null) {
    TreeNode ret = UpsideDownBinaryTree(left);
    left.left = right;
    left.right = parent;
    return ret;
  }
  return root;
}
```

iteration
```java

	public TreeNode UpsideDownBinaryTree(TreeNode root) {
		TreeNode node = root, parent = null, right = null;
		while (node != null) {
			TreeNode left = node.left;
			node.left = right;
			right = node.right;
			node.right = parent;
			parent = node;
			node = left;
		}
		return parent;
	}
```

postorder Traversal
```java
private TreeNode out = null;
public TreeNode UpsideDownBinaryTree(TreeNode root) {
  TreeNode dummy = new TreeNode(0);
  dummy.left = new TreeNode(0);
  out = dummy;

  postorder(root);
  return dummy.right;
}

private void postorder(TreeNode root) {
  if (root == null)
    return;

  postorder(root.left);
  postorder(root.right);

  if (out.left == null) {
    out.left = root;
    out.left.left = null;
    out.left.right = null;
  } else if (out.right == null) {
    out.right = root;
    out.right.left = null;
    out.right.right = null;
  }

  if (out.left != null && out.right != null)
    out = out.right;
}
```
