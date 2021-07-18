### 题目：

给你一个整数 x ，如果 x 是一个回文整数，返回 true ；否则，返回 false 。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。例如，121 是回文，而 123 不是。

### 难度：

简单。

### 示例：

```
输入：x = 121
输出：true
输入：x = -121
输出：false
解释：从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

### 解题思路：

```
本来第一反应是转换为字符串不久立马解决了嘛。结果要求是最好不要转换为字符串。
1.负数直接返回false，个位数直接返回True。
2.其余正数（number >= 10)判断最高位和最低位是否相等，循环处理。
3.可以把整数的每一位通过求余操作存到向量中去。

或者也可以将整数反转：
1.如果溢出，直接返回false。
2.如果不溢出，判断是否相等，不相等，返回false，相等返回false。
```

### 代码实现：

```c++
class Solution {
public:
    bool isPalindrome(int x) {
        //负数直接返回false，大于等于0小于十的数字直接返回。
        if(x<0){
            return false;
        }else if((x >= 0)&&(x < 10)){
            return true;
        }
        //用来存储反转后的整数
        long n = 0;
        int x_copy = x;
	    while (x_copy){
	    	n = n * 10 + x_copy % 10;
		    x_copy /= 10;
	    }
        //注意判别顺序。
	    if((n > INT_MAX )||((int)n != x)){
            return false;
        }
        return true; 
        
    }
};
```

### 知识点：

观看官方解答可以看到使用了更为巧妙的反转一半数字的做法，避免了溢出情形。

### 官方代码：

```
class Solution {
public:
    bool isPalindrome(int x) {
        // 特殊情况：
        // 如上所述，当 x < 0 时，x 不是回文数。
        // 同样地，如果数字的最后一位是 0，为了使该数字为回文，
        // 则其第一位数字也应该是 0
        // 只有 0 满足这一属性
        if (x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }

        int revertedNumber = 0;
        while (x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }

        // 当数字长度为奇数时，我们可以通过 revertedNumber/10 去除处于中位的数字。
        // 例如，当输入为 12321 时，在 while 循环的末尾我们可以得到 x = 12，revertedNumber = 123，
        // 由于处于中位的数字不影响回文（它总是与自己相等），所以我们可以简单地将其去除。
        return x == revertedNumber || x == revertedNumber / 10;
    }
};

```

### 热评：

```
简单的题效率完全比不上题解，复杂的题完全不会 -.-
```

```
回文数不会溢出。
```

