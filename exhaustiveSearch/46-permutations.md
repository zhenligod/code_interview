---
title: 46-permutations
tags: 在河之洲,算法,小书匠
grammar_cjkRuby: true
---

# problem
[46-permutations](https://leetcode.com/problems/permutations/#/description)

# solution

## 传统回溯法

使用回溯法解题的关键在于如何确定正确解及排除不符条件的解(**剪枝**)。

construct_candidate 里可以使用hashmap来使得查找的方法从**O(n)** 降低到 **O(1)**
```cpp
vector<vector<int>> result;
const int MaxCandidates = 10000;
vector<vector<int>> permute(vector<int>& nums) {
	if (nums.empty())
		return result;
	vector<int> a(nums.size(), -1);
	backTracking(a, 0, nums);
	return result;
}
void backTracking(vector<int> &a, int k, vector<int>& nums)
{
	//定义候选集合;
	int C[MaxCandidates];
	int candidates;
	if (k == nums.size())
		process_solution(a, nums);
	else
	{
		constructCandidates(k, a, C, &candidates);
		for (int i = 0; i < candidates; i++)
		{
			a[k] = C[i];
			backTracking(a, k + 1, nums);
		}
	}

}
void constructCandidates(int k, vector<int> &a, int C[], int * cand)
{
	*cand = a.size() - k;
	int j = 0;
	for (int i = 0; i < a.size(); i++)
	{
		if (find(a.begin(), a.begin() + k, i) == a.begin() + k)
			C[j++] = i;
	}
}
void process_solution(vector<int>& a, vector<int>& nums)
{
	vector<int> list(a.size());
	for (int i = 0; i < a.size(); i++)
	{
		list[a[i]] = nums[i];
	}
	result.push_back(list);
}
```
### 算法流程

对于每一个元素随机找一个它可能的取值，然后找下一个元素可能的取值，知道遍历完所有的元素，然后再回退去找上一个访问元素的其他可能取值。

### 算法复杂度分析

时间复杂度，共有n!中可能取值。时间复杂度为o(n x n!)，空间复杂度为o(n! + 3n)

### 使用比较简单的subsets模板

```cpp
vector<vector<int>> result;
vector<vector<int>> permute(vector<int>& nums) {
	if (nums.empty())
		return result;
	vector<int> list;
	backTracking(nums, list);
	return result;
}
void backTracking(vector<int> & nums, vector<int> &list)
{
    if (list.size() == nums.size() )
    {
        result.push_back(list);
        return;
    }
    for (int i = 0; i < nums.size(); i++)
    {
        if (find(list.begin(), list.end(), nums[i]) == list.end() )
        {
            list.push_back(nums[i]);
            backTracking(nums, list);
            list.pop_back();
        }
    }
}
```

### 代码分析

代码比较短，但是时间复杂度跟上面的是一样的。

# reference
[全面解析回溯法：算法框架与问题求解](http://www.cnblogs.com/wuyuegb2312/p/3273337.html)

[leetcode解析permutations](https://algorithm.yuanbin.me/zh-hans/exhaustive_search/permutations.html)