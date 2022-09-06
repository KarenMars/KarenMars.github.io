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
- Key problems
  1. How to set the difference between the speed of the two pointers?
  2. Use dummy node before the head node 
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
- [Leetcode 19. Remove Nth Node From End of List](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)
  - first solution: iterate the whole linked list and iterate the linked list again
  - second solution: two pointers 
    ```python
    class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        if not head:
            return None
        dummy_node = ListNode(0)
        dummy_node.next = head

        fast_cur, slow_cur = dummy_node, dummy_node

        i = 0
        
        while i in range(0,n):
            fast_cur = fast_cur.next
            i += 1

        while fast_cur.next:
            fast_cur = fast_cur.next
            slow_cur = slow_cur.next
        
        temp = slow_cur.next.next
        slow_cur.next.next = None
        slow_cur.next = temp
        return dummy_node.next
    ```
  

