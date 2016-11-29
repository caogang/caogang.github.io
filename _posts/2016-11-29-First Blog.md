---
layout: post
title: LeetCode 1.Two Sum
categories: Algorithms
tags: LeetCode
---
# 原题描述：
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution.

Example:

> Given nums = [2, 7, 11, 15], target = 9,  
> Because nums[0] + nums[1] = 2 + 7 = 9,  
  return [0, 1].

# HashMap查找法 O(N)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        // hash[i]表示nums中数值为i的下标
        unordered_map<int, int> hash;
        vector<int> result;

        // 一边循环每个数，一边加入hash表。
        for (int i = 0; i < nums.size(); i++) {
            if (hash.find(target - nums[i]) != hash.end()) {
                // target - nums[i]的下标更小，放在前面
                result.push_back(hash[target - nums[i]]);
                result.push_back(i);
                return result;
            }
            hash[nums[i]] = i;
        }

        // 无解的情况
        result.push_back(-1);
        result.push_back(-1);
        return result;
    }
};
```
