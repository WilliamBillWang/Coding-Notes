# Build a linked list
- using struct
- including value(usually deonoted by val)
- including next(a pointer that point at next node address)
## Code
``` c++
struct ListNode{
    int val;
    ListNode* next;

    ListNode(int x){
        val = x;
        next = nullptr;
    }
};
```
# Build a new node
## Code
```cpp
ListNode* A = new ListNode(10);
```
> [!IMPORTANT] '*'  in the code means the "A" is a pointer, which point at the new node.

# Connect two nodes
>"If you want to connect node A to node B, you need to make the pointer of node A point to B."
## Code
```cpp
A->next = B;
```
# What is head?
```cpp
ListNode *head = A;
```
>[!TIP] head is a pointer that always points at the first node of the linked list.
# Traversal

> [!Caution] Don't modify the haed node or you will lost the hold linked list.

>instead, you should cread a new node that point at the head at beginning.

## Code
```cpp
ListNode* cur = head;
```
>and the while the cur doesn't point at the nullptr, you sholud do two things.
1. print the value 
```cpp
cout << cur -> val;
```
2. point at next
```cpp
cut = cur->next;
```
### complete code
```cpp
ListNode* cur = head;

while(cur != nullptr){
    cout << cur->val << endl;
    cur = cur->next;
}
```
# What is the Dummy node?
>A dummy node is a placeholder node placed at the beginning of a linked list. It points to the actual first node, which helps simplify edge cases when inserting or deleting a node at the head of the list.

```cpp
ListNode dummy(0);
dummy.next = head;
```
### Schematic diagram
dummy
  |
  v
0 ->1 ->2 ->3

# Using three pointers to reverse the linked list
>the core three pointers are 
1. prev(has already reversed)
2. cur(processing)
3. next(temporarily preserve next)
```cpp
ListNode* prev = nullptr;
ListNode* cur = head;
ListNode* next = nullptr;

while(cur != nullptr){
	next = cur -> next;
	cur -> next = prev;
	prev = cur;
	cur = next;
}
```

# Fast and Slow Pointer
>the core of the two pointers are:
```
ListNode* slow = head;
ListNode* fast = head;
```
>the only difference are speed 
>-> slow move one at every step.
>-> fast move two at every step.

>[!Tip] It can be used to find the middle of the linked list.

# Insert
## type 1 : Insert to the first position

>Three steps:
1. Create a new node.
```cpp
ListNode* node = new ListNode("the value that you want to isnsert");
```
2. Let the new node point at the head.
```cpp
node -> next = head;
```
3. Let haed point at new node.
```cpp
head = node;
```
## type 2 : Insert to the middle position


>Three steps:
1. Create a new node.
```cpp
ListNode* node = new ListNode("the value that you want to isnsert.");
```
2. Let the new node point at the next node.
```cpp
node -> next = cur -> next;
```
3. Let the previous node point at the new node
```cpp
cur -> next = node;
```
## type 3 : Insert to the last position

>Three steps:
1. Let cur point at the last node
```cpp
ListNode* cur = head;
while(cur -> next != nullptr){
	cur = cur -> next;
}
```
2. Create a new node
```cpp
ListNode* node = new ListNode("the value that you want to insert.");
```
3. connect
```cpp
cur -> next = node;
```

# Delete
>[!tip] The essense of "Delete" is not destroying the node but rather making the linked list **skip** over it first.

## code
### method 1 :
```cpp
cur -> next = cur -> next -> next;
```
### method 2 : 
```cpp
ListNode* temp = cur ->next;
cur -> next = temp -> next;
delete temp;
```
# Merge Two Sorted Lists

>Now, we have two sorted list:
>List A :1 -> 3 -> 5	List B : 2 -> 4 -> 6

- the three core characters are:
1. tail(which always points at the answer's last node)
2. l1(traverse the firist linked list)
3. l2(traverse the second linked list)
## Code
```cpp
ListNode dummy(0);

List* tail = &dummy;   

ListNode* l1;
ListNode* l2;
```

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {

        ListNode dummy(0);
        ListNode* tail = &dummy;

        while (list1 != nullptr && list2 != nullptr) {

            if (list1->val <= list2->val) {
                tail->next = list1;      
                list1 = list1->next;     
            } else {
                tail->next = list2;      
                list2 = list2->next;     
            }   
            tail = tail->next;
        }
//------------------------------------------------------------------------
        if (list1 != nullptr) {
            tail->next = list1;
        }

        if (list2 != nullptr) {
            tail->next = list2;
        }
        return dummy.next;
    }
};
```

# Remove Nth Node From End
>[!tip] Using fast and slow pointers

>There are threes steps
1. Create a dummy
2. Create two pointers(fast and slow)
3. Let  fast move n+1 steps
## Code
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode dummy(0);
        dummy.next = head;

        ListNode* fast = &dummy;
        ListNode* slow = &dummy;
        for (int i = 0; i <= n; i++) {
            fast = fast->next;
        }
        while (fast != nullptr) {
            fast = fast->next;
            slow = slow->next;
        }
        ListNode* temp = slow->next;
        slow->next = temp->next;
        delete temp;

        return dummy.next;
    }
};
```

# Cycle
>[!tip] Using slow and fast pointers
- fast(move two step at a time)
- slow(move one step at a time)

>Conditional statement : 
```cpp
while(fast != nullptr && fast -> next != nullptr)
``` 
>Because it can ensure data accessibility for **fast** and **fast -> next -> next**

## Code
```cpp
while (fast != nullptr && fast->next != nullptr) {

    slow = slow->next;
    fast = fast->next->next;

    if (slow == fast) {
        return true;
    }
}

return false;
```
