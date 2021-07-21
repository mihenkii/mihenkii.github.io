title: number-is-prime
date: 2021-07-16 16:34:38
tags: 数据结构与算法
---

#### 问题
如何判断一个数字是否是素数

#### 思路

普通方法：从2..√n，如果n可以被整除， 那么n就不是素数

更高效方法:
> We can improve this method further. Observe that all primes greater than 3 are of the form 6k ± 1, where k is any integer greater than 0. This is because all integers can be expressed as (6k + i), where i = −1, 0, 1, 2, 3, or 4. Note that 2 divides (6k + 0), (6k + 2), and (6k + 4) and 3 divides (6k + 3). So, a more efficient method is to test whether n is divisible by 2 or 3, then to check through all numbers of the form 6k +-1. This is 3 times faster than testing all numbers up to √n.

所有整数都可以用6k + i表示，其中i = -1, 0, 1, 2, 3, 4，其中6k + 0, 6k + 2, 6k + 4可以被2整除， 6k + 3 可以被3整除.
所以，素数一定是包含在 6k-1 或者 6k+1里

#### 代码

```
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

bool is_prime(int n) {

    if(n < 2) {
        printf("Input number must greater than 2 what we got is %d\n", n);
        exit(-1);
    }

    if(n == 2 || n == 3) {
        return true;
    }

    if(n%2 == 0 || n %3 == 0) {
        return false;
    }

    for (int i=5; i*i<n; i+=6) {
        if(n % i == 0 || n % (i+2) == 0) {
            return false;
        }
    }

    return true;
}

int main() {
    int n;
    printf("please input a number: ");
    scanf("%d", &n);

    if (is_prime(n)) {
        printf("%d is prime number", n);
        return 0;
    } else {
        printf("%d is not prime number", n);
        return 1;
    }
}

```

#### 参考链接

https://alexanderell.is/posts/rpc-from-scratch/
https://en.wikipedia.org/wiki/Primality_test
