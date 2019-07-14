## 题目描述

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。 

## 解题思路

### 思路一

使用选择排序，时间复杂度是$O(kn)$.

需要注意的情况是：当k大于数组大小时，应该返回一个空向量。

```cpp
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        // 思路一 选择排序
        vector<int> res;
        if(k>input.size()||k<0) return res;
        for(int i=0; i<k; i++){
            int cur_min = input[i];
            int cur_index = i;
            int j=i;
            for(j=i+1; j<input.size(); j++){
                if(input[j] < cur_min){
                    cur_min = input[j];
                    cur_index = j;
                }
            }
            if(cur_min < input[i]) swap(input[cur_index], input[i]);
            res.push_back(input[i]);
        }
        return res;
    }
};
```

### 思路二

使用快速排序，当确定好位置k或者k-1处的数后，对前面[0, k-1]位置处的数再进行快排。

时间复杂度是

```cpp
class Solution {
public:
    int partition(vector<int>& nums, int l, int r){// 确定r位置处元素的正确位置并返回
        while(l<r){
            while(l<r && nums[l] <= nums[r]) l++;
            if(l<r) swap(nums[l], nums[r]);
            while(l<r && nums[r] >= nums[l]) r--;
            if(l<r) swap(nums[r], nums[l]);
        }
        return l;
        
    }
    void quicksort(vector<int>& nums,int l, int r, int k){
        if(l>r) return;
        int ipovt = partition(nums, l, r);
        if(ipovt==k) quicksort(nums, 0, k-1, k);
        else{
            quicksort(nums, l, ipovt-1, k);
            quicksort(nums, ipovt+1, r, k);
        }
    }
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        if(k>input.size() || k<0) return {};
        quicksort(input, 0, input.size()-1, k);
        vector<int> res;
        for(int i=0; i<k; i++){
            res.push_back(input[i]);
        }
        return res;
    }
};
```

### 思路三

堆排序。构建最小堆的时间复杂度是O(n)，取前k个最小的元素是O(klogn)。

```cpp
class Solution {
public:
    void adjust(vector<int> &nums, int idx, int tail){
        // 从idx向下调整
        int t_min = idx;
        int left = 2*idx+1, right=2*idx+2;
        if(left <= tail && nums[left] < nums[t_min]) t_min = left;
        if(right <= tail && nums[right] < nums[t_min]) t_min = right;
        if(idx!=t_min){
            swap(nums[idx], nums[t_min]);
            adjust(nums, t_min, tail);
        }
    }
    vector<int> GetLeastNumbers_Solution(vector<int> nums, int k) {
        // 最小的k的数   用最小堆实现
        // 构建最小堆 从最后一个非叶子结点开始 向上进行调整
        int n = nums.size();
        if(k >n) return {};
        // 构建小顶堆
        for(int i=(n-2)/2; i>=0; i--){
            adjust(nums, i, n-1);
        }
        vector<int> res;
        int tail = n-1;
        while(k>0){
            res.push_back(nums[0]);
            swap(nums[0], nums[tail--]);
            adjust(nums, 0, tail);
            k--;
        }
        return res;
    }
};
```

