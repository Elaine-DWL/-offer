## 题目描述

输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。 

## 解题思路

### 思路一

直接暴力，$O(n^2)$。

### 思路二

k-sum类问题中最简单的2-sum问题。这里题目中要求所得出的两个树数的乘积最小。使用两个指针初始化在数组的两头，最先找到的符合要求的2个数应该是相差最大的，它们的乘积肯定也是满足条件的所有数对中最小的。

```cpp
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int> array,int sum) {
        // 2sum类问题
        int n=array.size();
        if(n<2) return {};
        vector<int> res(2, 0);
        // int flag=0;   // 刚开始还准备找出所有的数对来比较乘积大小   实际上并不用
        int l=0, r=n-1;
        while(l<r){
            if(array[l]+array[r] > sum) r--;
            else if(array[l]+array[r] < sum) l++;
            else{
                    res[0] = array[l];
                    res[1] = array[r];
                    return res;
            }
        }
        return {};
    }
};
```



