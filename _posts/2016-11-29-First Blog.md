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
读完题最先想到的是普通的两层遍历法，但是其时间复杂度太高为O(N^2)。所以我们可以采用把逐个比较转变为直接查找，很自然的就想到了HashMap的查找时间为O(1)，这是一种典型的空间换时间的方案以O(N)的空间换取O(N)的时间。  
具体算法如下：  
1. 首先从头到尾遍历整个数组  
2. 每遍历一个元素$ele$，在Hash表中查找$target-ele$元素  
3. 若成功找到，则返回$vector = [index\ of\  target-ele, index\ of\ ele]$  
4. 若没有找到，以$ele$作为Key，以$index\ of\ ele$作为Value存入Hash表，继续遍历  
5. 若遍历完成后还没有返回，则返回$vector = [-1, -1]$作为无解输出  


## C++代码如下：

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

> Tips:  
> 这里采用的是unordered\_map而不是map
> 原因如下：  
> map的实现是基于红黑树的，对于数据具有自动排序的功能，但是效率相比于unordered\_map较差，因为unordered\_map是基于hash函数的  

------------

## Bug Free Procedure  
**2016.11.29 编译错误**  
1. unordered_map类的定义应该为`unordered_map<int, int> hash_map`而不是`unordered_map<int> hash_map`  
2. unordered_map查找Key的函数应该为`hash_map.find(target) != hash_map.end()`  
3. 遍历数组的时候index应该是从`0~nums.size()-1`，而不是`hash_map.size()-1`  
4. 返回vector时，应该先`push_back(hash_map[target-nums[i]])`，再`push_back(i)`  

