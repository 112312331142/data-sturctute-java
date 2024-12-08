## 七、树

### 7.1 二叉树的三种遍历

#### 1

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left),
 * right(right) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> v;
        stack<TreeNode*> stk;
        TreeNode* p;
        if (root == nullptr) {
            return v;
        }
        stk.push(root);
        while (!stk.empty()) {
            p = stk.top();
            v.push_back(p->val);
            stk.pop();
            if (p->right != nullptr) {
                stk.push(p->right);
            }
            if (p->left != nullptr) {
                stk.push(p->left);
            }
        }
        return v;
    }
};
```

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> stk;
        vector<int> vec;

        TreeNode* cur = root;
        while (cur != nullptr || !stk.empty()) {
            if (cur != nullptr) {
                vec.push_back(cur->val);
                stk.push(cur);
                cur = cur->left;
            } else {
                TreeNode* p = stk.top();
                stk.pop();
                cur = p->right;
            }
        }

        return vec;
    }
};
```

#### 2

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> stk;
        vector<int> vec;
        TreeNode* cur = root;
        while (cur != nullptr || !stk.empty()) {
            if (cur != nullptr) {
                cout << "go:" << cur->val << endl;
                stk.push(cur);
                cur = cur->left;
            } else {
                TreeNode* p = stk.top();
                cout << "come:" << p->val << endl;
                vec.push_back(p->val);
                stk.pop();
                cur = p->right;
            }
        }
        return vec;
    }
};
```

#### 3

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left),
 * right(right) {}
 * };
 */
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> vec;
        stack<TreeNode*> stk;
        TreeNode* cur = root;
        TreeNode* temp;//代表最近一次弹栈的元素
        while (cur != nullptr || !stk.empty()) {
            if (cur != nullptr) {
                stk.push(cur);
                cur = cur->left;
            } else {
                TreeNode* p = stk.top();//栈顶元素
                if (p->right == nullptr || p->right == temp) {//右子树处理完成
                //右子树为null和右子树为最近一次出栈的元素
                    temp = stk.top();
                    stk.pop();
                    vec.push_back(temp->val);
                } else {
                    cur = p->right;//右子树还没处理完成
                }
            }
        }
        return vec;
    }
};
```

#### 4

```java
import java.util.ArrayList;
import java.util.LinkedList;

public class LeetCode145 {
    public static void postOrder(TreeNode root) {
        LinkedList<TreeNode> stack = new LinkedList<>();
        ArrayList<Integer> preArray = new ArrayList<>();
        ArrayList<Integer> inArray = new ArrayList<>();
        ArrayList<Integer> postArray = new ArrayList<>();
        TreeNode curr = root;
        TreeNode pop = null;//最后一次出栈的节点

        while (curr != null || !stack.isEmpty()) {
            if (curr != null) {
                stack.push(curr);
                preArray.add(curr.val);
                curr = curr.left;//处理左子树
            } else {
                TreeNode p = stack.peek();
                if (p.right == null) {//没有右子树
                    inArray.add(p.val);
                    pop = stack.poll();
                    postArray.add(pop.val);
                } else if (p.right == pop) {//右子树处理完毕
                    pop = stack.poll();
                    postArray.add(pop.val);
                } else {//存在待处理右子树
                    inArray.add(p.val);
                    curr = p.right;
                }
            }
        }
        System.out.println(preArray);
        System.out.println(inArray);
        System.out.println(postArray);
    }

    public static void main(String[] args) {
        TreeNode root=new TreeNode(
                new TreeNode(new TreeNode(4),2,null),
                1,
                new TreeNode(new TreeNode(5),3,new TreeNode(6))
        );
        postOrder(root);
    }
}

//[1, 2, 4, 3, 5, 6]
//[4, 2, 1, 5, 3, 6]
//[4, 2, 5, 6, 3, 1]
```

#### 5

 前序遍历：

```cpp
 vector<int> preorderTraversal(TreeNode* root) {
     if(!root) return {};
     vector<int> result;
     stack<TreeNode*> stk;
     stk.push(root);
     while(!stk.empty()){
         TreeNode* node = stk.top();
         stk.pop();
         if(node){
             if(node -> right){
                 stk.push(node -> right);
             }
             if(node -> left){
                 stk.push(node -> left);
             } 
             stk.push(node);
             stk.push(nullptr);
         }else{
             result.push_back(stk.top()->val);
             stk.pop();
         }
     }
     return result;
 }
```

中序遍历：

```cpp
 vector<int> inorderTraversal(TreeNode* root) {
     if(!root) return {};
     vector<int> result;
     stack<TreeNode*> stk;
     stk.push(root);
     while(!stk.empty()){
         TreeNode* node = stk.top();
         stk.pop();
         if(node){
             if(node -> right){
                 stk.push(node -> right);
             }
             stk.push(node);
             stk.push(nullptr);
             if(node -> left){
                 stk.push(node -> left);
             } 
         }else{
             result.push_back(stk.top()->val);
             stk.pop();
         }
     }
     return result;
 }
```

后序遍历：

```cpp
 vector<int> postorderTraversal(TreeNode* root) {
     if(!root) return {};
     vector<int> result;
     stack<TreeNode*> stk;
     stk.push(root);
     while(!stk.empty()){
         TreeNode* node = stk.top();
         stk.pop();
         if(node){
             stk.push(node);
             stk.push(nullptr);
             if(node -> right){
                 stk.push(node -> right);
             }
             if(node -> left){
                 stk.push(node -> left);
             } 
         }else{
             result.push_back(stk.top()->val);
             stk.pop();
         }
     }
     return result;
 }
```

### 7.2 对称二叉树

#### 1

```cpp
public boolean isSymmetric(TreeNode root) {
    return check(root.left, root.right);
}

public boolean check(TreeNode left, TreeNode right) {
    // 若同时为 null
    if (left == null && right == null) {
        return true;
    }
    // 若有一个为 null (有上一轮筛选，另一个肯定不为 null)
    if (left == null || right == null) {
        return false;
    }
    if (left.val != right.val) {
        return false;
    }
    return check(left.left, right.right) && check(left.right, right.left);
}
```

### 7.3 最大深度

#### 1

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root==nullptr){
            return 0;
        }
        return max(maxDepth(root->left),maxDepth(root->right))+1;
    }
};
```

#### 2

```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        stack<TreeNode*> stk;

        TreeNode* curr = root;
        TreeNode* temp;
        int depth = 0;

        while (curr != nullptr || !stk.empty()) {
            if (curr != nullptr) {
                stk.push(curr);
                curr = curr->left;
            } else {
                TreeNode* p = stk.top();
                depth = max(depth, (int)stk.size());
                if (p->right == nullptr || p->right == temp) {
                    temp = stk.top();
                    stk.pop();
                } else {
                    curr = p->right;
                }
            }
        }
        return depth;
    }
};
```

后序遍历，深度优先遍历

#### 3

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left),
 * right(right) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
        queue<TreeNode*> que;
        que.push(root);
        int depth = 0;
        if (root == nullptr) {
            return depth;
        }
        while (!que.empty()) {
            int count = que.size();
            depth++;
            while (count > 0) {
                TreeNode* node = que.front();
                if (node->left != nullptr) {
                    que.push(node->left);
                }
                if (node->right != nullptr) {
                    que.push(node->right);
                }
                que.pop();
                count--;
            }
        }
        return depth;
    }
};
```

层次遍历

### 7.4 最小深度

#### 1

```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        if (root == nullptr) {
            return 0;
        }
        int d1 = minDepth(root->left);
        int d2 = minDepth(root->right);
        if (d1 == 0) {//当右子树为null
            return d2 + 1;//返回左子树深度+1
        }
        if (d2 == 0) {
            return d1 + 1;
        }
        return min(d1, d2) + 1;
    }
};
```

#### 2

```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        // 层序遍历：遇到的第一个叶子节点所在层就是最小深度
        if (root == nullptr) {
            return 0;
        }
        queue<TreeNode*> que;
        que.push(root);
        int depth = 0;
        while (!que.empty()) {
            int count = que.size();
            depth++;
            while (count > 0) {
                TreeNode* node = que.front();
                if (!node->left && !node->right) {
                    return depth;
                }
                if (node->left) {
                    que.push(node->left);
                }
                if (node->right) {
                    que.push(node->right);
                }

                que.pop();
                count--;
            }
        }
        return depth;
    }
};
```



### 7.5 反转二叉树

#### 1

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left),
 * right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == nullptr) {
            return root;
        }
        invert(root);
        return root;
    }

    void invert(TreeNode* node) {
        if (node == nullptr) {
            return;
        }
        swap(node);
        invert(node->left);
        invert(node->right);
    }

    void swap(TreeNode* node) {
        TreeNode* temp = node->left;
        node->left = node->right;
        node->right = temp;
    }
};
```

### 7.5 根据后缀表达式建树

#### 1

```cpp
public static TreeNode constructExpressTree(String[] tokens) {
    Stack<TreeNode> stack = new Stack<>();
    for (String str : tokens) {
        if (str.equals("+") || str.equals("-") || str.equals("*") || str.equals("/")) {
            TreeNode right = stack.pop();
            TreeNode left = stack.pop();
            TreeNode node = new TreeNode(left,str,right);
            stack.push(node);
        } else {
            stack.push(new TreeNode(str));
        }
    }
    return stack.peek();
}
```

### 7.6 从前序与中序遍历序列构造二叉树

[leetcode](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal)

#### 1

```cpp
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if (preorder.empty()) {
            return nullptr;
        }
        int rootValue = preorder[0];
        // 创建根节点
        TreeNode* root = new TreeNode(rootValue);
        // 区分左右子树
        for (int i = 0; i < inorder.size(); i++) {
            if (inorder[i] == rootValue) {
                // 0-i-1 左子树
                vector<int> inLeft;
                inLeft.resize(i);
                copy(inorder.begin(), inorder.begin() + i, inLeft.begin());
                
                // i+1-inOrder.size-1 右子树
                vector<int> inRight;
                inRight.resize(inorder.size() - i - 1);
                copy(inorder.begin() + i + 1, inorder.end(), inRight.begin());

                vector<int> preLeft;
                preLeft.resize(i);
                copy(preorder.begin() + 1, preorder.begin() + 1 + i,
                     preLeft.begin());

                vector<int> preRight;
                preRight.resize(inorder.size() - i - 1);
                copy(preorder.begin() + 1 + i, preorder.end(),
                     preRight.begin());

                root->left = buildTree(preLeft, inLeft);
                root->right = buildTree(preRight, inRight);

                break;
            }
        }
        return root;
    }
};
```

### 7.7 从后序与中序遍历序列构造二叉树

#### 1

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left),
 * right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (postorder.empty()) {
            return nullptr;
        }
        int rootValue = postorder[postorder.size() - 1];
        TreeNode* root = new TreeNode(rootValue);

        for (int i = 0; i < inorder.size(); i++) {
            if (inorder[i] == rootValue) {
                vector<int> inLeft;
                inLeft.assign(inorder.begin(), inorder.begin() + i);
                vector<int> inRight;
                inRight.assign(inorder.begin() + i + 1, inorder.end());
                vector<int> postLeft;
                postLeft.assign(postorder.begin(), postorder.begin() + i);
                vector<int> postRight;
                postRight.assign(postorder.begin() + i, postorder.end() - 1);
                root->left = buildTree(inLeft, postLeft);
                root->right = buildTree(inRight, postRight);
            }
        }
        return root;
    }
};
```

### 7.8 删除二叉搜索树中的节点

#### 1

```cpp
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (root == nullptr) {
            return nullptr;
        }
        TreeNode* dummy = new TreeNode(-1, root, nullptr);
        TreeNode* curr = dummy->left;
        TreeNode* parent = dummy;
        while (curr != nullptr) {
            if (key < curr->val) {
                parent = curr;
                curr = curr->left;
            } else if (key > curr->val) {
                parent = curr;
                curr = curr->right;
            } else {
                break;
            }
        }
        if (curr == nullptr) {
            return root;
        }
        if (curr->right == nullptr && curr->left == nullptr) {
            root = nullptr;
        }
        if (curr->right == nullptr) {
            shift(parent, curr, curr->left);
        } else if (curr->left == nullptr) {
            shift(parent, curr, curr->right);
        } else {
            TreeNode* s = curr->right;
            TreeNode* sParent = curr;
            while (s->left != nullptr) {
                sParent = s;
                s = s->left;
            }
            if (sParent != curr) {
                shift(sParent, s, s->right); // s为要移除的节点
                s->right = curr->right;
            }
            shift(parent, curr, s);
            s->left = curr->left;
        }
        
        return dummy->left;
    }

    void shift(TreeNode* parent, TreeNode* curr, TreeNode* child) {
        if (parent->right == curr) {
            parent->right = child;
        } else if (parent->left == curr) {
            parent->left = child;
        }
    }
};
```

#### 2

```cpp
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (root == nullptr) {
            return nullptr;
        }
        if (key < root->val) {
            root->left = deleteNode(root->left, key);
            return root;
        }
        if (key > root->val) {
            root->right = deleteNode(root->right, key);
            return root;
        }
        if (root->left == nullptr) {
            return root->right;
        }
        if (root->right == nullptr) {
            return root->left;
        }
        TreeNode* s = root->right;
        while (s->left != nullptr) {
            s = s->left;
        }
        s->right = deleteNode(root->right, s->val);
        s->left = root->left;

        return s;
    }
};
```

### 7.9 验证二叉搜索树

#### 1 非递归中序遍历

```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> stk;
        TreeNode* prev = nullptr;
        while (root != nullptr || !stk.empty()) {
            if (root != nullptr) {
                stk.push(root);
                root = root->left;
            } else {
                TreeNode* curr = stk.top();
                stk.pop();
                if (prev != nullptr && curr->val <= prev->val) {
                    return false;
                }
                root = curr->right;
                prev = curr;
            }
        }
        return true;
    }
};
```

#### 2 递归中序遍历

```cpp
class Solution {
public:
    long prev = LONG_MIN;
    bool isValidBST(TreeNode* root) {
        if (root == nullptr) {
            return true;
        }
        bool a = isValidBST(root->left);
        if(!a){
            return false;
        }
        if (prev >= root->val) {
            return false;
        }
        prev = root->val;
        bool b = isValidBST(root->right);
        return a && b;
    }
};
```

#### 3

```cpp
class Solution {
public:
    // 上下限递归实现
    bool isValidBST(TreeNode* root) {
        return doValid(root, LONG_MIN, LONG_MAX);
    }

    bool doValid(TreeNode* node, long min, long max) {
        if (node == nullptr) {
            return true;
        }
        if (node->val <= min || node->val >= max) {
            return false;
        }
        return doValid(node->left, min, node->val) &&
               doValid(node->right, node->val, max);
    }
};
```

### 7.10 根据前序遍历构造二叉搜索树

#### 1

```cpp
class Solution {
public:
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        TreeNode* root = new TreeNode(preorder[0]);
        TreeNode* curr = root;
        for (int i = 1; i < preorder.size(); i++) {
            int val = preorder[i];
            insert(root, val);
        }
        return root;
    }

private:
    TreeNode* insert(TreeNode* node, int val) {
        if (node == nullptr) {
            return new TreeNode(val);
        }
        if (val < node->val) {
            node->left = insert(node->left,val);
        }else{
            node->right = insert(node->right,val);
        }
        return node;
    }
};
```

#### 2

```cpp
class Solution {
public:
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        /*
        1.遍历数组中的每个值，根据值创建节点
            - 每个节点若成功创建都有，左孩子上限，右孩子上限
        2.处理下一个值时，如果超过此上限，那么：
            - 将null作为上个节点的孩子
            - 不能创建节点对象
            - 直到不超过上限为止
        3.重复1，2两步
        */
        return insert(preorder,INT_MAX);
    }

private:
    int i = 0;
    TreeNode* insert(vector<int>& preorder, int max) {
        if (i == preorder.size()) {
            return nullptr;
        }
        int value = preorder[i];
        if (value > max) {
            return nullptr;
        }
        TreeNode* node = new TreeNode(preorder[i]);
        i++;
        node->left = insert(preorder, value);
        node->right = insert(preorder, max);
        return node;
    }
};
```

