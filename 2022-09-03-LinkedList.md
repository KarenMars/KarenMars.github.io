## Linked list

- [Linked list](#linked-list)
  - [What is linked list ?](#what-is-linked-list-)
  - [Basic operations of a linked list](#basic-operations-of-a-linked-list)
  - [Examples](#examples)

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