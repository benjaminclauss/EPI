## Problems

## Primitive Types

- Integers in Python 3 are unbounded - the maximum integer representable is a function of the available memory.
- The constant `sys.maxsize` can be used to find the word-size; specifically, it's the maximum value integer that can be stored in the word, e.g., $2**63 - 1$ on a 64-bit machine.
- Bounds on floats are specified in `sys.float-info`.

### Top Tips for Primitive Types
- Be very comfortable with the **bitwise operators**, particularly XOR.
- Understand how to use **masks** and create them in an **machine independent** way,
- Know fast ways to **clear the lowermost set bit** (and by extension, set the lowermost 0, get its index, etc.)
- Understand **signedness** and its implications to **shifting**.
- Consider using a **cache** to accelerate operations by using it to brute-force small inputs.
- Be aware that **commutativity** and **associativity** can be used to perform operations in **parallel** and **reorder** operations.

### Bit-wise Operators
- Negative numbers are treated as their 2's complement value.
- There is no concept of an unsigned shift in Python, since integers have infinite precision.

| Operator | Example | Meaning |
| ---- | ---- | ---- |
| `&` | `a & b` | Bitwise AND |
| `\|` | `a \| b` | Bitwise OR |
| `^` | `a ^ b` | Bitwise XOR (exclusive OR) |
| `~` | `~a` | Bitwise NOT |
| `<<` | `a << n` | Bitwise left shift |
| `>>` | `a >> n` | Bitwise right shift |

`x & (x-1)`  = `x` with its lowermost  
### Methods for Numeric Types
- `abs(-34.5)`
- `math.ceil(2.17)`
- `math.floor(3.14)`
- `min(x, -4)`
- `max(3.14, y)`
- `math.pow(2.71, 3.14)` (alternatively, `2.71 ** 3.14`)
- `math.sqrt(225).
### Random
- `random.randrange(28)`
- `random.randint(8, 16)`
- `randon.random()`
- `random.shuffle(A)`
- `random.choice(A)`
### Other
- Know how to interconvert integers and strings.
- Unlike integers, floats are not infinite precision, and it's convenient to refer to infinity as `float('inf')` and `float('-inf')`.
	- These values are comparable to integers, and can be used to create pseudo max-int and pseudo min-int.
- When comparing floating point values consider using `math.isclose()` - it is robust, e.g., when comparing very large values, and can handle both absolute and relative differences.
-

## Arrays

### Top Tips for Arrays
- Array problems often have simple brute force solutions that use $O(n)$ space, but there are subtler solutions that **use the array itself** to **reduce space** complexity to $O(1)$.
- Filling an array from the front is slow, so see if it's possible to **write values from the back**.
- Instead of deleting an entry (which requires moving all entries to its left), consider **overwriting** it.
- When dealing with integers encoded by an array consider processing the **digits from the back** of the array. Alternatively, reverse the array so the **least-significant digit is the first entry**.
- Be comfortable with writing code that operates on **subarrays**.
- It's incredibly easy to **make off-by-1** errors when operating on arrays - reading past the last element of an array is a common error which has catastrophic consequences.
- Don't worry about preserving the **integrity** of the array (sortedness, keeping equal entries together, etc.) until it is time to return.
- An array can serve as a good data structure when you know the distribution of the elements in advance. For example, a Boolean array of length $W$ is a good choice for representing a subset of ${0, 1, ..., W-1}$. (When using a Boolean array to represent a subset of ${1, 2, 3, ..., n}$ allocate an array of size $n+1$ to simplify indexing.) .
- When operating on 2D arrays, **use parallel logic** for rows and for columns.
- Sometimes it's easier to **simulate the specification**, than to analytically solve for the result. For example, rather than writing a formula for the $i$-th entry in the spiral order for an $n x n$ matrix, just compute the output from the beginning.



### Array Libraries

Arrays in Python are provided by the `list` type. 
The tuple type is very similar to the list type, with the constraint that it is immutable.) 
The key property of a `list` is that it isdynamically-resized, i.e., there's no bound as to how many values can be added to it.
In the same way, values can be deleted and inserted at arbitrary locations.

- Know the syntax for instantiating a list, e.g., `[3, 5, 7, 11]`, `[1] + [0] * 10`, `list(range(100))`. (List comprehension, described later, is also a powerful tool for instantiating arrays.)  
- The basic operations are `len(A)`, `A.append(42)`, `A.remove(2)`, and `A.insert(3, 28)`.
- Know how to instantiate a 2D array, e.g., `[[1 , 2, 4], [3, 5, 7, 9], [13]]`.
- Checking if a value is present in an array is as simple as `a in A`. (This operation has $O(n)$ time complexity, where $n$ is the length of the array.)  
- Understand how copy works, i.e., the difference between `B = A` and `B = list(A)`.
	- Understand what a deep copy is, and how it differs from a shallow copy, i.e., how `copy.copy(A)` differs from `copy.deepcopy(A)`.

**Key Methods**
- `min(A)`,
- `max(A)`
- binary search for sorted lists (`bisect.bisect(A, 6)`, `bisect.bisect-left(A, 6)`, and `bisect.bisect_right(A, 6)`),
- `A.reverse()` (in-place)
- `reversed(A)` (returns an iterator),
- `A.sort()` (in-place)
- `sorted(A)` (returns a copy)
- `del A[i]` (deletes the $i$-th element),
- `del A[i:j]` (removes the slice).

 - Slicing is a very succinct way of manipulating arrays. 
	 - It can be viewed as a generalization of indexing.
	 - The most general form of slice is `A[i:j:k]`, with all being optional. 
	 - Slicing can also be used to rotate a list `A[k:] + A[:k]` rotates `A` by `k` to the left.
	 - It can also be used to create a copy: `B = A[:]` does a (shallow) copy of `A` into `B`.

Python provides a feature called list comprehension that is a succinct way to create lists. A list comprehension consists of (1.) an input sequence, (2.) an iterator over the input sequence, (3.) a logical condition over the iterator (this is optional), and (4.) an expression that yields theelementsof thederivedlist. Forexample, [x**2 for x in range(6)J yields [0, 1, 4, 9, 16, 25l,and[x**2 for x in range(6) if x%2 == 0]yields[0, 4, 16].

Although list comprehensions can always be rewritten using mapO, filterO, and lamb- das, they are clearer to read, in large part because they do not need lambdas.

List comprehension supports multiple levels of looping. This can be used to create the productof sets,e.g.,ifA - [1, 3, 5] andB = ['d', 'b'],then [(x, y) for x in A for y in B] creates [(1, 'a'), (1, 'b'), (3, 'a'), (3, 'b'), (5, 'a'), (5, 'b')]. It can alsobeusedtoconverta2DlisttoalDlist,e.g.,ifltl = [['a', 'b', 'c'], ['d', 'e',

'f'7f ,x for row in 11 for x in rowcreates ['a', 'b', 'c', 'd', 'e', 'f ']. TWolev- els of looping also allow for iterating over each entry in a 2D list, e.g., lf A = [[1, 2, 3f , [4, 5, 6]l then [[xoo2 for x in row] for row in ]Il yields [[1, 4, 9], [16, 25 , 361 1.

As a general rule, it is best to avoid more than two nested comprehensions, and use conventional nested for loops-the indentation makes it easier to read the program.

Finally, sets and dictionaries also support list comprehensions, with the same benefits.




## Strings

### Libraries
- `s[3]`
- `len(s)`
- `s + t`
- `s[2:4]`
- `s.startswith(prefix)`
- `s.endswith(suffix)`
- `'Euclid,Axiom 5,Parallel Lines'.split(')`
- `3 * '01'`
- `','.join(('Gauss', 'Prince of Mathematics', '1777-1855'))`
- `s.tolower()`
- `'Name {name}, Rank {rank}'.format(name='Archimedes', rank=3)`

### Top Tips for Strings
- Similar to arrays, string problems often have simple brute-force solutions that use $O(n)$ space solution, but subtler solutions that use the string itself to **reduce space complexity** to $O(1)$. 
- Understand the **implications** of a string type which is **immutable**, e.g., the need to allocate a new string when concatenating multiple strings. Know **alternatives** to immutable strings, e.g., a `list` in Python.
- Updating a mutable string from the front is slow, so see if it's possible to **write values from the back**.
## Linked Lists
### Overview
- A list is similar to an array in that it contains objects in a linear order.
- The key differences are that inserting and deleting elements in a list has time complexity $O(1)$.
- On the other hand, obtaining the $k$-th element in a list is expensive, having $O(n)$ time complexity.

```python
class ListNode:
	def __init__(self, data=0, next_node=None):
		self.data = data
		self.next = next_node

def search_list(L, key):
	while L and L.data != key:
		L = L.next  
	# If key was not present in the List, L will have become null
	return L

# Insert new_node after node.
def insert_after(node, new_node):
	new_node.next = node.next
	node.next = new_node

# DeTete the node past this one. Assume node is not a tail
def delete_after(node):
	node.next = node.next.next
```

### Top Tips for Linked Lists
- List problems often have a simple brute-force solution that uses $O(N)$ space, but a subtler solution that uses the **existing list nodes** to reduce space complexity to $O(1)$.
- Very often, a problem on lists is conceptually simple, and is more about **cleanly coding what's specified**, rather than designing an algorithm.
- Consider using a **dummy head** (sometimes referred to as a sentinel) to avoid having to check for empty lists. This simplifies code, and makes bugs less likely.
- It's easy to forget to **update next** (and previous for double linked list) for the head and tail.
- Algorithms operating on singly linked lists often benefit from using **two iterators**, one ahead of the other, or one advancing quicker than the other.

## Stacks and Queues

### Stacks
- A stack supports two basic operations - push and pop.
- Elements are added (pushed) and removed (popped) in last-in, first-out order.
- If the stack is empty, pop typically returns null or throws an exception. 
#### Implementation
- When the stack is implemented using a linked list these operations have $O(1)$ time complexity.
- If it is implemented using an array, there is maximum number of entries it can have-push and pop are still $O(1)$.
- If the array is dynamically resized, the amortized time for both push and pop is $O(1)$.
- A stack can support additional operations such as peek, which returns the top of the stack without popping it.

#### Top Tips for Stacks
- Learn to recognize when the stack **LIFO** property is applicable. For example, **parsing** typically benefits from a stack.
- Consider **augmenting** the basic stack or queue data structure to support additional operations, such as finding the maximum element.

#### Stack Libraries
- `s.append(e)` pushes an element onto the stack. Not much can go wrong with a call to push.
- `s[-1]` will retrieve, but does not remove, the element at the top of the stack.
- `s.pop()` will remove and return the element at the top of the stack.
- `len(s) == 0` tests if the stack is empty.
- When called on an empty list `s`, both `s[-1]` and `s.pop()` raise an `IndexError` exception.

### Queues
- A queue supports two basic operations - enqueue and dequeue.
	- If the queue is empty, dequeue typically returns null or throws an exception.
- Elements are added (enqueued) and removed (dequeued) in first-in, first-out order.
- The most recently inserted element is referred to as the tail or back element, and the item that was inserted least recently is referred to as the head or front element.

#### Implementation
- A queue can be implemented using a linked list, in which case these operations have $O(1)$ time complexity.
- The queue API often includes other operations.
	- a method that returns the item at the head of the queue without removing it
	- a method that returns the item at the tail of the queue without removing it, etc.
- A queue can also be implemented using an array.

#### Deque
- A deque, also sometimes called a double-ended queue, is a doubly linked list in which all insertions and deletions are from one of the two ends of the list, i.e., at the head or the tail.
- An insertion to the front is commonly called a push, and an insertion to the back is commonly called an inject.
- A deletion from the front is commonly called a pop, and a deletion from the back is commonly called an eject.
	- Different languages and libraries may have different nomenclature.

```python
class Queue:
	def __init__(self):
		self._data = collections.deque()
	
	def enqueue(self, x):
		self._data.append(x)
	
	def dequeue(self):  
		return self._data.popleft()
	
	def max(self):  
		return max(self.data)
```

#### Top Tips for Queues
- Learn to recognize when the queue property is applicable. For example, queues are ideal when order needs to be preserved.

#### Queue Libraries
- `q.append(e)` pushes an element onto the queue. Not much can go wrong with a call to push.
- `q[0]` will retrieve, but not remove, the element at the front of the queue.
- Similarly, `q[-1]` will retrieve, but not remove, the element at the back of the queue.
- `q.popleft()` will remove and return the element at the front of the queue.
- Dequeing or accessing the head/tail of an empty collection results in an `IndexError` exception being raised.

## Binary Trees

```python
class BinaryTreeNode:
	def __init__(data=None, left=None, right=None)
		self.data = data
		self.left = left
		self.right = right
```

The parent-child relationship defines an ancestor-descendant relationship on nodes in a binary tree.
- Specifically, a node is an *ancestor* of *d* if it lies on the search path from the root to *d*.
- If a node is an ancestor of *d*, we say *d* is a *descendant* of that node.
- Our convention is that a node is an ancestor and descendant of itself.
- A node that has no descendants except for itself is called a *leaf*.

### Attributes
- The *depth* of a node *n* is the number of nodes on the search path from the root to *n*, not including *n* itself.
- The *height* of a binary tree is the maximum depth of any node in that tree.
- A *level* of a tree is all nodes at the same depth.

### Types
- A *full* binary tree is a binary tree in which every node other than the leaves has two children.
- A *perfect* binary tree is a full binary tree in which all leaves are at the same depth, and in which every parent has two children.
- A *complete* binary tree is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible. (This terminology is not universal, e.g., some authors use complete binary tree where we write perfect binary tree.) 

### Traversals
A key computation on a binary tree is traversing all the nodes in the tree. 
Traversing is also sometimes called walking.
- Traverse the left subtree, visit the root, then traverse the right subtree (an *inorder* traversal).
- Visit the root, traverse the left subtree, then traverse the right subtree (a *preorder* traversal).
- Traverse the left subtree, traverse the right subtree, and then visit the root (a *postorder* traversal).

Let $T$ be a binary tree of $n$ nodes, with height $h$.
Implemented recursively, these traversals have $O(n)$ time complexity and $O(h)$ additional space complexity. (The space complexity is dictated by the maximum depth of the function call stack.)
If each node has a parent field, the traversals can be done with $O(1)$ additional space complexity.

### Top Tips for Binary Trees
- Recursive algorithms are well-suited to problems on trees. Remember to include space implicitly allocated on the **function call stack** when doing complexity analysis.
- Some tree problems have simple brute-force solutions that use $O(n)$ space, but subtler solutions that use the **existing tree nodes** to reduce space complexity to $O(1)$.
- Consider **left- and right-skewed trees** when doing complexity analysis. Note that $O(h)$ complexity, where $h$ is the tree height, translates into $O(log n)$ complexity for balanced trees, but $O(n)$ complexity for skewed trees.
- If each node has a **parent field**, use it to make your code simpler, and to reduce time and space complexity.
- It's easy to make the **mistake** of treating a node that has a **single child** as a leaf.


## Heaps

### Overview
- A *heap* is a specialized binary tree.
- Specifically, it is a complete binary tree. The keys must satisfy the *heap property* - the key at each node is at least as great as the keys stored at its children.
- A max-heap can be implemented as an array; the children of the node at index $i$ are at indices $2i + 1$ and $2i + 2$.
- A max-heap supports $O(log n)$ insertions, $O(1)$ time lookup for the max element, and $O(log n)$ deletion of the max element.
- The extract-max operation is defined to delete and return the maximum element.
- Searching for arbitrary keys has $O(n)$ time complexity.
- A heap is sometimes referred to as a priority queue because it behaves like a queue, with one difference: each element has a "priority" associated with it, and deletion removes the element with the highest priority.
- The *min-heap* is a completely symmetric version of the data structure and supports $O(1)$ time lookups for the minimum element.

### Top Tips for Heaps
- Use a heap when **all you care about** is the **largest** or **smallest** elements, and you **do not need** to support fast lookup, delete, or search operations for arbitrary elements.
- A heap is a good choice when you need to compute the $k$ **largest** or $k$ **smallest** elements in a collection. For the former, use a min-heap, for the latter, use a max-heap.
### Heap Libraries
Heap functionality in Python is provided by the `heapq` module.
- `heapq.heapify(L)` which transforms the elements in `L` into a heap in-place
- `heapq.nlargest(k, L)` (`heapq.nsmallest(k, L)`) returns the $k$ largest (smallest) elements in `L`
- `heapq.heappush(h, e)` which pushes a new element on the heap, 
- `heapq.heappop(h)`, which pops the smallest element from the heap
- `heapq.heappushpop(h, a)` which pushes `a` on the heap and then pops and returns the smallest element
- `e = h[0]` returns the smallest element on the heap without popping it.

**It's very important to remember that `heapq` only provides min-heap functionality.** 
If you need to build a max-heap on integers or floats, insert their negative to get the effect of a max-heap using `heapq`.
For objects, implement `__lt__(self, obj)` appropriately.


## Searching

### Top Tips for Searching
- **Binary search** is an effective search tool. It is applicable to more than just searching in **sorted arrays**, e.g., it can be used to search an **interval of real numbers or integers**.
- If your solution uses sorting, and the computation performed after sorting is faster than sorting, e.g., $O(n)$ or $O(log n)$, **look for solutions that do not perform a complete sort**.
- Consider **time/space tradeoffs**, such as making multiple passes through the data.

### Search Libraries
The `bisect` module provides binary search functions for sorted list. Specifically, assuming `a` is a sorted list.
- To find the first element that is not less than a targeted value, use `bisect.bisect_left(a, x)`. This call returns the index of the first entry that is greater than or equal to the targeted value. If all elements in the list are less than `x`, the returned value is `len(a)`.
- To find the first element that is greater than a targeted value, use `bisect.bisect_right(a,x)`. This call returns the index of the first entry that is greater than the targeted value. If all elements in the list are less than or equal to `x`, the returned value is `len(a)`.

## Hash Tables

A hash table is a data structure used to store keys, optionally, with corresponding values.
Inserts, deletes and lookups run in $O(1)$ time on average.

A hash function has one hard requirement - equal keys should have equal hash codes.
A softer requirement is that the hash function should "spread" keys, i.e., the hash codes for a subset of objects should be uniformly distributed across the underlying array.

### Top Tips for Hash Tables
- Hash tables have the **best theoretical and real-world performance** for lookup, insert and delete. Each of these operations has a $O(1)$ time complexity. The $O(1)$ time complexity for insertion is the average case - a single insert can take $O(n)$ if the hash table has to be resized.
- Consider using a hash code as a **signature** to enhance performance, e.g., to filter out candidates
- Consider using a precomputed `lookup table` instead of boilerplate if-then code for mappings, e.g., from character to value, or character to character.
- When defining your own type that will be put in a hash table, be sure you understand the relationship between **logical equality** and the fields the hash function must inspect. Specifically, anytime equality is implemented, it is imperative that the correct hash function is also implemented, otherwise when objects are placed in hash tables, logically equivalent objects may appear in different buckets, leading to lookups returning false, even when the searched item is present.
- Sometimes you'll need a `multimap`, i.e., a map that contains multiple values for a single key, or a bi-directional map. If the language's standard libraries do not provide the functionality you need, learn how to implement a multimap using lists as values, or find a **third party library** that has a multimap.

### Hash Table Libraries

#### Counters
```python
c = collections.Counter(a=3, b=1)  
d = collections.Counter(a=1, b=2)  
# add two counters together: c[x] + d[x], collections.Counter({'a': 4, 'b': 3})
c + d
# subtract (keeping only positive counts), collections.Counter({'a': 2})
c - d  
# intersection: min(c[x], d[x]), collections.Counter({'a': 1, 'b': 1})
c & d
# union: max(c[x], d[x]), collections.Counter({'a': 3, 'b': 2})
c | d
# del actually removes the entry
del c['sausage']                        
```

#### `set`
The most important operations for `set` are `s.add(42)`, `s.remove(42)`, `s.discard(123)`, `x in s` as well as `s <= t` (is `s` a subset of `t`), and `s - t` (elements in `s` that are not in `t`).

#### Iterators
 - Iteration over a key-value collection yields the keys.
 - To iterate over the key-value pairs, iterate over `items()`.
 - To iterate over values, use `values()`.
 - The `keys()` returns an iterator to the keys.

#### "Unhashable" Types
Not every type is "hashable", i.e., can be added to a `set` or used as a key in a `dict`. In particular, mutable containers are not hashable - this is to prevent a client from modifying an object after adding it to the container, since the lookup will then fail to find it if the slot that the modified object hashes to is different.

#### `hash`
Note that the built-in `hash()` function can greatly simplify the implementation of a hash function for a user-defined class, i.e., implementing `__hash(self)__`.


## 13. Sorting

### Problems
- [x] 13.1
- [x] 13.2
- [x] 13.5
- [x] 13.7
- [ ] 13.10
- [ ] _13.8_

### Sorting Boot Camp

```python
class Student(object):
	def __init__(self, name, grade_point_average):
		self.name = name
		self.grade_point_average = grade_point_average

	def __lt__(self, other):
		return self.name < other.name

students = [
	Student('A', 4.0),
	Student('C', 3.0),
	Student('B', 2.0),
	Student('D', 3.2),
]


# Sort according to __lt__ defined in Student. students remained unchanged.
students_sort_by_name = sorted(students)

# Sort students in-pIace by grade_point_average.  
students.sort(key=lambda student: student.grade_point_average)
```

The time complexity of any reasonable library sort is $O(n log n)$ for an array with $n$ entries.
Most library sorting functions are based on quicksort, which has $O(1)$ space complexity.

### Top Tips for Sorting
- Sorting problems come in two flavors: (1.)  **use sorting to make subsequent steps in an algorithm simpler**, and (2.) design a **custom sorting routine**. For the former, it's fine to use a library sort function, possibly with a custom comparator. For the latter, use a data structure like a BST, heap, or array indexed by values.
- Certain problems become easier to understand, as well as solve, when the input is sorted. The most natural reason to sort is if the inputs have a **natural ordering**, and sorting can be used as a preprocessing step to **speed up searching**.
- For **specialized input**, e.g., a very small range of values, or a small number of values, it's possible to sort in $O(n)$ time rather than $O(n \log_{}(n))$ time.
- It's often the case that sorting can be implemented in **less space** than required by a brute-force approach.
- Sometimes it is not obvious what to sort on, e.g., should a collection of intervals be sorted on starting points or endpoints?

### Sorting Libraries

To sort a list in-place, use the `sort()`; to sort an iterable, use the function `sorted()`.
- The `sort()` method implements a stable in-place sort for list objects. It returns `None` - the calling list itself is updated. It takes two arguments, both optional: `key=None`, and `reverse=False` If `key` is not `None`, it is assumed to be a function which takes list elements and maps them to objects which are comparable - this function defines the sort order.
- The `sorted` function takes an iterable and return a new list containing all items from the iterable in ascending order. The original list is unchanged.

## 14. Binary Search Trees

### Problems
- [x] 14.1
- [x] 14.2
- [x] 14.3
- [x] 14.4
- [ ] 14.5 (Read)
- [ ] 14.8
- [ ] _14.7_

### Overview
- The keys stored at nodes have to respect the BST property - the key stored at a node is greater than or equal to the keys stored at the nodes of its left subtree and less than or equal to the keys stored in the nodes of its right subtree.
- Key lookup, insertion, and deletion take time proportional to the height of the tree, which can in worst-case be $O(n)$, if insertions and deletions are naively implemented.
- However, there are implementations of insert and delete which guarantee that the tree has height $O(\log(n))$.
- These require storing and updating additional data at the tree nodes.
- Red-black trees are an example of height-balanced BSTs and are widely used in data structure libraries.

```python
class BSTNode:
	def __init__(data=None, left=None, right=None):
		self.data, self.left, self.right = data, left, right

def search_bst(tree, key):
	return (tree
			if not tree or tree.data == key else search_bst(tree.left, key)
			if key < tree.data else search_bst(tree.right, key))
```

### Top Tips for Binary Search Trees
- With a BST you can **iterate** through elements in **sorted order** in time $O(n)$ (regardless of whether it is balanced).
- Some problems need a **combination of a BST and a hashtable**. For example, if you insert student objects into a BST and entries are ordered by GPA, and then a student's GPA needs to be updated and all we have is the student's name and new GPA, we cannot find the student by name without a full traversal. However, with an additional hash table, we can directly go to the corresponding entry in the tree.
- Sometimes, it's necessary to **augment** a BST to make it possible to manipulate more complicated data, e.g., intervals, and efficiently support more complex queries, e.g., the number of elements in a range.
- The BST property is a **global property** - a binary tree may have the property that each node's key is greater than the key at its left child and smaller than the key at its right child, but it may not be a BST.

### Binary Search Tree Libraries

- Python does **not** come with a built-in BST library.
- The `sortedcontainers` module the best-in-class module for sorted sets and sorted dictionaries
	- The underlying data structure is a sorted list of sorted lists.
	- Its asymptotic time complexity for inserts and deletes is $O(\sqrt(n))$ since these operations entail insertion into a list of length roughly $\sqrt(n)$, rather than $O(\log(n))$ of balanced BSTs.
	- In practice, this is not an issue, since CPUs are highly optimized for block data movements.

```python
t = bintrees.RBTree([(5, 'Alfa'), (2, 'Bravo'), (7, 'Charlie'), (3, 'Delta'), (6, 'Echo')])

print(t[2]) # 'Bravo'

print(t.min_item(), t.max_item()) # (2, 'Bravo'), (7, 'Charlie')

# {2: 'Bravo', 3: 'Delta', 5: 'Alfa', 6 'Echo', 7: 'Charlie', 9: 'Golf'] 
t.insert(9, 'Golf')  
print(t)

print(t.min_key(), t.max_key()) # 2, 9

t.discard(3)  
print(t) # {2: 'Bravo', 5: 'Alfa', 6: 'Echo', 7: 'Charlie', 9: 'Golf'}

# a = (2: 'Bravo')  
a = t.pop_min()  
print(t) # {5: 'Alfa', 6: 'Echo', 7: 'Charlie', 9: 'Golf'}

# b = (9, 'Golf')  
b = t.pop_max()
print(t) # {5: 'Alfa', 6: 'Echo', 7: 'Charlie'}
```





## 15. Recursion

### Problems
- [x] 15.1
- [x] 15.2
- [x] 15.3
- [ ] 15.4
- [ ] 15.9
- [ ] 15.6
- [ ] _15.10_
### Overview
- Divide-and-conquer is **not** synonymous with recursion.
	- In divide-and-conquer the problem is divided into two or more independent smaller problems that are of the same type as the original problem.
	- Recursion is more general:
		- There may be a single subproblem (e.g., binary search).
		- The subproblems may not be independent (e.g., dynamic programming).
		- They may not be of the same type as the original (e.g., regular expression matching).
	- In addition, sometimes to improve runtime, and occasionally to reduce space complexity, a divide-and-conquer algorithm is implemented using iteration instead of recursion.
### Top Tips for Recursion
- Recursion is especially suitable when the **input is expressed using recursive rules** such as a computer grammar.
- Recursion is a good choice for **search**, **enumeration**, and **divide-and-conquer**.
- Use recursion as **alternative to deeply nested iteration loops**. For example, recursion is much better when you have an undefined number of levels, such as the IP address problem generalized to $k$ substrings.
- If you are asked to **remove recursion** from a program, consider mimicking call stack with the **stack data structure**.
- Recursion can be easily removed from a **tail-recursive** program by using a while-loop - no stack is needed. (Optimizing compilers do this.)
- If a recursive function may end up being called with the same arguments more than once, **cache** the results - this is the idea behind Dynamic Programming (Chapter 16).


## 16. Dynamic Programming

### Problems
- [ ] 16.1
- [ ] 16.2
- [ ] 16.3
- [ ] 16.6
- [ ] 16.5
- [ ] 16.7
- [ ] _16.12_

### Overview
- DP is a general technique for solving optimization, search, and counting problems that can be decomposed into subproblems.
- You should consider using DP whenever you have to make choices to arrive at the solution, specifically, when the solution relates to subproblems.
- Like divide-and-conquer, DP solves the problem by combining the solutions of multiple smaller problems, but what makes DP different is that the same subproblem may reoccur.
- Therefore, a key to making DP efficient is caching the results of intermediate computations.

### Top Tips for Dynamic Programming
- Consider using DP you to **make choices** to arrive at the solution. Specifically, DP is applicable when you can construct a solution to the given instance from solutions to sub-instances of smaller problems of the same kind.
- In addition to optimization problems, DP is also **applicable** to **counting and decision problems** - any problem where you can express a solution recursively in terms of the same computation on smaller instances.
- Although conceptually DP involves recursion, often for efficiency the cache is **built "bottom-up"**, i.e., iteratively.
- When DP is implemented recursively the cache is typically a dynamic data structure such as a hash table or a BST; when it is implemented iteratively the cache is usually a one- or multi-dimensional array.
- To save space, **cache space** may be **recycled** once it is known that a set of entries will not be looked up again.
- Sometimes, **recursion may out-perform a bottom-up DP** solution, e.g., when the solution is found early or subproblems can be pruned through bounding.  
  A common **mistake** in solving DP problems is trying to think of the recursive case by splitting the problem into **two equal halves**, *a la* quicksort, i.e., solve the subproblems for subarrays and combine the results. However, in most cases, these two subproblems are not sufficient to solve the original problem.
- DP is based on **combining solutions** to subproblems to **yield a solution** to the original problem. However, for some problems DP will not work. For example, if you need to compute the longest path from City 1 to City 2 without repeating an intermediate city, and this longest path passes through City 3, then the subpaths from City 1 to City 3 and City 3 to City 2 may not be individually longest paths without repeated cities.

## [ ] 17. Greedy Algorithms and Invariants
17.4 17.6 17.5 17.7 _17.8_
## [ ] 18. Graphs
18.1 18.7 18.2 18.3 _18.5_

## [ ] 19. Parallel Computing
19.3 19.6 19.8 19.9

## [ ] 20. Design Problems
20.9 20.13 20.15 20.16 20.1 20.2

## [ ] 21. Language Questions

## [ ] 22. Object-Oriented Design

## [ ]  23. Common Tools

## [ ] 24. Honors Class
# Personal

## Top Tips
- Read the problem **multiple times**. Identify if there are any key phrases that may affect an implementation (e.g., "an array of **distinct** integers").
- **Always** write a solution in pseudocode before attempting to write code. Otherwise, you may run in to issues that could have been resolved earlier. 


https://docs.python.org/3/library/functools.html#functools.reduce

Apply _function_ of two arguments cumulatively to the items of _iterable_, from left to right, so as to reduce the iterable to a single value. For example, `reduce(lambda x, y: x+y, [1, 2, 3, 4, 5])` calculates `((((1+2)+3)+4)+5)`. The left argument, _x_, is the accumulated value and the right argument, _y_, is the update value from the _iterable_. If the optional _initializer_ is present, it is placed before the items of the iterable in the calculation, and serves as a default when the iterable is empty. If _initializer_ is not given and _iterable_ contains only one item, the first item is returned.

- Know `lambda` notation. Use it. You can use `lambda`s within other data structures.

## Named Tuples

```python
BalancedStatusWithHeight = collections.namedtuple('BalancedStatusttithHeight', ('balanced', 'height'))
example = BalancedStatuswithHeight(True, -1)
```

## Dictionary Comprehension

```python
node_to_inorder_idx = {data: i for i, data in enunerate(inorder)}
```
- `enumerate` returns the index and data as a tuple.
