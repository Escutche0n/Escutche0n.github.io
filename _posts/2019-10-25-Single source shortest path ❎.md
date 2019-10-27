# **Week 5: Single Source Shortest Paths**

### Priority Queues and Dijkstra’s Algorithm

In today’s lectures we’ll be discussing the **single source shortest paths** problem in a weighted, directed graph.



In particular we’ll be interested in **Dijkstra’s Algorithm** which is based on an **abstract data structure** called a **priority queue** which can be efficiently implemented as a **binary heap**.



- Vertices are junctions
- Edges are roads
- Edge weights are in miles
- Directed edges are one-way streets.



### Priority Queues 

A **priority queue**, **Q** stores a set of distinct elements.

Each element x has an associated value called its key - x.key 

A priority queue supports the following operations:

**`INSERT(x, k)`** - inserts **`x​`** with **`x.key = k`**

**`DECREASEKEY(x, k)`** - decreases the value of **`x.key`** to **`k`** where **`k < x.key`** 

**`EXTRACTMIN()`** - removes and returns the element with the smallest key 



### Using a Linked List as a Priority Queue

There are many ways in which we could implement a priority queue but they aren’t all efficient.



Let $n$ denote the number of elements in the queue

​	- our goal is to implement a queue with operations which scale well as $n$ *grows* 



We could implement a Priority Queue using an unsorted linked list: 



**`INSERT`** is very efficient,
 \- add the new item to the head of the list in $O(1)$ time 



**`EXTRACTMIN`** and **`DECREASEKEY`** are very *inefficient*, they take $O(n)$ time 

\- we have to look through the entire linked list to find an item *(in the worst case)* 



Instead, we could implement a Priority Queue using a sorted linked list:

**`EXTRACTMIN`** is very efficient,

​	- remove the head of the list in $O(1)$ time 



### Binary Heaps

A binary heap is an ‘almost complete’ binary tree, where every level is full except (possibly) the lowest, which is filled from left to right.



**Heap Property** Any element has a key *less than or equal to* the keys of its children.



A binary heap can be efficiently stored implicitly as an array A:

Moving around using:$ PARENT(i) = ⌊i/2⌋$ $LEFT(i) = 2i$ $RIGHT(i) = 2i + 1 $



|                      | INSERT   | DECREASEKEY | EXTRACTMIN |
| -------------------- | -------- | ----------- | ---------- |
| Unsorted Linked List | O(1)     | O(n)        | O(n)       |
| Sorted Linked List   | O(n)     | O(n)        | O(1)       |
| Binary Heap          | O(log n) | O(log n)    | O(log n)   |



That pesky **assumption**. . .



**Old Assumption** we can find the location of any element x in the Heap in O(1) time.

Previously we said that. . . 

Each element x has an associated value called its key - x.key

**New** **(more reasonable)** **Assumption
** Each element x also has an associated (unique) positive integer **ID** - x.ID 􏰅 N



|  | INSERT   | DECREASEKEY | EXTRACTMIN | SPACE |
| -------------------- | -------- | ----------- | ---------- | ----- |
| Unsorted Linked List | O(1)     | O(n)        | O(n)       | O(n)  |
| Sorted Linked List   | O(n)     | O(n)        | O(1)       | O(n)  |
| Binary Heap          | O(log n) | O(log n)    | O(log n)   | O(N)  |

 

**Part two** Dijkstra’s Algorithm 

In today’s lectures we’ll be discussing the **single source shortest paths** problem in a weighted, directed graph. . . 



In particular we’ll be interested in **Dijkstra’s Algorithm** which is based on an abstract data structure called a priority queue which can be efficiently implemented as a binary heap.





Single source shortest paths.

![](data:image/svg+xml;charset=utf8,%3C%3Fxml%20version%3D%221.0%22%20encoding%3D%22UTF-8%22%3F%3E%0A%3C!DOCTYPE%20svg%20PUBLIC%20%22-%2F%2FW3C%2F%2FDTD%20SVG%201.1%2F%2FEN%22%20%22http%3A%2F%2Fwww.w3.org%2FGraphics%2FSVG%2F1.1%2FDTD%2Fsvg11.dtd%22%3E%0A%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20xmlns%3Axlink%3D%22http%3A%2F%2Fwww.w3.org%2F1999%2Fxlink%22%20version%3D%221.1%22%20width%3D%2281px%22%20height%3D%2281px%22%20viewBox%3D%22-0.5%20-0.5%2081%2081%22%20content%3D%22%26lt%3Bmxfile%20host%3D%26quot%3Bwww.draw.io%26quot%3B%20modified%3D%26quot%3B2019-10-27T22%3A37%3A47.056Z%26quot%3B%20agent%3D%26quot%3BMozilla%2F5.0%20(Macintosh%3B%20Intel%20Mac%20OS%20X%2010_15)%20AppleWebKit%2F605.1.15%20(KHTML%2C%20like%20Gecko)%20Version%2F13.0.2%20Safari%2F605.1.15%26quot%3B%20etag%3D%26quot%3BKDFRMRk2J0G9M84rtbIC%26quot%3B%20version%3D%26quot%3B12.1.7%26quot%3B%20type%3D%26quot%3Bdevice%26quot%3B%20pages%3D%26quot%3B1%26quot%3B%26gt%3B%26lt%3Bdiagram%20id%3D%26quot%3BQw3vJJ5kyIjivK_xdnf8%26quot%3B%20name%3D%26quot%3BPage-1%26quot%3B%26gt%3BjZJNb4MwDIZ%2FDcdJhXQMrmXd1sOkVWgf1whcEimQKKQD%2BusXFocPVZV6iezHju28TkCyun%2FVVLF3WYIIok3ZB%2BQ5iKKURPYcweBAvE0cqDQvHQpnkPMLINwgPfMS2lWikVIYrtawkE0DhVkxqrXs1mknKdZdFa3gCuQFFdf0m5eGOZpETzN%2FA14x3zmMUxepqU%2FGl7SMlrJbILIPSKalNM6q%2BwzEqJ3Xxd17uRGdBtPQmHsuHLKPJN6qY7ctvi6fMTscf9IHrPJLxRkfjMOawStgq1ixrbPrGDeQK1qMkc6u2zJmamG90Jq0VW4DJ96DbbrD2qAN9DeHDicp7BcCWYPRg03xFwiqh98nekS%2FWywDEVvswTOK66%2BmyrNC1kCRvDsv4z%2B2%2BNFk%2Fwc%3D%26lt%3B%2Fdiagram%26gt%3B%26lt%3B%2Fmxfile%26gt%3B%22%20style%3D%22background-color%3A%20rgb(255%2C%20255%2C%20255)%3B%22%3E%3Cdefs%2F%3E%3Cg%3E%3Cellipse%20cx%3D%2240%22%20cy%3D%2240%22%20rx%3D%2240%22%20ry%3D%2240%22%20fill%3D%22%23ffffff%22%20stroke%3D%22%23000000%22%20pointer-events%3D%22none%22%2F%3E%3C%2Fg%3E%3C%2Fsvg%3E)