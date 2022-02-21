### 快慢指针

#### 有序列表中位数

**问题**：找出有序列表中的中位数

**思路**：fast 指针每次移动两格， slow 指针每次移动一格。fast 指针移动到末尾时，slow 正好移动到中间。



#### 环形列表 No.141

**问题**：判断列表中是否有环

**思路**：fast 指针每次移动两格， slow 指针每次移动一格， 如果存在环路， 则fast最终一定会和slow相遇

时间复杂度O(n)，空间复杂度O(1)

```c++
bool hasCycle(ListNode *head) {
    ListNode *fast = head, *slow = head;
    
    while(fast != NULL && fast->next != NULL ){
        fast = fast->next-> next;
        slow = slow->next;
        if(fast == slow)
            return true;
    }
    
    return false;
}
```



#### 环形列表 Ⅱ No.142

**问题**：给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`

**思路**

<img src="https://pic2.zhimg.com/80/v2-58d45b079192d14e0aebbbe8aa8b0dcd_1440w.jpg"  width=""  height = "200" />

- 快慢指针判断链表中是否有环，记录相遇位置 z
- 两个指针分别从 z 和 x 同时出发，每次移动一个单位，相遇位置为环的起点
- 时间复杂度O(n)，空间复杂度O(1)



**详解**

在 z 相遇时，slow 指针移动距离为 $a+b$ ，fast 指针移动距离为 $a + n(b + c )+ b$ 

行动次数相同时，fast 指针走过的距离是 slow 指针走过的距离的2倍，即 $a + b + n(b + c) = 2 (a + b)$，简化得 $ a = (n-1)(b + c) + c$

可以看出，a 的长度，即为 n - 1 倍环的长度，再加上 c 的长度

因此，再放速度相同的两个指针，分别从 x 和 z 出发， 它们相遇位置即为环的起点

```c++
ListNode *detectCycle(ListNode *head){
    ListNode *fast = head, *slow = head;
    while (fast != NULL && fast->next != NULL){
        fast = fast->next->next;
        slow = slow->next;
        if (fast == slow)
            break;
    }

    if (fast == NULL || fast->next == NULL)
        return NULL;

    fast = head;
    while (fast != slow){
        fast = fast->next;
        slow = slow->next;
    }
    return fast;
}
```



### 合并两个有序链表

**问题**：给定两个升序链表头节点，返回合并后新的升序链表

**迭代思路**：

- 设置头部哨兵节点 preHead，用 **new 初始化**，结果返回 `preHead -> next`
- 循环终止时，非空链表接在最后即可

```c++


```



**递归思路**

- 倒着推，从后向前合并，pre 指向， List1 与 List2 为未合并链表的头节点
- 先考虑循环截止情况，某个链表（比如 List2）结束了，此时 pre 指向 List1，函数返回 List 1
- 循环截止的上一步，List 2 还剩最后一个节点，







### DFS

https://leetcode-cn.com/problems/number-of-islands/solution/dao-yu-lei-wen-ti-de-tong-yong-jie-fa-dfs-bian-li-/

##### 树

```c++
void traverse(TreeNode root) {
    if (root == null)
        return;
    
    traverse(root.left);
    traverse(root.right);
}
```



#### 岛屿数量 No.200



```c++
```





## 哈希表应用

#### 记录字母出现次数

- 例中字母全为小写

```c++
string s = "apple";
vector<int> table(26, 0);

for(char c : s)
    ++table['z' - c];

```





### 常见特殊情况

- 输入 NULL or 0
- 
