---
layout: post
title: leetcode-001-Two Sum
date: 2018-05-31 22:20：01 +08:00
category:
    - leetcode
keywords: C++ leetcode
tags:
    - leetcode
---

# 题目

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:

>Given nums = [2, 7, 11, 15], target = 9,
>
>Because nums[0] + nums[1] = 2 + 7 = 9,
>return [0, 1].

# 方法

目前有两种解决方法，如下

## 方法一

通过双重循环来找到满足条件的数据索引，实现代码如下

```C++
#include <iostream>
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int i = 0, j = 0;
        vector<int> result;
        for (i = 0; i < nums.size() - 1; i++)
        {
            for (j = i + 1; j < nums.size(); j++)
            {
                if (nums[i] + nums[j] == target)
                {
                    result.push_back(i);
                    result.push_back(j);
                    cout << i << j << endl;
                    break;
                }
            }
        }
        return result;

    }
};

int main() {
    Solution slt;
    vector<int> nums;
    int target = 9;

    nums.push_back(2);
    nums.push_back(7);
    nums.push_back(11);
    nums.push_back(15);
    slt.twoSum(nums, target);

    return 0;
}
```

上述方法由于使用了双重循环，因此时间复杂度为$$O(n^2)$$, 以下介绍一种改进的方法

## 方法二

```C++
#include <iostream>
#include <vector>
#include <unordered_map>
using namespace std;

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int>  m;
        for (int i = 0; i < nums.size(); ++i) {
            int second = target - nums[i];
            if (m.find(second) != m.end()) {
                cout << m[second] << " " << i << endl;
                return vector<int>({m[second], i});
            } else {
                m.insert(pair<int, int>{nums[i],i});
            }
        }
        return vector<int>();
    }
};

int main() {
    Solution slt;
    vector<int> nums;
    int target = 9;

    nums.push_back(2);
    nums.push_back(7);
    nums.push_back(11);
    nums.push_back(15);
    slt.twoSum(nums, target);

    return 0;
}
```

其中涉及到C++中vector和unordered_map的使用，后续再介绍
