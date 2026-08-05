## Algorithms
### Linked list
Is a list of nodes in which each node keeps two major components:
1. Data.
2. Pointer of its next node. `nullptr` if there is no next node.

* Node "head" points to the first node.
* The pointer to the next node in the last node is set to `nullptr`.

Example:
```c++
struct Node
{
  int m_data;
  Node* m_next;
}
```
### Binary Search Tree (BST)
Is a binary tree of nodes. Each node has three major components:
1. Data.
2. Pointer to the node on left. The value of the data in node on left is less than the value of the data in this node.
3. Pointer to the node on right. The value of the data in node on right is more than the value of the data in this node.

Example:
```
             50
            /   \
          30     70
        /   \   /  \
      20    40 60   80
```
Example of node declaration:
```c++
struct Node
{
  int m_data;
  Node* m_left;
  Node* m_right;
}
```
Searching for a node, based on its data, in such tree is easy: We start from the root, 50 in above example, and iteratively:
* If our value is smaller than the value of current node, take the node on left.
* If our value is greater than the value of current node, take the node on right.

till the target node is found.



```
