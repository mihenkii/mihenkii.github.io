title: fibonacci
date: 2022-04-08 17:49:04
tags: 数据结构与算法
---

fibonacci数列: 1 1 2 3 5 8 13 21...

今天看到了fibonacci的非递归写法

```
func fibonacci(n int) int {
	a, b := 0, 1
	for i := 2; i < n; i++ {
		a, b = b, a+b
	}
	return a + b
}

func fibonacci(n int) int {
	a, b := 0, 1
	for i := 2; i < n; i++ {
        // 有的编程语言不支持上面的那种a, b = b, a+b写法
        sum := a + b
        a = b
        b = sum
	}
	return a + b
}
```

递归写法
```
func fibonacci(n int) int {
    if n == 1 || n == 2 {
        return 1
    }
    return fibonacci(n-1) + fibonacci(n-2)
}

```
