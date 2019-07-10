## 题目描述

给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）    中，按结点数值大小顺序第三小结点的值为4。

## 解题思路

### 思路一

一般二叉搜索树相关的题目基本都是在中序遍历的基础上进行修改。

```cpp
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};
*/
class Solution {
public:
    TreeNode* inorder(TreeNode *root,  int &k){
        if(root==nullptr) return nullptr;
        TreeNode * left = inorder(root->left, k);
        if(left) return left;
        k--;
        if(k==0){
            return root;
        }
        TreeNode *right = inorder(root->right, k);
        if(right) return right;
        return nullptr;
    }
    TreeNode* KthNode(TreeNode* pRoot, int k)
    {
        // 借助中序遍历  中序遍历的第k个结点就是
        return inorder(pRoot, k);
    }
};
```

另外一种写法：

```cpp
class Solution {
public:
    int count = 0;
    TreeNode* KthNode(TreeNode* pRoot, int k)
    {
        if(pRoot){
            TreeNode *left = KthNode(pRoot->left, k);
            if(left) return left;
            count++;
            if(count==k) return pRoot;
            TreeNode *right = KthNode(pRoot->right, k);
            if(right) return right;
        }
        return nullptr;
    }
};
```

