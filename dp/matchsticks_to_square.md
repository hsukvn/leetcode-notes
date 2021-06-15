# 473. [Matchsticks to Square](https://leetcode.com/problems/matchsticks-to-square/) [Medium]

You are given an integer array `matchsticks` where `matchsticks[i]` is the length of the ith matchstick. You want to use all the matchsticks to make one square. You should not break any stick, but you can link them up, and each matchstick must be used exactly one time.

Return true if you can make this square and false otherwise.

```
Example 1:

Input: matchsticks = [1,1,2,2,2]
Output: true
Explanation: You can form a square with length 2, one side of the square came two sticks with length 1.
```

```
Example 2:

Input: matchsticks = [3,3,3,3,4]
Output: false
Explanation: You cannot find a way to form a square with all the matchsticks.
```

Constraints:

* `1 <= matchsticks.length <= 15`
* `0 <= matchsticks[i] <= 109`

### DFS

* calculate width of the square
  * return false when sum of `matchsticks` is not able to divided by 4
  * return false if any of the `matchsticks` is larger than width
* dfs by putting the matchsticks into one of the edge

```cpp
class Solution {
    bool dfs(vector<int> &matchsticks, int idx, vector<unsigned long> &edges) {
        if (idx == matchsticks.size()) {
            return true;
        }

        for (int i = 0; i < edges.size(); ++i) {
            if (matchsticks[idx] <= edges[i]) {
                edges[i]-=matchsticks[idx];
                if (dfs(matchsticks, idx+1, edges)) {
                    return true;
                }
                edges[i]+=matchsticks[idx];
            }
        }
        return false;
    }
public:
    bool makesquare(vector<int>& matchsticks) {
        unsigned long total_length = 0;

        for (const auto &m : matchsticks) {
            total_length+=m;
        }

        if (total_length % 4 != 0) {
            return false;
        }

        unsigned long width = total_length / 4;

        vector<unsigned long> edges(4, width);

        sort(matchsticks.begin(), matchsticks.end(), std::greater<int>());

        return dfs(matchsticks, 0, edges);
    }
};
```

* Time Complexity: O(4^N)
  * Test every matchsticks in one of the edges.
* Space Complexity: O(N)
  * For recursive solution the space complexity is the stack space and the deepest recursive call is with size N.


### Dynamic Programming

todo

