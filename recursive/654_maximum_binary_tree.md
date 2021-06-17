# 654. [Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/) [Medium]

You are given an integer array nums with no duplicates. A maximum binary tree can be built recursively from nums using the following algorithm:

1. Create a root node whose value is the maximum value in nums.
2. Recursively build the left subtree on the subarray prefix to the left of the maximum value.
3. Recursively build the right subtree on the subarray suffix to the right of the maximum value.

Return the maximum binary tree built from nums.

```
Example 1:

Input: nums = [3,2,1,6,0,5]
Output: [6,3,5,null,2,0,null,null,1]
Explanation: The recursive calls are as follow:
- The largest value in [3,2,1,6,0,5] is 6. Left prefix is [3,2,1] and right suffix is [0,5].
    - The largest value in [3,2,1] is 3. Left prefix is [] and right suffix is [2,1].
        - Empty array, so no child.
        - The largest value in [2,1] is 2. Left prefix is [] and right suffix is [1].
            - Empty array, so no child.
            - Only one element, so child is a node with value 1.
    - The largest value in [0,5] is 5. Left prefix is [0] and right suffix is [].
        - Only one element, so child is a node with value 0.
        - Empty array, so no child.
```

```
Example 2:

Input: nums = [3,2,1]
Output: [3,null,2,null,1]
```

Constraints:

* `1 <= nums.length <= 1000`
* `0 <= nums[i] <= 1000`
* All integers in nums are unique.


### Recursive

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
    TreeNode* TreeBuild(vector<int>& nums, int left, int right) {
        int max = INT_MIN;
        int index = -1;

        for (int i = left; i <= right; ++i) {
            if (nums[i] > max) {
                max = nums[i];
                index = i;
            }
        }

        TreeNode *root = new TreeNode(max);
        if (index-1 >= left) {
            root->left = TreeBuild(nums, left, index-1);
        }
        if (index+1 <= right) {
            root->right = TreeBuild(nums, index+1, right);
        }
        return root;

    }
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return TreeBuild(nums, 0, nums.size()-1);
    }
};
```

* Time Complexity: O(n^2)
  * In worse case we will have a sorted array which lead to a tree with depth n
  * loop the array to find the max element in each round
    * (1+n)*n/2 times of array traversal => O(n^2)
    * will have O(nlogn) in normal case
* Space Complexity: O(n)
  * max depth in worse case is n


### Linear

* use a stack to keep tree nodes and ensure a decreasing order
* scan all the numbers from left to right
  * create a node for current number
  * keep pop the stack until empty or a bigger number than current
  * assign the largest element to the current node's left
* assign the current node to the right of the top node of the stack if any
* return the bottom element of the stack cause it is the largest element


```
class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        vector<TreeNode*> stk;
        for (int i = 0; i < nums.size(); ++i)
        {
            TreeNode* cur = new TreeNode(nums[i]);
            while (!stk.empty() && stk.back()->val < nums[i])
            {
                cur->left = stk.back();
                stk.pop_back();
            }
            if (!stk.empty())
                stk.back()->right = cur;
            stk.push_back(cur);
        }
        return stk.front();
    }
};
```

* Time Complexity: O(n)
* Space Complexity: O(n)
