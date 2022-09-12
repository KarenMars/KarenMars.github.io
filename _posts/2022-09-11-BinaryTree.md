## Binary Tree 

- [Binary Tree](#binary-tree)
  - [What is binary tree ?](#what-is-binary-tree-)
  - [Iteration](#iteration)




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

### Iteration
- Preorder 
    - root, left, right 
      - iteration by **recursion**
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
      - iteration with a stack
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
- Postorder 
  - right, left, root