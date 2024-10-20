## 一、排序

### 1.1 冒泡排序

#### 1

```java
public static void sort(int[] arr) {
    recursion(arr, arr.length);
}

public static void recursion(int[] arr, int len) {
    if (len == 1) {
        return;
    }
    int x = 0;
    for (int i = 0; i < len - 1; i++) {
        if (arr[i] > arr[i + 1]) {
            swap(arr, i, i + 1);
            x = i;
        }
    }
    recursion(arr, x+1);
}
```

### 1.2 选择排序



### 1.3 插入排序

#### 1

```java
 public static void sort(int[] a) {
        insertion(a, 1);
    }

 private static void insertion(int[] a, int low) {
     if (low == a.length) {
         return;
     }
     int temp = a[low];
     int i = low;
     for (i = low; i > 0 && a[i - 1] > temp; i--) {
         a[i] = a[i - 1];
     }
     a[i] = temp;

     insertion(a, ++low);
 }
```



## 二、查找

### 2.1 二分查找

#### 1

二分查找普通版：

```java
public static int binarySearchNormal(int[] arr, int target) {
    int l = 0;
    int r = arr.length - 1;
    while (l <= r) {
        int mid = l + ((r - l) >> 1);
        if (target < arr[mid]) {
            r = mid - 1;
        } else if (target > arr[mid]) {
            l = mid + 1;
        } else {
            return mid;
        }
    }
    return -(l + 1);
}
```

#### 2

在数组中有重复元素，求数组的最左（右）边目标i元素时，要对代码的if条件进行更改

```java
public static int binarySearchLeftMost2(int[] arr, int target) {
    int l = 0;
    int r = arr.length - 1;
    int index = -1;
    while (l <= r) {
        int mid = l + ((r - l) >> 1);
        if (target < arr[mid]) {
            r = mid - 1;
        } else if (target > arr[mid]) {
            l = mid + 1;
        } else {
            index = mid;
            r = mid - 1;
        }
    }
    return index;
}
```

在找到指定元素时，将不能直接返回索引，需要先添加补充一个变量存储该元素的索引，将有边界减小为mid-1（与目标元素的在中点的左边所处的情况一致），只是多了一步存储操作

#### 3

在边界的值无法值无法直接不参与查找时，循环的边界条件要进一步更改

```java
public static int binarySearch(int[] a, int target) {
    int i = 0, j = a.length;
    while (i < j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {			// 在左边
            j = m;
        } else if (a[m] < target) {		// 在右边
            i = m + 1;
        } else {
            return m;
        }
    }
    return -1;
}
```

上述代码的有边界j所处的值无法取到，所以在二分时j不能在中点的基础上减一；



## 三、链表

### 3.1 反转链表

#### 1

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode newList=null;
        for(ListNode node=head;node!=null;node=node.next){
            newList=new ListNode(node.val,newList);
        }
        return newList;
    }
}
```

创建一个新链表，依次将创建新节点，将遍历到节点的值赋予它，并将它的下一个节点指针指向头节点，最后将该节点变为新的头节点，进行头插法插进newList中

#### 2

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 * int val;
 * ListNode next;
 * ListNode() {}
 * ListNode(int val) { this.val = val; }
 * ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
// 方法2
class Solution {
    public ListNode reverseList(ListNode head) {
        List list1 = new List(head);
        List list2 = new List(null);
        while (true) {
            ListNode first = list1.removeFirst();
            if (first == null) {
                break;
            }
            list2.addFirst(first);
        }
        return list2.head;

    }

    static class List {
        ListNode head;

        public List(ListNode head) {
            this.head = head;
        }

        public void addFirst(ListNode node) {
            node.next = head;
            head = node;
        }

        public ListNode removeFirst() {
            ListNode first = head;
            if (first != null) {
                head = first.next;
            }
            return first;
        }
    }
}
```

与方法1 类似，构造一个新链表，从**旧链表头部**移除节点，添加到**新链表头部**，完成后新链表即是倒序的，区别在于原题目未提供节点外层的容器类，这里提供一个，另外一个区别是并不去构造新节点

list1包装旧链表，list2包装新链表

#### 3

递归解决

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head.next==null){
            return head;//找到最后节点返回
        }
        ListNode last=reverseList(head.next);
        return last;
    }
}
```

上述代码是找到该链表的最后一个节点并返回，作为新链表的头节点

下面是伪代码的调用流程：

```java
reverseList(ListNode p = 1) {
    reverseList(ListNode p = 2) {
    	reverseList(ListNode p = 3) {
    		reverseList(ListNode p = 4) {
    			reverseList(ListNode p = 5) {
    				if (p == null || p.next == null) {
                        return p; // 返回5
                    }
				}
                // 此时p是4, p.next是5
			}
            // 此时p是3, p.next是4
		}
        // 此时p是2, p.next是3
	}
    // 此时p是1, p.next是2
}
```

根据上述伪代码的流程，在出递归的时候要让当前指针的指向的指针指向自己

```JAVA
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null||head.next == null) {
            return head;// 找到最后节点返回
        }
        ListNode last = reverseList(head.next);
        head.next.next = head;
        head.next=null;
        return last;
    }
}
```

head.next.next=head;

当前指针必须指向null，这是防止在最后一次递归时，两个指针互相指向，而不能向前几次递归那样，后面的递归可以帮前一个指针更改指向

#### 4

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode o1 = head;
        ListNode o2 = head.next;
        while (o2 != null) {
            o1.next = o2.next;//将o1指向o2的下一个节点，使o2从链表中断开，
            o2.next = head;//将o2插在头节点的前方
            head = o2;//o2所处的值变为新的头节点
            o2 = o1.next;//在每次循环结束后，o1的下一个节点一定是o2；
        }
        return head;
    }
}
```

#### 5

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev=null;
        ListNode curr=head;
        while(curr!=null){
            ListNode temp=curr.next;
            curr.next=prev;
            prev=curr;
            curr=temp;
        }

        return prev;
    }
}
```

与方法4不同的是，该方法是直接更改节点的指向，最后返回的是原链表的尾节点



### 3.2 删除倒数第n个节点

#### 1

```C++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* s = new ListNode(0, head);
        ListNode* slowNode = s;
        ListNode* fastNode = s;

        for (int i = 0; i < n; i++) {
            fastNode = fastNode->next;
        }
        while (fastNode->next != nullptr) {
            slowNode = slowNode->next;
            fastNode = fastNode->next;
        }
        slowNode->next = slowNode->next->next;
        return s->next;
    }
};
```

快慢指针，先让快指针与慢指针的差距变为n，随后快指针的下一个节点遍历到null，此时慢指针就是倒数n的位置

#### 2

```C++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* s=new ListNode(0,head);
        recursion(s, n);
        return s->next;
    }

    int recursion(ListNode* p, int n) {
        if (p == nullptr) {
            return 0;
        }

        int nth = recursion(p->next, n);
        if (nth == n) {
            p->next = p->next->next;
        }
        return nth + 1;
    }
};
```

递归，在当p指向为空时，返回0。

当nth的值与n的值相等时，代表要删除的节点找到，此时p所指向的为要删除节点的前一个节点，删除下一个节点

Q：p.next.next 不怕空指针吗？

A：

- p 是待删除节点的上一个节点，如果能递归回到 p，那么 p.next 肯定有值，不会是 null
- 且题目说明了 n >=1，不会因为 nth == 0 而让 p.next 指向最后的 null

### 3.3 删除排序链表中的重复元素（只留下不重复的元素）

#### 1

```C++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {

        if (head == nullptr || head->next == nullptr) {
            return head;
        }
        if (head->val == head->next->val) {
            ListNode* x = head->next->next;
            while (x != nullptr && x->val == head->val) {
                x = x->next;
            }
            return deleteDuplicates(x);
        }

        head->next = deleteDuplicates(head->next);
        return head;
    }
};
```

### 3.4 合并多个有序链表

#### 1

```C++
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        return merge(lists, 0, lists.size()-1);
    }

    ListNode* merge(vector<ListNode*>& lists, int l, int r) {
        if (l == r) {
            return lists[l];
        } else if (l > r) {
            return nullptr;
        }
        int m = l + ((r - l) >> 1);
        return mergeTwoLists(merge(lists, l, m), merge(lists, m + 1, r));
    }

    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        if (!list1 || !list2) {
            return list1 ? list1 : list2;
        }
        ListNode* head = new ListNode(-1);
        ListNode* tail = head;
        while (list1 && list2) {
            if (list1->val >= list2->val) {
                tail->next = list2;
                list2 = list2->next;
            } else {
                tail->next = list1;
                list1 = list1->next;
            }
            tail = tail->next;
        }
        tail->next = list1 ? list1 : list2;

        return head->next;
    }
};
```

分治法

#### 2

小根堆 

```C++
class MinHeap {
public:
    vector<ListNode *> v;
    int size = 0;

    explicit MinHeap(int capacity) {
        v.resize(capacity);
    }

    bool empty() const {
        return size == 0;
    }

    bool full() const {
        return size == v.capacity();
    }

    bool put(ListNode *node) {
        if (full()) {
            return false;
        }
        int child = size++;
        int parent = (child - 1) / 2;
        while (child > 0 && v[parent]->val > node->val) {
            v[child] = v[parent];
            child = parent;
            parent = (child - 1) / 2;
        }
        v[child] = node;
        return true;
    }

    ListNode *pop() {
        if (empty()) {
            return nullptr;
        }
        swapNum(0, size - 1);
        size--;
        ListNode *removed = v[size];
        v[size] = nullptr;

        down(0);

        return removed;
    }

    void down(int parent) {
        int left = parent * 2 + 1;
        int right = left + 1;
        int min = parent;

        if (left < size && v[left]->val < v[min]->val) {
            min = left;
        }
        if (right < size && v[right]->val < v[min]->val) {
            min = right;
        }
        if (min != parent) {
            swapNum(parent, min);
            down(min);
        }

    }
    void swapNum(int i, int j) {
        ListNode *temp = v[i];
        v[i] = v[j];
        v[j] = temp;
    }
};

class Solution {
public:
    static ListNode *mergeKLists(vector<ListNode *> &lists) {
        auto *heap = new MinHeap((int) lists.size());
        for (ListNode *list: lists) {
            if (list != nullptr) {
                heap->put(list);
            }
        }
        auto *head = new ListNode(-1, nullptr);
        ListNode *tail = head;
        while (!heap->empty()) {
            ListNode *pNode = heap->pop();
            tail->next = pNode;
            tail = pNode;
            if (pNode->next != nullptr) {
                heap->put(pNode->next);
            }
        }
        return head->next;
    }
};
```





## 四、数组

### 4.1 合并有序数组

#### 1

```C++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int len = m + n - 1;
        for (int i = len; i >= 0; i--) {
            if (m == 0) {
                for (int j = 0; j < n; j++) {
                    nums1[j] = nums2[j];
                }
                break;
            }
            if (n == 0) {
                break;
            }
            nums1[i] =
                nums1[m - 1] < nums2[n - 1] ? nums2[n-- - 1] : nums1[m-- - 1];
        }
    }
};
```

反向输入

### 4.2 找到数组中第k大的元素

[215. 数组中的第K个最大元素 - 力扣（LeetCode）](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

#### 1









## 五、队列

### 5.1 二叉树层次遍历

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
    vector<vector<int>> levelOrder(TreeNode* root) {
        if (root == nullptr) {
            vector<vector<int>> v;
            return v;
        }
        vector<vector<int>> vs;
        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            int count = q.size();
            vector<int> v;
            while (count > 0) {
                TreeNode* node = q.front();
                q.pop();
                v.push_back(node->val);
                if (node->left != nullptr) {
                    q.push(node->left);
                }
                if (node->right != nullptr) {
                    q.push(node->right);
                }
                count--;
            }
            vs.push_back(v);
        }
        return vs;
    }
};
```



## 六、栈

### 6.1 有效的括号

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号

#### 1

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> sk;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(' || s[i] == '[' || s[i] == '{') {
                sk.push(s[i]);
            }
            if (s[i] == ')') {
                if (sk.empty() || sk.top() != '(') {
                    return false;
                } else {
                    sk.pop();
                }
            }
            if (s[i] == '}') {
                if (sk.empty() || sk.top() != '{') {
                    return false;
                } else {
                    sk.pop();
                }
            }
            if (s[i] == ']') {
                if (sk.empty() || sk.top() != '[') {
                    return false;
                } else {
                    sk.pop();
                }
            }
        }
        return sk.empty() ? true : false;
    }
};
```

### 6.2 逆波兰表达式求值

#### 1

```C++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> s;
        for (int i = 0; i < tokens.size(); i++) {
            if (tokens[i] == "+") {
                int num1 = s.top();
                s.pop();
                int num2 = s.top();
                s.pop();
                s.push(num1 + num2);
            } else if (tokens[i] == "*") {
                int num1 = s.top();
                s.pop();
                int num2 = s.top();
                s.pop();
                s.push(num1 * num2);
            } else if (tokens[i] == "-") {
                int num1 = s.top();
                s.pop();
                int num2 = s.top();
                s.pop();
                s.push(num2 - num1);
            } else if (tokens[i] == "/") {
                int num1 = s.top();
                if (num1 == 0) {
                    return 0;
                }
                s.pop();
                int num2 = s.top();
                s.pop();
                s.push(num2 / num1);
            } else {
                int num = stoi(tokens[i]);//将字符串转换为数字型
                s.push(num);
            }
        }
        return s.top();
    }
};
```

### 6.3 中缀表达式转后缀表达式

```C++
/*
    思路
    1. 遇到数字, 拼串
    2. 遇到 + - * /
    - 优先级高于栈顶运算符 入栈
    - 否则将栈中高级或平级运算符出栈拼串, 本运算符入栈
    3. 遍历完成, 栈中剩余运算符出栈拼串
    - 先出栈,意味着优先运算
    4. 带 ()
    - 左括号直接入栈,左括号优先级设为0
    - 右括号要将栈中直至左括号为止的运算符出栈拼串
*/

int priority(char c) {
    switch (c) {
        case '*':
        case '/':
            return 2;
        case '+':
        case '-':
            return 1;
        case '(':
            return 0;
        default:
            return -1;
    }
}

string infixToSuffix(string &infix) {
    stack<char> stk;
    string suffix;
    for (char c: infix) {
        switch (c) {
            case '*':
            case '+':
            case '/':
            case '-':
                if (stk.empty()) {
                    stk.push(c);
                } else {
                    if (priority(c) > priority(stk.top())) {
                        stk.push(c);
                    } else {
                        while (!stk.empty() && priority(stk.top()) >= priority(c)) {
                            suffix.push_back(stk.top());//将符号添加到字符串上
                            stk.pop();
                        }
                        stk.push(c);
                    }
                }
                break;
            case '(':
                stk.push(c);
                break;
            case ')': {
                while (stk.top() != '(') {
                    cout << stk.top();
                    suffix.push_back((stk.top()));
                    stk.pop();
                }
                stk.pop();
            }
                break;
            default: {
                suffix.push_back(c);
            }
        }
    }
    while (!stk.empty()) {
        suffix.push_back(stk.top());
        stk.pop();
    }
    return suffix;
}

int main() {
//    string infix = "(a+b*c-d)*e";
    string infix1="(a-b)/(c+d)";
    const string &suffix = infixToSuffix(infix1);
    cout << suffix << endl;
}
```



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

```C++
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



## 八、图



## 九、字符串

### 9.1 二进制求和

给你两个二进制字符串 `a` 和 `b` ，以二进制字符串的形式返回它们的和。

#### 1

```c++
class Solution {
public:
    string addBinary(string a, string b) {
        int i1 = a.size() - 1, i2 = b.size() - 1;
        int carry = 0;//妙
        //用carry/2来表示上一位是否有进位，只有当carry>2(carry为2或者3时，才会进位)
        string ans;
        while (i1 >= 0 && i2 >= 0) {
            int sum = carry;//sum表示这一位相加后的数值
            sum += (a.at(i1--) - '0') + (b.at(i2--) - '0');//相加求和
            carry = sum / 2;//进位
            ans += ('0' + sum % 2);//'0'+1来将int类型的数字转换为字符串类型
        }
        while (i1 >= 0) {//i1与i2长度不相等时，会有一个有剩余
            int sum = carry + a.at(i1--) - '0';
            carry = sum / 2;
            ans += ('0' + sum % 2);
        }
        while (i2 >= 0) {
            int sum = carry + b.at(i2--) - '0';
            carry = sum / 2;
            ans += ('0' + sum % 2);
        }
        if (carry == 1) {
            ans += '1';//最后如果还有进位，字符串要在加一
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```



## 十、经典算法问题

### 10.1 斐波那契数列

#### 1 递归

递归解决，太简单，就不写了

#### 2 动态规划

```java
public int Fibonacci(int n) {
        int prev = 0;
        int cur = 1;

        int temp = 0;
        for (int i = 0; i < n; i++) {
            temp=cur;
            cur=cur+prev;
            prev=temp;           
        }
        return cur;
    }
```

用prev代表当前的前一个值，初始值为0，用cur代表要返回的当前值，初始值为1

由于斐波那契数列的关系为f(n)=f(n-1)+f(n-2)，在该代码中，前一个循环的cur为f(n-1)，前一个循环的prev为f(n-2)

`cur=cur+prev;`

在相加之前，用一个临时变量temp存储cur，防止cur改变，无法给prev赋予正确的值，让其正确的参与到下一次循环中，与两数交换所用的思想一致

```java
 public static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
```

#### 3 数学公式

```java
public static void main(String[] args) {
    int n=6;
    double p=Math.pow(5,0.5);
    System.out.println((1 / p) * (Math.pow((1 + p) / 2, n+1) - Math.pow((1 - p) / 2, n+1)));
}
```

#### 3 变体问题

* 兔子问题
  - 第一个月，有一对未成熟的兔子（黑色，注意图中个头较小）
  - 第二个月，它们成熟
  - 第三个月，它们能产下一对新的小兔子（蓝色）
  - 所有兔子遵循相同规律，求第 n 个月的兔子数
* 爬楼梯问题
  - 楼梯有 n 阶
  - 青蛙要爬到楼顶，可以一次跳一阶，也可以一次跳两阶
  - 只能向上跳，问有多少种跳法

#### 4 记忆递归

```java
 public static int memorization(int n) {//假设n=5
     int[] cache = new int[n + 1];
     Arrays.fill(cache, -1); //cache=[-1,-1,-1,-1,-1,-1]
     cache[0] = 0;
     cache[1] = 1;
     return f(n, cache);
 }

 public static int f(int n, int[] cache) {
     if (cache[n] != -1) {
         return cache[n];//在数组中已经存储了，不需要在做重复的运算，直接返回
     }
     int x = f(n - 1, cache);
     int y = f(n - 2, cache);
     cache[n] = x + y;
     return cache[n];
 }
```

时间复杂度：O(n)，与传统递规相比增加了空间复杂度，空间换时间

### 10.2 汉诺塔

```java
/**
 * @param n 是圆盘的歌数
 */
private static void recursion(int n, LinkedList<Integer> a,
                              LinkedList<Integer> b, LinkedList<Integer> c) {
    count++;
    if (n == 0) {
        return;
    }
    recursion(n - 1, a, c, b);
    c.addLast(a.removeLast());//中间步骤
    recursion(n - 1, b, a, c);
}
```

## 十一、递归

在函数最后一步是调用一个函数，那么称之为尾调用

### 11.1 时间复杂度计算

若有递归式 T(n) = aT(n/b) + f(n)其中

- T(n) 是问题的运行时间，n是数据规模
- a 是子问题个数
- T(nb) 是子问题运行时间，每个子问题被拆成原问题数据规模的 nb
- f(n) 是除递归外执行的计算

**例1**

T(n)=2T(n2)+n4

- 此时 x=1<4，由后者决定整个时间复杂度 Θ(n4)
- 如果觉得对数不好算，可以换为求【$b$ 的几次方能等于 a】

**例2**

T(n)=T(7n/10)+n

- a=1,b=10/7,x=0,c=1
- 此时 x=0<1，由后者决定整个时间复杂度 Θ(n)

**例3**

T(n)=16T(n/4)+n2

- a=16,b=4,x=2,c=2
- 此时 x=2=c，时间复杂度 Θ(n2log⁡n)