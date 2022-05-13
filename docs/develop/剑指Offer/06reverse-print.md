
# 从尾到头打印链表

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

头插法。时间复杂度O(N)，空间复杂度O(N)。

```java
class ListNode {
    int val;
    ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}

public int[] reversePrint(ListNode head) {
    ListNode newList = null;
    int len = 0;
    while (head != null) {
        ListNode next = head.next;
        // 插入头部
        head.next = newList;
        newList = head;
        // 下一个继续遍历
        head = next;
        len++;
    }

    int arr[] = new int[len];
    while (newList != null) {
        arr[arr.length - len] = newList.val;
        newList = newList.next;
        len--;
    }
    return arr;
}
```

辅助栈。时间复杂度O(N)，空间复杂度O(N)。
```java
public int[] reversePrint(ListNode head) {
    Stack<Integer> stack = new Stack<>();
    while (head != null) {
        stack.push(head.val);
        head = head.next;
    }
    int arr[] = new int[stack.size()];
    for (int i = 0; i < arr.length; i++) {
        arr[i] = stack.pop();
    }
    return arr;
}
```

递归。时间复杂度O(N)，空间复杂度O(N)。
```java
public int[] reversePrint1(ListNode head) {
    return recursive(head, 0);
}
public int[] recursive(ListNode head, int i) {
    if (head == null)
        return new int[i];//遍历到最后一个元素的下个 null 节点得到数组长度了就可以创建数组返回，下面操纵赋值

    int arr[] = recursive(head.next, i+1);
    arr[arr.length - i - 1] = head.val;//倒叙赋值
    return arr;
}
```