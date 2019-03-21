## 题目描述

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。 

## 解题思路

### 思路一

时间复杂度要求是常量，借助一个辅助栈(B)来实现，主栈记为A。

出栈入栈操作的时候，栈A正常地进行。

入栈时，如果B为空栈或者新的元素小于B的栈顶元素，那么压入B栈，否则不对B栈做处理；

出栈时，如果A栈顶元素和B栈顶元素相等，那么B同A一样，也弹栈。

需要返回栈A的最小元素时，直接返回栈B的栈顶元素。

```cpp
class Solution {
public:
    stack<int> s, mins;// mins是辅助栈
    void push(int value) {
        if(mins.empty() || value<=mins.top())
            mins.push(value);
         s.push(value);
    }
    void pop() {
        if(s.top() == mins.top())
            mins.pop();
        s.pop();
    }
    int top() {
        return s.top();
    }
    int min() {
        return mins.top();
    }
};
```

