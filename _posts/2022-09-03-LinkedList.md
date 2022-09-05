## Linked list

- [Linked list](#linked-list)
  - [What is linked list ?](#what-is-linked-list-)
  - [Basic operations of a linked list](#basic-operations-of-a-linked-list)
  - [Examples](#examples)
    - [Two pointers (Fast and slow pointers)](#two-pointers-fast-and-slow-pointers)

### What is linked list ?
- [Linked list](https://leetcode.cn/leetbook/read/linked-list/jsumh/)
  - Single linked list (one link between two elements)
  - Double linked list (two link between two elements)
- [Implementation in Python](https://realpython.com/linked-lists-python/#implementing-your-own-linked-list)
    ```python
    class Node:
        def __init__(self,data=None):
            self.data = data
            self.next = None
    class SingleLinkedList:
        def __init(self):
            self.head = None
    ```
    Also the method of [`__repr__`](https://www.educative.io/answers/what-is-the-repr-method-in-python) can be added to class. We can define our own string representation of our class objects.

### Basic operations of a linked list
- Insertion
  - Time complexity O(1)
    ```python 
    # add node after a specific data
    def add_after(self, target_node_data, new_node):
        if self.head is None:
            raise Exception("empty linked list")
        for node in self:
            if node.data == target_node_data:
                new_node.next = node.next
                node.next = new_node
        raise Exception("Node with data '%s' not found" %  target_node_data) 
    ```
- Deletion
  - Time complexity O(1)
- Search
  - Time complexity O(N)

### Examples
#### Two pointers (Fast and slow pointers)
- [Leetcode 141. Linked list cycle](https://leetcode.cn/problems/linked-list-cycle/)
- [Leetcode 160. Intersection of Two Linked Lists](https://leetcode.cn/problems/intersection-of-two-linked-lists/)
  - first solution: using hash set to store visited nodes, time complexity O(m+n), space complexity O(m+n)
  - second solution: reduce the space complexity, use time complexity replace the space complexity
    ```python
    class Solution(object):
      def getIntersectionNode(self, headA, headB):
          """
          :type head1, head1: ListNode
          :rtype: ListNode
          """
          if not headA or not headB:
            return None
          cur_A, cur_B = headA, headB
          while cur_A or cur_B:
              if cur_A == cur_B:
                  return cur_A
              if not cur_B:
                  cur_B = headA
              else:
                  cur_B = cur_B.next
              if not cur_A:
                  cur_A = headB
              else:
                  cur_A = cur_A.next
          return None
    ```
  

