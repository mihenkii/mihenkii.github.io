title: N个数的全排列
date: 2022-04-13 19:45:18
tags: 数据结构与算法
---

问题: 输入一个数N，打印出1到N的全排列
比如输入数字是 3
```
123
132
213
231
312
321
```

#### 思路

想象成老虎机上的N个格子，每个格子都是在进行1-N的循环，并且不能重复。

```
package main

import "fmt"

var (
	num    int
	bucket []int // 格子
	flag   []int // 用来记录1-N的数字是否已经使用，防止重复出现
)

func fill_bucket(n int) {
	if n > num { //当所有格子都填满，打印一次
		pretty_print(bucket)
		return
	}

	for i := 1; i <= num; i++ {
		if flag[i] == 1 {
			continue
		}
		bucket[n] = i
		flag[i] = 1
		fill_bucket(n + 1)
		flag[i] = 0
	}
}

func pretty_print(bucket []int) {
	for _, s := range bucket {
		if s != 0 {
			fmt.Print(s)
		}
	}
	fmt.Println()
}

func main() {
	fmt.Println("please input a number")
	fmt.Scanf("%d", &num)
	bucket = make([]int, num+1) // 用不到第0个元素
	flag = make([]int, num+1)   // 用不到第0个元素
	fill_bucket(1)              // 从左到右遍历
}
```
