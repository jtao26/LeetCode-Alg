# Day4 链表 Part2

## 24 Swap Nodes in Pairs

### First Time Code

```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode *fakeHead = new ListNode(-1, head);
        ListNode *curr = fakeHead;
        while(curr->next && curr->next->next) {
            ListNode *first = curr->next;
            curr->next = curr->next->next;
            first->next = curr->next->next;
            curr->next->next = first;

            curr = curr->next->next;
        }
        ListNode *newHead = fakeHead->next;
        delete fakeHead;
        return newHead;
    }
};
```

- 假头部能帮助更好地解决这个问题，两两交换的难点包括
    - 头部节点的特殊处理，可以通过假头部解决，这样链表中的每个节点都能被一致处理
    - 节点长度以及何时终止：通过判断接下来两个节点是否存在，决定需不需要交换

---

## 19 Remove Nth Node From end of List

### First Time Code

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *fakeHead = new ListNode(-1, head);
        ListNode *curr = fakeHead;
        while(curr->next && n > 0) {
            curr = curr->next;
            --n;
        }
        if(n > 0){
            return nullptr;
        }
        ListNode *before = fakeHead;
        while(curr->next) {
            curr = curr->next;
            before = before->next;
        }
        ListNode *toDelete = before->next;
        before->next = before->next->next;
        delete toDelete;
        toDelete = nullptr;
        ListNode *newHead = fakeHead->next;
        delete fakeHead;
        fakeHead = nullptr;
        return newHead;
    }
};
```

- 第一次AC，在写算法的时候没往假头部考虑，因为被倒数吸引，感觉头部不重要
- 然而事实证明假头部一样重要，因为当链表长 1 且 n=1 时删除的就是头节点，此时假头节点能够更好地处理这种情况

---

- AC first time

## 160 Intersection of Two Linked Lists

### First Time Code

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *curr = headA;
        int lenA = 1;
        while(curr->next){
            curr = curr->next;
            ++lenA;
        }
        curr = headB;
        int lenB = 1;
        while(curr->next){
            curr = curr->next;
            ++lenB;
        }

        curr = lenA > lenB ? headA : headB;
        ListNode *other = lenA > lenB ? headB : headA;
        int diff = abs(lenA-lenB);
        while(diff > 0){
            curr = curr->next;
            --diff;
        }
        while(curr) {
            if(curr == other)
                return curr;
            curr = curr->next;
            other = other->next;
        }
        return nullptr;
    }
};
```

- 本次没有涉及到伪头部，纯粹是链表性质的应用，虽然我们不知道在何处相交，但我们可以通过遍历得出长度差，再从减去该长度差的位置查起

---

- AC first time

## 1992 Linked List Cycle II

### First Time Code

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *fast = head;
        ListNode *slow = head;
        while(fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
            if(slow == fast) {
                ListNode *curr = head;
                while(curr != slow) {
                    curr = curr->next;
                    slow = slow->next;
                }
                return curr;
            }
        }
        return nullptr;
    }
};
```

---

- 典型的快慢指针问题，通过每次前进两步的快指针和每次前进一步的慢指针解决问题
- 如果链表中存在环，那么快指针一定会在某一时刻追上慢指针，即 fast == slow
- 当快指针追上慢指针后，为了得到环的起始位置，我们需要另一个指针辅助寻找
    - 另一个指针应从头节点出发，和慢指针同时继续遍历链表 (此时快指针不再移动)
    - 当第三个指针与慢指针重合时，两指针所在的位置就是环的起点