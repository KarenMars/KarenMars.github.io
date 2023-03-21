## Binary Tree 

- [Binary Tree](#binary-tree)
  - [What is binary tree ?](#what-is-binary-tree-)
  - [Traversal](#traversal)




### What is binary tree ?
- Binary tree in Python 
    ```python
    class TreeNode:
        def __init__(self,val):
            self.val = val
            self.left = None
            self.right = None
    ```
    -  For binary search tree
        vals of left subtree <= root.val <= vals of right subtree 

### Traversal
- Preorder 
    - root, left, right 
      - traversal by **recursion**
        ```python
        class Solution:
            def preorderTraversal(self, root: TreeNode) -> List[int]:
                def preorder(root: TreeNode):
                    if not root:
                        return
                    res.append(root.val)
                    preorder(root.left)
                    preorder(root.right)
                
                res = list()
                preorder(root)
                return res
        ```
      - traversal with a stack
        ```python
        class Solution(object):
            def preorderTraversal(self, root):
                """
                :type root: TreeNode
                :rtype: List[int]
                """

                res = []
                if not root: return res

                stack = []
                node = root

                while node or stack:
                    while node:
                        stack.append(node)
                        res.append(node.val)
                        node = node.left
                    node = stack.pop()
                    node = node.right

                return res
        ```
- Inorder 
  - left, root, right
  - [example](https://leetcode.cn/problems/binary-tree-inorder-traversal/solution/er-cha-shu-de-zhong-xu-bian-li-by-leetcode-solutio/)
- Postorder 
  - left, right, root
  - [example](https://leetcode.cn/problems/binary-tree-postorder-traversal/solution/er-cha-shu-de-hou-xu-bian-li-by-leetcode-solution/)

- Level order
  - [example](https://leetcode.cn/problems/binary-tree-level-order-traversal/solution/bfs-de-shi-yong-chang-jing-zong-jie-ceng-xu-bian-l/)
  - 