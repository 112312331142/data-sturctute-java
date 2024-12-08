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

```java
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

```c++
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

```c++
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

```c++
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

```c++
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

```c++
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

