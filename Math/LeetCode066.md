### 题目：

给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

### 难度：

简单。

### 示例：

```
输入：digits = [4,3,2,1]
输出：[4,3,2,2]
解释：输入数组表示数字 4321。
```

### 解题思路：

```
逐位计算。
```

### 代码实现：

```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        //获取数组长度
        int len = digits.size();
        //进位标志
        int flag = 1;
        int temp;
        temp = digits[len - 1] + 1;
        for(int i=len-1; i>=0; i--){
            temp = digits[i] + flag;
            if( temp < 10){
                digits[i] = temp;
                return digits;
            }else{
                digits[i] = temp % 10;
            }
        }
        digits.insert(digits.begin(), 1);
        return digits;
    }
};
```

### 知识点：

简单题我重拳出击。

### 优秀解答：

```
class Solution {
    public int[] plusOne(int[] digits) {
        for (int i = digits.length - 1; i >= 0; i--) {
			if (digits[i] != 9) {
                                digits[i]++;
				return digits;
			} 
			digits[i] = 0;
		}
                //跳出for循环，说明数字全部是9
		int[] temp = new int[digits.length + 1];
		temp[0] = 1;
		return temp;
    }
}
```

### 热评：

```
难道就我一个人看不懂题？
```

