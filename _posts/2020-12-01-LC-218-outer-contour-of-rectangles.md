---
published: false
---
## 218 The Skyline Problem

https://leetcode.com/problems/the-skyline-problem/

### Problem

Given the `left`, `right` and `height` of the rectangles, these rectangles are sitting on the ground. Find the `outer contour` which is the shadow of these rectangle, the `outer contour` can be represented by a set of `(x,y)` points but no two adjacent point can have the same `y` or `x`. Sequence of the `(x,y)` is also important. 

Therefore, points like `[(2,4), (3, 4), (5,0)]` is not allowed and should be `[(2,4),(5,0)]`. Also points like `[(2,4), (2,0)]` is not allowed because its ambiguious.

### Thought Process

First we need to note that we only changes state(add new solution) when we are seeing a new point of the building(starting point/ending point).

And a statement in the problem, 

> The output is a list of "key points" (red dots in Figure B) in the format of [ [x1,y1], [x2, y2], [x3, y3], ... ] that uniquely defines a skyline. **A key point is the left endpoint of a horizontal line segment.**

should sparks some ideas.

> At x, if we keep a pool(array/heap) of heights, and the highest heights is the contour we need to draw.

The catch is that if there is some other drawn segment with the same height or the same x, then we don't draw them.

### Solution

1. Create a `new list of the buildings` to be sorted by `left` and `right`.
2. Create a new array, `have_seen` initialised `false` with length of the buildings. Because its easier this way, than to pop arbitary item out of the heap.
3. Create a `heap`
4. for each item in the `new list of buildings`:
	1. if its the `ending point`: set have seen to `true`
    2. else: push this item into the `heap`
    3. while `top of heap` have seen:
    	1. pop the `heap`
    4. if `heap` is empty:
    	1. if the `last item in solution` is not `y` == `0`:
        	1. While `last item in solution` has the same `x`:
            	1. pop the `solution`
            2. if the `last item in solution` is still not `y` == `0`:
            	1. `(x, 0)` is pushed into the `solution`
        2. else if the `last item in solution` has different `y` as the `top of the heap`:
        	1. while `last item in solution` has the same `x`:
            	1. pop the `solution`
            2. if the `last item in solution` has different `y` as the `top of the heap`:
            	1. `(x, top_heap)` is pushed into the `solution`
5. return `solution`


```python
class Solution:
    def getSkyline(self, buildings: List[List[int]]) -> List[List[int]]:
        
        solution = [(-1,0)]
        buildingRep = []
        
        for idx, (l, r, h) in enumerate(buildings):
            buildingRep.append((l, (idx, l, h)))
            buildingRep.append((r, (idx, r, 0)))
        
        buildingRep.sort()
        
        buildingSeen = [False]*10000
        
        heap = []
        
        push = heapq.heappush
        pop = heapq.heappop
        # heapq.heappush
        # heapq.heappop
        # heapq.heapify
        
        for b_x, (idx, x, y) in buildingRep:
            
            if y == 0:
                buildingSeen[idx] = True
                
            else:
                push(heap, (-y, idx))
            
            while heap and buildingSeen[heap[0][1]]:
                pop(heap)
            
            if not heap:
                if solution[-1][1] != 0:
                    while solution[-1][0] == x:
                        solution.pop()
                    if solution[-1][1] != 0:
                        solution.append((x, 0))
            else:
                if solution[-1][1] != -heap[0][0]:
                    while solution[-1][0] == x:
                        solution.pop()
                    if solution[-1][1] != -heap[0][0]:
                        solution.append((x, -heap[0][0]))
        return solution[1:]
```

