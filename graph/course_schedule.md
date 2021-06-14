# 210. [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/) [Medium]

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.
Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.


```
Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
```

```
Example 2:

Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
```

```
Example 3:

Input: numCourses = 1, prerequisites = []
Output: [0]
```

Constraints:

* `1 <= numCourses <= 2000`
* `0 <= prerequisites.length <= numCourses * (numCourses - 1)`
* `prerequisites[i].length == 2`
* `0 <= ai, bi < numCourses`
* `ai != bi`
* All the pairs `[ai, bi]` are distinct.

### DFS

* build the graph G(V,E)
* label node as white, gray, black
  * white - never visit
  * gray - visit but not dependencies not finished (detect cycle)
  * black - no other leaf to go
* dfs search each node
  * turn the color to gray when visit
  * turn the color to black if no other leaf to go
    * put into stack
  * if visit gray node before it fill filled dependency > there is cycle > impossible
* return list from top of the stack

```cpp
class Solution {
private:

    enum class Color {
        White,
        Gray,
        Black,
    };

    map<int, Color> node_state;
    map<int, vector<int>> node_edge;
    stack<int> topology;
    bool cycle;

    void dfs(int course) {
        if (node_state[course] == Color::Gray) { // cycle
            cycle = true;
        }

        if (cycle) {
            return;
        }

        node_state[course] = Color::Gray;

        for (const auto & c : node_edge[course]) {
            if (node_state[c] != Color::Black) {
                dfs(c);
            }
        }

        node_state[course] = Color::Black;
        topology.push(course);
    }

public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        cycle = false;

        for (int i = 0; i < numCourses; ++i) {
            node_state[i] = Color::White;
        }

        for (const auto & p : prerequisites) {
            node_edge[p[1]].push_back(p[0]); // p[1] -> p[0]
        }

        for (int i = 0; i < numCourses; ++i) {
            if (node_state[i] == Color::White) {
                dfs(i);
            }
        }

        vector<int> res;

        if (cycle) {
            return res;
        }

        while(!topology.empty()) {
            res.push_back(topology.top());
            topology.pop();
        }

        return res;
    }
};
```

* Time Complexity: O(V+E)
* Space Complexity: O(V+E)
