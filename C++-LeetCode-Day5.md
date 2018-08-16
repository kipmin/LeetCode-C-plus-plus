---
title: LeetCode-Day5
date: 2018-08-12 12:00:00
tags:
---

### 两数之和	LeetCode-1

##### 题目

给定一个整数数组和一个目标值，找出数组中和为目标值的**两个**数。

你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。

**示例:**

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

##### 思路

因为只有一种答案，所以我直接用了一个双层循环得到答案返回，n²的时间复杂度果然很慢。

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int len = nums.size();
        vector<int> result;
        for(int i = 0; i < len; i++) {
            for (int j = 0; j < len; j++) {
                if (i!=j) {
                    if (nums[i] + nums[j] == target) {
                        result.push_back(i);
                        result.push_back(j);
                        return result;
                    }
                }
            }
        }
        return result;
    }
};
```

于是又看了一下前面的算法是怎样的，我大概了解了思路，用target减去数组中的数，放进容器里，再找到数组中相同的答案，把位置记在新的数组中返回即可。

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int len = nums.size();
        int a[len] = {1};
        vector<int> result;
        for(int i = 0; i < len; i++) {
            for (int j = 0; j < len; j++) {
                if (i!=j) {
                    if (nums[i] + nums[j] == target) {
                        result.push_back(i);
                           result.push_back(j);
                           return result;
                    }
                }
            }
        }
        return result;
    }
};
```

### 有效的数独 	LeetCode-36

##### 题目

判断一个 9x9 的数独是否有效。只需要**根据以下规则**，验证已经填入的数字是否有效即可。

1. 数字 `1-9` 在每一行只能出现一次。
2. 数字 `1-9` 在每一列只能出现一次。
3. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

上图是一个部分填充的有效的数独。

数独部分空格内已填入了数字，空白格用 `'.'` 表示。

**示例 1:**

```
输入:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: true
```

**示例 2:**

```
输入:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: false
解释: 除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。
     但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。
```

**说明:**

- 一个有效的数独（部分已被填充）不一定是可解的。
- 只需要根据以上规则，验证已经填入的数字是否有效即可。
- 给定数独序列只包含数字 `1-9` 和字符 `'.'` 。
- 给定数独永远是 `9x9` 形式的。

##### 思路

用三个二维数组去储存行重复，列重复，和9宫格的重复信息，遍历数独。

```c++
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        if (board.empty() || board[0].empty())
            return false;
        bool row[9][9] = {false};
        bool col[9][9] = {false};
        bool sudo[9][9] = {false};
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (isdigit(board[i][j])) {
                    int a = board[i][j]-'1';
                    int k = 3*(i/3)+j/3;
                    if (row[i][a] || col[j][a] || sudo[k][a]) {
                        return false;
                    }
                    row[i][a] = true;
                    col[j][a] = true;
                    sudo[k][a] = true;
                }
            }
        }
        return true;
    }
};
```
