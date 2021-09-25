### 题目：

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中。
pop() —— 删除栈顶的元素。
top() —— 获取栈顶元素。
getMin() —— 检索栈中的最小元素。

### 难度：

简单。

### 示例：

```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

### 解题思路：

```c++
初始思路：
    使用两个栈来实现Minstack。
    一个栈用来存放所有数据，一个栈用来存放当前最小值。

改进思路：
    使用一个栈存放数据。
    每次存放时存放两个数据。当前数据+当前最小值。

```

### 代码实现：

```c++
class MinStack {
private:
    //初始化两个栈
    stack<int> minVal;
    stack<int> value;
    int minNum;
public:
    /** initialize your data structure here. */
    //初始化，minNum赋值为零。
    MinStack():minNum(0){

    }
    //入栈
    void push(int val) {
        //如果是空，初始化minNum。
        if(value.empty()){
            minNum = val;
        }
        value.push(val);
        //cout << "Value入栈" << val << endl;
        if(val <= minNum){
            minVal.push(val);
            minNum = minVal.top();
            //cout << "minVal入栈" << val << endl;
        }
        //测试
        //cout << minVal.size() << endl;
        return ;
    }
    //出栈
    void pop() {
        int temp = value.top();
        value.pop();
        //cout << "Value出栈" << temp << endl;
        if(temp == minNum){
            minVal.pop();
            if(!minVal.empty()){
                minNum = minVal.top();
            }
            //cout << "minVal出栈" << temp << endl;
        }
        return ;
    }
    //获取栈顶元素
    int top() {
        //cout << "value.top()" << value.top() << endl;
        return value.top();
    }
    
    int getMin() {
        //cout << "minVal.top()" << minVal.top() << endl;
        return minVal.top();
    }

};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(val);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

### 知识点：

因为实际上还是直接调用了C++提供的stack，所以实现起来很简单。

### 官方代码：

```c++
class MinStack {
    stack<int> x_stack;
    stack<int> min_stack;
public:
    MinStack() {
        min_stack.push(INT_MAX);
    }
    
    void push(int x) {
        x_stack.push(x);
        min_stack.push(min(min_stack.top(), x));
    }
    
    void pop() {
        x_stack.pop();
        min_stack.pop();
    }
    
    int top() {
        return x_stack.top();
    }
    
    int getMin() {
        return min_stack.top();
    }
};
```

### 热评：

```
面试的时候被问到不能用额外空间，就去网上搜了下不用额外空间的做法。思路是栈里保存差值。
```

