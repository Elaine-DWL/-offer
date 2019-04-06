## 题目描述

小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!

输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序。

## 解题思路

### 思路一【暴力】

一开始只能想到2层循环的暴力来做。

### 思路二【双指针】

看了讨论区，可以注意以下几点：

* 区间求和不需要遍历，直接用等差数列求和公式（$\frac{n(a_1+a_n)}{2}$）来做
* 这个连续序列中，最右边那个数也就是最大的那个数必定<= $\frac{sum+1}{2}$

使用两个指针来确定区间，然后用等差数列求和公式求出该区间所有数的和sum。

1. sum == target。将序列记录起来，low++;
2. sum < target。high++;
3. sum > target。low++;

```cpp
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {// 这里的sum就是 上面分析中的target
        // 找连续的序列 和刚好等于sum
        vector<vector<int>> res;
        int low=1, high=2;
        while(low<high && high <=(sum+1)/2){
            int cur_sum = (high-low+1)*(low+high)/2;
            if(cur_sum == sum){
                vector<int> temp;
                for(int i=low; i<=high; i++) temp.push_back(i);
                res.push_back(temp);
                low++;
            }
            else if(cur_sum < sum) high++;
            else low++;
        }
        return res;
    }
};
```

