## 50. Pow(x, n)
Implement pow(x, n).

#### Solution
1. Solution 1: Recursive
2. Solution 2: Iterative

**切记注意考虑边界情况，和n<=0的情况**

Corner case when n equals to Integer.MIN_VALUE.
~~~~
8.88023
3
8.88023
-4
8.88023
0
-1.00000
-2147483648
1.00000
-2147483648
2.00000
-2147483648
~~~~

~~~
public class Math {

  // 50. Pow(x, n)
  // Solution 1: Recursive
  public double myPow(double x, int n) {
        if (n == 0) return 1;
        if (n == 1) return x;
        if (n == -1) return 1 / x;

        int m = n / 2;
        double res = myPow(x, m);
        res *= res;
        if (n < 0) {
            x = 1 / x;
        }
        return (n & 1) == 1 ? x * res : res;
  }

  // Solution 2: Iterative
  public double myPow(double x, int n) {
       if (n == 0) {
           return 1;
       }

       long m = (long) n ;
       if (n < 0) {
           m = -m;
           x =  1 / x;
       }
       double ans = 1;
       while (m > 0) {
           if (m % 2 == 1) {
               ans *= x;
           }
           x = x * x;
           m = m / 2;
       }
       return ans;
   }
}
~~~

~~~
大哥 pow(x, n) 先讨论输出可能是什么类型 我说你的expected time是什么 线性的话就直接做 否则需要递归~  有哪些异常 要抛出异常 然后要抛出哪种异常  写好 大哥说ok
follow up: 问当n小于零的时候 也可以直接变成 算pow(1/x, abs(n)) 为什么不这么做？ 楼主说 这样recursive 你就要重新创造function 不能用本身 大哥说还有吗 楼主说可能溢出？ 大哥说我们不考虑溢出 楼主蒙了几秒 反应说这样精度会缺失  大哥表示满意 我说float double 表示都不太精确 然后国人姐姐追问这两者区别 小数在计算机里面的存储
~~~

## 69. Sqrt(x)
Implement int sqrt(int x).

Compute and return the square root of x.

#### Solution
Binary Search <br>
1. corner cases, x <= 1, return x
2. 二分查找的时候如果当前m < x / m, 检查是否满足 m + 1 > x / m + 1, 如果满足的话直接返回

~~~
public class Solution {
    public int mySqrt(int x) {
        if (x <= 1) return x;
        int i = 1;
        int j = x / 2;
        while (true) {
            int m = (j - i) / 2 + i;
            if (m == x / m) {
                return m;
            }
            else if (m > x / m) {
                j = m - 1;
            }
            else {
                if (m + 1 > x / (m + 1))
                    return m;
                i = m + 1;
            }
        }
    }
}
~~~

## Greatest Common Factor

~~~
public int gcf(int a, int b) {
  if (b == 0) return a;
  else return gcf(b, a % b);
}
~~~
