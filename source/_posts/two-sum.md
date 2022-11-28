title: two-sum
date: 2022-04-16 17:18:06
tags: 数据结构与算法
---

#### 第一题

很早之前就知道Leetcode的第一题叫two-sum

> 给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。
> 你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。
> 你可以按任意顺序返回答案。
> 示例 1：

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

#### 我的解法

两层for循环, 时间复杂度是O(n2)

```
package main

import "fmt"

func twoSum(arr []int, target int) []int {

	for i := 0; i < len(arr); i++ {
		temp := arr[i]
		if temp >= target {
			continue
		}
		another := target - temp

		for j := len(arr) - 1; j > i; j-- {
			if arr[j] == another {
				return []int{i, j}
			}
		}
	}
	return []int{}
}

func main() {
	s := []int{2, 7, 11, 15}
	ans := twoSum(s, 9)
	fmt.Println(ans)
}

```

#### 别人的思路

构造一个HashMap，通过O(n)的时间复杂度即可找到结果

```
func twoSum(arr []int, target int) []int {
	var table = make(map[int]int)
	for k, v := range arr {
	    if v >= target {
	        continue
	   	}
	    table[v] = k
	    another := target - v
	    if table[another] > 0 {
	        return []int{table[another], k}
	    }
	}
	return []int{}
}
```

##### 思考

开始只考虑从左到右先找到第一个值，然后遍历找到另一个值，思维比较固化

这种一边构造hashmap一边找another的方法, 先找到的是another，然后再hashmap里找另一个。
