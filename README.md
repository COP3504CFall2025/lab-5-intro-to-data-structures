# **Lab 5: Intro to Data Structures**
***
## **1. LinkedList.hpp**

### **Purpose**

A **templated doubly linked list** used as the internal container for all linked-list-based structures (`LLS`, `LLQ`, `LLDQ`).

### **Core Attributes**

| Variable | Type | Description |
| :-- | :-- | :-- |
| `Node* head` | Pointer | Points to the first node |
| `Node* tail` | Pointer | Points to the last node |
| `unsigned int count` | Unsigned Int | Number of elements |

**Node Structure**:

```cpp
struct Node {
    T data;
    Node* prev;
    Node* next;
};
```


### **Key Methods**

| Method | Behavior | Complexity |
| :-- | :-- | :-- |
| `AddHead(const T&)` | Inserts at the beginning | O(1) |
| `AddTail(const T&)` | Inserts at the end | O(1) |
| `RemoveHead()` | Removes the first node | O(1) |
| `RemoveTail()` | Removes the last node | O(1) |
| `Clear()` | Deletes all nodes | O(n) |
| `getCount()` | Returns node count | O(1) |
| `getHead()` / `getTail()` | Returns pointers to head/tail | O(1) |

### **Utility Visualization**

- `PrintForward()` → Prints elements from head to tail.
- `PrintReverse()` → Prints elements from tail to head.

**All LinkedList-based implementations must properly follow the Big Five** since the list allocates nodes dynamically (`new Node()`).

***

## **2. Interfaces.hpp**

### **Abstract Interfaces Defined**

1. **StackInterface<T>**
2. **QueueInterface<T>**
3. **DequeInterface<T>**

These define *behavioral contracts* for data structure interactions.

### **StackInterface**

Defines standard **LIFO** (Last-In-First-Out) operations:

```cpp
void push(const T& item);
T pop();
T peek() const;
std::size_t getSize() const noexcept;
```


### **QueueInterface**

Defines standard **FIFO** (First-In-First-Out) operations:

```cpp
void enqueue(const T& item);
T dequeue();
T peek() const;
std::size_t getSize() const noexcept;
```


### **DequeInterface**

Defines **double-ended sequential** access operations:

```cpp
void pushFront(const T& item);
void pushBack(const T& item);
T popFront();
T popBack();
const T& front() const;
const T& back() const;
std::size_t getSize() const noexcept;
```

Each inherits no internal data, relying on linked or array containers for definition. All functions mentioned above will be overriden inside the derived classes, and **each interface should be defined as Abstract Base Classes.**

***

## **3. Array-Based Implementations**

These use internal **dynamic arrays** and manage resizing logic.
All implementors of `StackInterface`, `QueueInterface`, or `DequeInterface` **must fully define the Big Five** due to dynamic memory.

***

### **A. ABS.hpp — Array-Based Stack**

Implements `StackInterface<T>` (LIFO).


| Attribute | Type | Description |
| :-- | :-- | :-- |
| `T* array_` | Pointer | Dynamic array storing elements |
| `std::size_t capacity_` | Size_t | Total allocated slots |
| `std::size_t curr_size_` | Size_t | Current stack depth |

**Methods**


| Method | Action |
| :-- | :-- |
| `push(const T&)` | Push at top; doubles capacity if full |
| `pop()` | Removes the last inserted element |
| `peek() const` | Returns top element |
| `getSize()` | Returns element count |
| `PrintForward()` | Prints stack from bottom to top |
| `PrintReverse()` | Prints stack from top to bottom |

All resizing operations double capacity (scale factor = 2).
**Big 5** required to handle possible reallocations.

***

### **B. ABQ.hpp — Array-Based Queue**

Implements `QueueInterface<T>` (FIFO).


| Attribute | Type | Description |
| :-- | :-- | :-- |
| `T* array_` | Dynamic storage |  |
| `std::size_t capacity_` | Total buffer size |  |
| `std::size_t curr_size_` | Number of active elements |  |

**Methods**


| Method | Description |
| :-- | :-- |
| `enqueue(const T&)` | Adds element to back |
| `dequeue()` | Removes from front |
| `peek()` | Returns front element |
| `getSize()` | Returns size |
| `PrintForward()` | Print front-to-back contents |
| `PrintReverse()` | Print in reverse order |


***

## **4. Linked-List Implementations**

These rely on the `LinkedList<T>` class for internal node management.

***

### **A. LLS.hpp — Linked-List Stack**

Implements `StackInterface<T>`


| Attribute | Type | Purpose |
| :-- | :-- | :-- |
| `LinkedList<T> list` | Internal container | Maintains stack order |

**Methods**


| Operation | Description |
| :-- | :-- |
| `push(const T&)` | Adds node at head |
| `pop()` | Removes and returns head |
| `peek()` | Returns head’s data |
| `getSize()` | Returns node count |
| `PrintForward()` | Uses LinkedList’s forward traversal |
| `PrintReverse()` | Uses LinkedList’s reverse traversal |

All methods O(1).
**Big Five** must correctly copy/move internal list state.

***

### **B. LLQ.hpp — Linked-List Queue**

Implements `QueueInterface<T>`


| Attribute | Type | Purpose |
| :-- | :-- | :-- |
| `LinkedList<T> list` | Internal member | Implements FIFO |

| Method | Description |
| :-- | :-- |
| `enqueue(const T&)` | Adds new tail node |
| `dequeue()` | Removes head node |
| `peek()` | Returns head data |
| `getSize()` | Returns list count |
| `PrintForward()` | Forward traversal |
| `PrintReverse()` | Reverse traversal |

All operations maintain **O(1)** efficiency.

***

### **C. LLDQ.hpp — Linked-List Deque**

Implements `DequeInterface<T>`


| Attribute | Type | Purpose |
| :-- | :-- | :-- |
| `LinkedList<T> list` | Composition | Double-ended linked container |

| Method | Description |
| :-- | :-- |
| `pushFront(const T&)` | Insert head |
| `pushBack(const T&)` | Insert tail |
| `popFront()` / `popBack()` | Remove from front/back |
| `front()` / `back()` | Retrieve end data |
| `getSize()` | Total nodes |
| `PrintForward()` | Forward traversal |
| `PrintReverse()` | Reverse traversal |

Like all link-based structures, LLDQ has **O(1)** double-ended operations and **must follow the Big 5** to manage internal state safely.

***

## **5. ABDQ.hpp — Array-Based Deque**

Implements **DequeInterface<T>** using a **circular buffer** model.

### **Attributes**

| Attribute | Type | Description |
| :-- | :-- | :-- |
| `T* data_` | Pointer | Circular array buffer |
| `std::size_t capacity_` | Total array size |  |
| `std::size_t size_` | Active count |  |
| `std::size_t front_` | Front index pointer |  |
| `std::size_t back_` | Back insertion index |  |

### **Methods**

| Operation | Explanation |
| :-- | :-- |
| `pushFront()` / `pushBack()` | Insert element from either end |
| `popFront()` / `popBack()` | Remove element from either end |
| `front()` / `back()` | Access extremities |
| `ensureCapacity()` | Resizes to `capacity_ * 2` |
| `shrinkIfNeeded()` | Reduces to half when sparse |
| `PrintForward()` | Traverses queue order |
| `PrintReverse()` | Traverses reverse order |

**All array-based classes must implement the Big 5** to safely transfer `data_` ownership during copies and moves.

***

## **6. Summary Hierarchy**

```
             (Interfaces)
     ┌─────────────────────────────┐
     │  StackInterface<T>          │
     │  QueueInterface<T>          │
     │  DequeInterface<T>          │
     └────────────┬────────────────┘
                  │
        ┌─────────┼────────────────────┐
        │          │                    │
   Array-Based   Linked-List           Deque
     (ABS/ABQ)   (LLS/LLQ)     ┌───────┬────────┐
                                │ ABDQ  │ LLDQ   │
```


***

## **7. Implementation Expectations**

1. **Every class with dynamic memory** must explicitly define all **five special member functions** (The Big 5):
    - Destructor
    - Copy Constructor
    - Copy Assignment
    - Move Constructor
    - Move Assignment
2. **LinkedList-based classes** should **delegate these for internal list members**:
    - The contained `LinkedList<T>` handles its Big Five.
    - Derived objects correctly follow composition semantics.
3. **Print methods required in every class**:
    - `PrintForward()`
    - `PrintReverse()`
These functions enhance debugging and visual validation for every ADT.
4. **Interfaces define contract, not storage.**
None of the interfaces inherit `LinkedList<T>` — instead, concrete `LLS`, `LLQ`, and `LLDQ` **include** a list as a private attribute.

***

## **8. Deliverable Goals**

By the end of this lab, you should:

- Implement **safe, reusable ADTs** leveraging polymorphism and composition.
- Demonstrate understanding of the **Rule of Five** for class lifecycle control.
- Extend each class with directional printing via `PrintForward()` and `PrintReverse()`.
- Maintain **constant-time access patterns** for all primary operations.

***

## **References**

- [GeeksForGeeks — Rule of Five in C++][^1]
- [CppReference — Rule of Three/Five/Zero][^4]
- [Fluent C++ — Compiler-Generated Functions][^8]

***

**Summary:**
This lab bridges the gap between data abstraction and implementation detail in C++. You are expected to respect proper memory management, polymorphic inheritance, and the Rule of Five across all ADTs while reinforcing traversal output through the universal `PrintForward()` and `PrintReverse()` utility functions.
<span style="display:none">[^2][^3][^5][^6][^7]</span>

<div align="center">⁂</div>

[^1]: https://www.geeksforgeeks.org/cpp/rule-of-five-in-cpp/

[^2]: https://stackoverflow.com/questions/5976459/how-to-actually-implement-the-rule-of-five

[^3]: https://www.codementor.io/@sandesh87/the-rule-of-five-in-c-1pdgpzb04f

[^4]: http://en.cppreference.com/w/cpp/language/rule_of_three.html

[^5]: https://learncplusplus.org/what-is-the-rule-of-five-in-modern-c/

[^6]: https://www.reddit.com/r/cpp_questions/comments/11jpv13/what_are_the_implications_of_the_the_rule_of_5/

[^7]: https://leimao.github.io/blog/CPP-Rule-of-Five/

[^8]: https://www.fluentcpp.com/2019/04/19/compiler-generated-functions-rule-of-three-and-rule-of-five/

