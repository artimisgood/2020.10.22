# 2020.10.22——删除链表的倒数第N个节点
## 题目
给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

示例：
```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```
说明：

给定的 n 保证是有效的。

## 解题
### 思路1——计算长度
们首先从头节点开始对链表进行一次遍历，得到链表的长度 L。随后我们再从头节点开始对链表进行一次遍历，当遍历到第 L-n+1 个节点时，它就是我们需要删除的节点。

为了与题目中的 n 保持一致，节点的编号从 11 开始，头节点为编号1的节点。

为了方便删除操作，我们可以从哑节点开始遍历 L−n+1 个节点。当遍历到第 L−n+1 个节点时，它的下一个节点就是我们需要删除的节点，这样我们只需要修改一次指针，就能完成删除操作。


#### 语言
java
#### 代码
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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode du = new ListNode(0,head);
        int length = getLength(head);
        ListNode cur = du;
        for(int i=1;i<length-n+1;++i){
            cur = cur.next;
        }//进行遍历
        cur.next = cur.next.next;//删除操作
        ListNode ans = du.next;
        return ans;
    }

    public int getLength(ListNode head){//测量长度的函数
        int length = 0;
        while(head!=null){
            ++length;
            head = head.next;
        }
        return length;
    }
}
```
### 思路2——栈
根据栈「先进后出」的原则，我们弹出栈的第 nn 个节点就是需要删除的节点，并且目前栈顶的节点就是待删除节点的前驱节点。这样一来，删除操作就变得十分方便了。
#### 语言
java
#### 代码
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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode du = new ListNode(0,head);
        Deque<ListNode> stack = new LinkedList<ListNode>();//建立栈
        ListNode cur = du;
        while(cur!=null){//将节点放入栈
            stack.push(cur);
            cur = cur.next;
        }   
        for(int i=0;i<n;++i){//弹出节点
            stack.pop();
        }
        ListNode prev = stack.peek();//找到栈顶的值
        prev.next = prev.next.next;//进行删除操作
        ListNode ans = du.next;
        return ans;
    }
}
```
