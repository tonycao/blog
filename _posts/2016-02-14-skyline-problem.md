---
layout: post
title:  "Skyline Problems"
date:   2016-02-14 17:27:33
categories: Technical, Interview
---

A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Now suppose you are given the locations and height of all the buildings as shown on a cityscape photo (Figure A), write a program to output the skyline formed by these buildings collectively (Figure B).

The geometric information of each building is represented by a triplet of integers `[Li, Ri, Hi]`, where Li and Ri are the x coordinates of the left and right edge of the ith building, respectively, and Hi is its height. It is guaranteed that `0 ≤ Li`, `Ri ≤ INT_MAX`, `0 < Hi ≤ INT_MAX`, and `Ri - Li > 0`.


For instance, the dimensions of all buildings in Figure A are recorded as: `[ [2 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ]`

The output is a list of "key points" (red dots in Figure B) in the format of `[ [x1,y1], [x2, y2], [x3, y3], ... ]` that uniquely defines a skyline. A key point is the left endpoint of a horizontal line segment.

For instance, the skyline in Figure B should be represented as: `[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0] ]`.


![](http://i66.tinypic.com/2cr2hqp.png)

More details on [Leetcode 218 The skyline problem](https://leetcode.com/problems/the-skyline-problem/).

Idea:
* sort the start and end points for all the buildings and check each point one by one.
* use a heap to store the heights of current buildings, i.e. overlapped buildings.
* use pre and cur to store the heights of previous building and current building respectively. if `pre!=cur` output the reuslt.

Code:

```c++
  vector<pair<int, int>> getSkyline(vector<vector<int>>& buildings) {
        // http://www.cnblogs.com/easonliu/p/4531020.html
        // Time Complexity: O(n log n)
        // Space Complexity: O(n)
        vector<pair<int, int>> height;
        for (auto &b : buildings) {
            height.push_back({b[0], -b[2]});
            height.push_back({b[1], b[2]});
        }
        sort(height.begin(), height.end());
        multiset<int> heap;
        heap.insert(0);
        vector<pair<int, int>> res;
        int pre = 0, cur = 0;
        for (auto &h : height) {
            if (h.second < 0) {
                heap.insert(-h.second);
            } else {
                heap.erase(heap.find(h.second));
            }
            cur = *heap.rbegin();
            if (cur != pre) {
                res.push_back({h.first, cur});
                pre = cur;
            }
        }
        return res;
    }
```

Another similar LintCode problem [Building Outline](http://www.lintcode.com/en/problem/building-outline/)

![](http://www.lintcode.com/media/problem/jiuzhang3.jpg)
* the only difference is when to output the result

Code:

```c++
  vector<vector<int> > buildingOutline(vector<vector<int> > &buildings) {
        // write your code here
        vector<pair<int, int>> height;
        for (auto b : buildings) {
            height.push_back(make_pair(b[0], -b[2]));
            height.push_back(make_pair(b[1],  b[2]));
        }

        sort(height.begin(), height.end());
        multiset<int> heap;
        heap.insert(0);
        vector<vector<int>> res;
        int pre = 0, cur = 0, start = 0;

        for (auto h : height) {
            if (h.second < 0) {
                heap.insert(-h.second);
            } else {
                heap.erase(heap.find(h.second));
            }
            cur = *heap.rbegin();
            if (cur != pre) {
                if (pre != 0)
                    res.push_back({start, h.first, pre});   
                start = h.first;
            }
            pre = cur;
        }
        return res;
    }
```
