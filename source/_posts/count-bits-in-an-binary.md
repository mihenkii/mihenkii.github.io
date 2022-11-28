title: count-bits-in-an-binary
date: 2022-11-28 19:25:36
tags: 数据结构与算法
---

#### 统计一个二进制数里面1的个数

[原题](http://www.robalni.org/posts/20220428-counting-set-bits-in-an-interesting-way.txt)

> This is not the fastest but maybe the most interesting way to count
> how many bits are 1 in a binary number.

```
输入：3
输出：2
解释：3的二进制是101， 里面有2个1
```

#### 解法
这个文章里面的算法不太通用，主要用了2个特性：
1. 整数的除法，会向下取整。 比如 7/2结果是3
2. x/2 + x/4 + x/8... == x

向右移位，刚好等于除以2.  满足上面第二个特性.
x - (x/2 + x/4 + x/8..) 在float情况下，应该刚好等于0，
但是在整型的情况下，就等于被向下取整的1的总和，也就是1的个数.

```
package main

import (
	"fmt"
)

func countbit(num int) int {
	temp := num
	sum := 0
	for temp > 0 {
		temp = temp >> 1
		sum += temp
	}
	return num - sum
}

func main() {
	fmt.Println(countbit(11))
}

```
#### 普通解法

将数据的最后一位求和

#### 总结
原作者给了如下解释。我觉得这个题比较巧，不太通用，意义不是特别大
> The interesting thing about this algorithm is that we calculate
> something that we can't calculate directly, by calculating everything
> except for that, and then looking at the difference.






