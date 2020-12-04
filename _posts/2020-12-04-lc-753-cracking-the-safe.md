---
published: true
layout: post
author: eugene
categories: Algorithm
tags:
  - lc
  - algorithm
  - graph
  - euler_path
  - euler_circuit
  - hierholzer_algorithm
  - string
---
LC 753

# 753 Cracking the Safe

https://leetcode.com/problems/cracking-the-safe/

# Notes:

## Euler Path/Circuit
> Given a graph, euler path is the path that goes through all edges.

> Euler Circuit connects the start and ends of the Euler Path.

This is very similar to topological sort, but it's harder than topological sort, because the two adjacent result must be connected by an edge.

The graph is suppose to be strongly connected for all the nodes with at least one edge.

For undirected graph:
> Euler Path exist if (0/2) # of nodes have odd number of edges.

> Euler Circuit exist if all nodes have even number of edges.

For directed graph:
> Euler Path exist if all other nodes have same number of i/o edges, expect one node have extra in edges and another have extra out edges.

> Euler Circuit exist if all nodes have same number of i/o edges.

## Hierholzer's algorithm
```
initialise a stack
initialise a deque

stack.push(
    if graph is a euler circuit then push any node
    else (the graph would be euler path) push the startNode (which is the node with odd number of edges or 1 extra in edge)
)

while stack is not empty:
    top = stack.top
    
    if outEdges of top is zero:
    	stack.pop()
    	deque.push_front( top )
    else:
    	for each edge:
        	if edge is not visited:
            	stack.push(edge.toNode)
                break;
deque;
```

### Problem

Given a safe, which can be opened by a `n` length of password of `k` characters.

You can insert an arbitary length of a string; in which if the string contains the password, the safe is cracked.

Find the string that can crack all safe with a minimum length.

$$ n \in [1,4]$$

$$ k \in [1,9]$$

### Examples

|input||output|
|--|--|--|
|`n=1 k=1`||`0`|
|`n=1 k=4`||`0123`|
|`n=2 k=1`||`00`|
|`n=2 k=2`||`001101`|
|`n=3 k=2`||`00011100101`|

### Thought Process

The first string we can think of which is the worst solution is to concatenate all combination password as one.

`n=2 k=2` then `00011011` can be the solution.

Next step is that can see that if we merge two password that have parts that are the same. Ie. 
`00` and `01` have the same `0` one at the *front* and another at the *back*.

In which we can see that `00` and `01` are like the edge between different nodes from *front* to *back*.

But what is the node though? 

When `n` is 3

The `...010...` password would have the `01` as the *front* and `10` as the *back*.

The `...001...` password would have the `00` as the *front* and `01` as the *back*.

Therefore the *front*s and the *back*s are the nodes!

Indeed if we extend our substring abit longer ie.`...0011100...` We can see that we are actually moving from `00` -> `01` -> `11` -> `11` -> `10` -> `00`

We can save space if we keep moving on the graph and not to start from some other place. Indeed if we can find the euler path that will be the optimal solution. 

Recall that a graph contains a euler path if it satisfy 
> Euler Path exist if all other nodes have same number of i/o edges, expect one node have extra in edges and another have extra out edges.

Since all node have `k` edges, the euler path requirements is satisfied!

So we can just perform the euler path search.

### Solution

1. initialise some values:
	1. `visited` boolean array of False - edge visited
    2. `out` int array of k - out edges of nodes.
    3. `ten_to_n` - 10^n
    4. `stack` - we perform our operation on the item of stack
    	1. put node `0000`(which is same as `0`) into `stack`
    5. `path` - our result
2. while `stack` is not empty:
    1. if `top` of the `stack` does not have out edges (`out[top]` == 0):
        1. pop the top of the `stack`
        2. add `top` to `path`
    2. else:
    	1. dont pop the top of the `stack`
        2. for each out `edge`:
            1. if not `edge` is not `visited`:
            	1. set `edge` `visited` to True
                2. reduce out of the node(`top`) by one (`out[top]--`)
                3. add the `node` point by the other side of the `edge` to `stack`
                4. break
3. the reverse of the `path` is the solution
    

### Code

```python
class Solution:
    def crackSafe(self, n: int, k: int) -> str:
        if n == 1:
            return ''.join(map(str,range(k)))
        
        visited = [False] * (10**4);
        
        out = [k] * (10**4);
        
        ten_to_n = 10**n;
        
        stack = [0]
        
        path = []
        
        
        while stack:
            # print(stack)
            top = stack[-1];
            
            if out[top] == 0:
                stack.pop();                
                path.append(top)
            else:
                for edgeMod in range(k):
                    
                    edge = (top * 10) % ten_to_n + edgeMod
                    if not visited[edge]:
                        visited[edge] = True;
                        out[top] -= 1;
                        stack.append(edge%(ten_to_n//10));
                        break;
        path = list(reversed(path))
        return str(path[1]).zfill(n) + ''.join(map(lambda x: str(x % 10), path[2:]))
        
        
    
```
