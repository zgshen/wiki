
# 删除链表的节点

给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

时间复杂度O(N)，空间复杂度O(1)。

```java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        if (head==null) return head;
        if (head.val==val) return head.next;
        ListNode cur=head;

        while (cur!=null && cur.next.val!=val) cur=cur.next;
        if (cur.next!=null) cur.next = cur.next.next;
        return head;
    }
}

class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```

双指针更容易理解一些。

```java
public ListNode deleteNode(ListNode head, int val) {
    if(head.val == val) return head.next;
    ListNode pre = head, cur = head.next;
    while(cur != null && cur.val != val) {
        pre = cur;
        cur = cur.next;
    }
    if(cur != null) pre.next = cur.next;
    return head;
}
```