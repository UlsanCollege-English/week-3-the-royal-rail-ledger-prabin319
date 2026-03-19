# Week 3: The Royal Rail Ledger

## Summary

This assignment focuses on linked-list operations set in a railway story context.
I implemented five functions across two linked-list types: a singly linked list (SLL)
for building, reading, and finding repeated cargo entries, and a doubly linked list (DLL)
for removing banned cargo and checking palindromic train sequences.
The functions implemented are `build_sll_from_list`, `sll_to_list`, `find_first_repeat_sll`,
`remove_all_from_dll`, and `is_train_palindrome`.
The hardest part was correctly updating all four pointers when removing nodes from the DLL,
especially for consecutive target values and nodes at the head or tail.

---

## Approach

### `build_sll_from_list(values)`
- I started with an empty `SinglyLinkedList` and handled the empty-input case immediately by returning it.
- I built the list by creating a head node from `values[0]`, then iterating through the remaining values and attaching each as a new node via `current.next`.
- I made sure the values stayed in the correct order by walking forward through the input list, which naturally preserves left-to-right order.

### `sll_to_list(sll)`
- I started at `sll.head` with an empty Python list to collect results.
- I traversed the list by following `.next` pointers in a `while` loop until reaching `None`.
- I collected values by appending `current.value` at each step and returning the list at the end.

### `find_first_repeat_sll(sll)`
- I tracked values I had already seen using a Python `set` called `seen` for O(1) average-case lookups.
- When I found a repeated value (`current.value` already in `seen`), I returned it immediately without scanning further.
- If I reached the end with no repeat, I returned `None`.

### `remove_all_from_dll(dll, target)`
- I traversed the list by saving `next_node = current.next` before any removal to avoid losing the chain.
- When I found the target value, I updated the neighbors' pointers: `current.prev.next` and `current.next.prev`, patching both sides around the removed node.
- I checked special cases where the removed node was the head (update `dll.head`) or the tail (update `dll.tail`), and handled consecutive target nodes correctly.

### `is_train_palindrome(dll)`
- I compared values from both ends using a `left` pointer (starting at `dll.head`) and a `right` pointer (starting at `dll.tail`) walking inward.
- I stopped when the pointers met on the same node (odd length) or when `left` stepped past `right` (even length).
- I returned `True` or `False` based on whether all compared pairs matched — returning `False` immediately on any mismatch.

---

## Complexity

### `build_sll_from_list(values)`
- **Time complexity:** O(n)
- **Space complexity:** O(n)
- **Why:** We visit each value once to create n nodes, and store all n nodes in memory.

### `sll_to_list(sll)`
- **Time complexity:** O(n)
- **Space complexity:** O(n)
- **Why:** We traverse all n nodes once, and the output list holds n values.

### `find_first_repeat_sll(sll)`
- **Time complexity:** O(n)
- **Space complexity:** O(n)
- **Why:** In the worst case we scan all n nodes; the `seen` set stores up to n values.

### `remove_all_from_dll(dll, target)`
- **Time complexity:** O(n)
- **Space complexity:** O(1)
- **Why:** We make one pass through all n nodes; pointer updates are done in-place with no extra storage.

### `is_train_palindrome(dll)` *(stretch)*
- **Time complexity:** O(n)
- **Space complexity:** O(1)
- **Why:** Two pointers walk inward from both ends — at most n/2 comparisons, no extra storage needed.

### Complexity notes

- Linked lists have no direct indexed access — you must traverse from the head to reach any node.
- Traversal is always linear O(n); there is no O(1) random access like Python lists.
- Pointer updates (insert/remove) are O(1) once you already hold a reference to the node.
- SLLs are simpler but only allow forward traversal; DLLs add backward traversal at the cost of maintaining a `prev` pointer on every node.

---

## Edge-Case Checklist

- [x] empty SLL
- [x] empty DLL
- [x] single-node SLL
- [x] single-node DLL
- [x] no repeated values in SLL
- [x] repeated value appears later in SLL
- [x] repeated value includes the head value
- [x] removing from DLL when target is at head
- [x] removing from DLL when target is at tail
- [x] removing consecutive target values in DLL
- [x] removing all nodes from DLL
- [x] palindrome with odd length
- [x] palindrome with even length
- [x] non-palindrome DLL

---

## Assistance & Sources

### AI use
- I used AI: Yes
- AI helped with: reviewing the implementation of all five functions, explaining pointer update logic for DLL removal, and filling out this README.

### Other sources
- Class notes: linked list lecture slides
- Python docs: built-in `set` for O(1) membership testing

---

## Debugging Notes

- I first got stuck on `remove_all_from_dll` — removing the head node crashed because `current.prev` was `None` and I tried to access `.next` on it without checking first.
- The failing test that helped me most was the one that removes all nodes; it forced me to handle both `dll.head` and `dll.tail` becoming `None`.
- I fixed the issue by separating the prev-patch and next-patch into guarded `if/else` blocks: update `dll.head` when there is no prev, update `dll.tail` when there is no next.
- I also discovered I needed to save `next_node` before removal; without it, consecutive target values would lose the traversal reference.
- One mistake I will avoid next time is assuming head/tail are always non-`None` — always check before dereferencing.

---

## Final Reflection

This assignment showed me that linked lists trade random access for efficient insertion and deletion — once you have a pointer, restructuring is cheap, but finding a node still requires a full traversal. The DLL was harder than the SLL because every remove operation touches up to four pointers instead of one, and the head/tail edge cases multiply. The most surprising edge case was removing consecutive duplicate values: saving `next_node` before the removal was essential, which would not be an issue with a regular Python list. Before the next assignment I want to review how sentinel nodes can simplify head/tail edge-case handling.