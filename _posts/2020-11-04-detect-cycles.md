---
published: true
layout: post
author: eugene
categories: Algorithm
tags:
  - lc
  - algorithm
  - graph
  - cycle
---

LC 207 <br/>
LC 210

### Detect Cycles in Directed Graph 1

Steps:

1. Keep two lists `visited` and `completed` same length of number of nodes and initialised to *false*
2. define a function(`haveCycle`) to solve for node:
    1. `visited[node]` = *true*
    2. for each children of node:
        1. if `visited[children]` , but not `completed[children]`:
            1. **HAVE CYCLE**
        2. else:
            1. if children `haveCycle` :
                1. **HAVE CYCLE**
    3. `completed[node]` = *true*
3. for each `node`:
    1. if not `visited` and `haveCycle(node)`:
        1. **HAVE CYCLE**


### Detect Cycles in Directed Graph 2

Steps:

1. Keep a lists `indegree` initialised to number of child point into that node.
2. Keep a `queue` to queue that contains the nodes that do not have child
3. while `queue` is not empty:
    1. `node` = `queue.pop()`
    2. for each `parent` of `node`:
        1. detected dependency and we remove this dependency `indegree[parent]` -= 1
        2. if `indegree[parent]` is 0: there is no child anymore
            1. push `parent` into `queue`
4. if all `indegree` is not 0: there is still some dependencies
    1. **HAVE CYCLE**
    
How this works? If the node is not a leaf, we the dependencies of parent by reducing the number of dependencies of parent. If the parent have no more dependencies the parent become the leaf and we can do this continuously. If the parent still have dependencies and its child dependent on something its depend on the child will never be pushed into the list, because child was never resolved. Therefore parent will never be solved too (indegree more than 1).
    
### Flatten a tree with dependencies

Resolve the tree by solving the leaf first then the parent. This algorithm is the similar to the one above.

Steps:
1. Keep two lists `visited` and `completed` same length of number of nodes and initialised to *false*
2. A list of `solution` = [];
3. define a function `solve` to solve for `node`:
    1. `visited[node]` = *true*
    2. for each `children` of `node`:
        1. if `visited[children]` , but not `completed[children]`:
            1. **HAVE CYCLE**
        2. else:
            1. if children `haveCycle` :
                1. **HAVE CYCLE**
    3. `completed[node]` = *true*
    4. push `node` to `solution`
4. for each `node`:
    1. if not `visited` and `solve` for `node`:
        1. **HAVE CYCLE**
5. print `solution`


### Code
Method 1
```python
def haveCycle(node, graph, visited, completed):
    visited[node] = True
    
    for out_node in graph[node]:
        if not visited[out_node] and \
            haveCycle(out_node, graph, visited, completed):
            return True
        else if not completed[out_node]:
            return True
    
    completed[node] = True
    return False

numNodes = 3
graph = [
	0: [1,2],  # 0 to 1 and 0 to 2
    1: [0],
    2: [2]
]
visited = [False] * numNodes
completed = [False] * numNodes

for node in range(numNodes):
    if not visited[node] and haveCycle(node, graph, visited, completed):
        print('Cycle found')
```

Method 2
```python
from collections import deque

numNodes = 3
graph = [
	0: [1,2],  # 0 to 1 and 0 to 2
    1: [0],
    2: [2]
]

indegree = [0] * numNodes

queue = deque()
for node in graph:
    for out_node in graph[node]:
        indegree[out_node] += 1

for node, indeg in enumerate(indegree):
    if indeg == 0:
        queue.append(node)

while queue:
    node = queue.popleft()
    for out_node in graph[node]:
        indegree[out_node] -= 1
        if indegree[out_node] == 0:  # no more indegree all of them are solved
            queue.append(out_node)

if sum(indegree) != 0:
    print('Cycle found')
```
