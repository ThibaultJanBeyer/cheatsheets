[back to overwiev](/../..)

If you have a stock-chart, what would be the best day to invest in

# Algorithms & Data Structures CheatSheet
in JavaScript / TypeScript

It is based on a [Frontent Masters course](https://frontendmasters.com/courses/algorithms/) from ThePrimeagen.

## Table of Contents

- [Arrays](#arrays)
- - [Arrays vs linked lists](#arrays-vs-linked-lists)
- - [Array lists](#array-lists)
- - [ArrayBuffer / RingBuffer](#arraybuffer--ringbuffer)
- [Searching](#searching)
- - [Linear Search](#linear-search)
- - [Binary Search](#binary-search)
- - [Crystal Balls Exercise](#crystal-balls-exercise)
- [Sorting](#sorting)
- - [Bubble sort](#bubble-sort)
- - [Divide and Conquer](#divide-and-conquer)
- - [Quick sort](#quick-sort)
- - [Merge sort](#merge-sort)
- [Linked Lists](#linked-lists)
- - [Queue](#queue)
- - [Stack](#stack)
- - [Double Linked Lists](#double-linked-lists)
- [Recursion](#recursion)
- - [Examples](#examples)
- - - [Maze Solver](#maze-solver)
- - - [Green houses](#green-houses)
- [Trees](#trees)
- - [Tree Traversal](#tree-traversal)
- - - [Depth First Search](#depth-first-search)
- - - - [Pre-Order](#pre-order)
- - - - [In-Order](#in-order)
- - - - [Post-Order](#post-order)
- - - - [Compare Trees](#compare-trees)
- - - - [Lowest Common Ancestor](#lowest-common-ancestor)
- - - - [Binary Search Tree](#binary-search-tree)
- - - [Breath First Search](#breath-first-search)
- - - - [BFS Loop](#bfs-loop)
- - - - [BFS Recurse](#bfs-recursive)
- - - - [BFS Level by Level](#bfs-level-by-level)
- [Heap / Priority Queue](#heap--priority-queue)
- [Trie / Prefix Tree / Digital Tree](#trie--prefix-tree--digital-tree)
- [Graphs](#graphs)
- - [Adjacency List](#adjacency-list)
- - [Adjacency Matrix](#adjacency-matrix)
- - [Searching the Graph](#searching-the-graph)
- - - [Breath First Search Matrix](#breath-first-search-matrix)
- - - [Depth First Search List](#depth-first-search-list)
- - - [Dijkstra Shortest Path in Graph](#dijkstra-shortest-path-in-graph)

## Arrays

In javascript you’d think this `[]` is an array. Well you’re wrong, it’s actually an [Array List](#array-lists).

- An Array is just a contiguous memory space (contiguous = non breaking)
- It’s just a chunk of memory that is understood as an array

Here is how you could build an array in node:

```JavaScript
const a = new ArrayBuffer(6)
// ArrayBuffer { [Uint8Contents]: <00 00 00 00 00 00>, byteLength: 6 }
const a8 = new Uint8Array(a) // numbers between 0 and 255. Creates a view into the array buffer, does not create anything new
a8[0] = 45
a8[2] = 45
// ArrayBuffer { [Uint8Contents]: <2d 00 2d 00 00 00>, byteLength: 6 }
const a16 = new Uint16Array(a) // numbers between 0 and 255. Creates a view into the array buffer, 
a16[2] = 0x4545
// ArrayBuffer { [Uint8Contents]: <2d 00 2d 00 45 45>, byteLength: 6 }
// because I look at it in 16bit unity, I can only walk in 16 units at a time, this is why the 45 45 was placed in the last bits
```

### Arrays vs linked lists
- I’s simple
- You can’t add a value because there is only override so you’ll have to shift over your values to write something in between
- O(1) everything for Arrays
- Memory has to be allocated in advance for arrays. Linked lists have better memory usage but more computational
- In a list you have to always traverse the list (linear search)
- Linked Lists are usually the better choice for queues, Array Lists for stacks, or Array Buffer for both

### Array lists
Array access with the ability to grow:

```
[1, , ] Length: 1, Capacity 3 => .push(2) => [1,2, ] Length: 2, Capacity 3 
Getting: if index is >= length throw/undefined
Push: is length in capacity? Great just add and increment length. No? Create new bigger array and move items over.
Pop: length -1 get the value, decrement the length (might need to zero out the data).
Enqueue: Grow capacity, move over all values one to the right
dequeue: move over all values one to the left
```

- Get O(1)
- Really good for push and pop O(1)
- Really bad for enqueue and dequeue O(n)
- Getting something is great just add/remove on the beginning is not so great (but it is in a linked list)

### ArrayBuffer / RingBuffer

- Head & Tail are not in the beginning nor end of the array, the items are just within the head and the tail

```
[ , ,1,2, , ]
Dequeue: +1 to the head
Enqueue: -1 to the head
Push: +1 to the tail
Pop: -1 to the tail
```

- Push, pop, enqueue, dequeue are all O(1)
- If you max out the head or the tail you can, i.e. move the tail over to the front before the head, as long as there is space. Which is why it’s called "RingBuffer" (this.tail % length gives you then the position of the tail). I.e. if array length is 10 and you move the tail to position 12, you’ll find it at 12 % 10 = 2 (remaining)
- If the tail is at the head you need to resize: create a new buffer with more capacity and write the old to the new one

## Searching

### Linear Search

Searching a list linearly in JavaScript:

```TypeScript
export default function linear_search(
    haystack: number[],
    needle: number,
): boolean {
    for (let i = 0, il = haystack.length; i < il; i++)
        if (haystack[i] === needle) return true;

    return false;
}
```

- Time Complexity is `O(N)`

### Binary Search

As a recursive method

```TypeScript
export default function bs_list(haystack: number[], needle: number): boolean {
  const midpoint = Math.floor(haystack.length / 2)
  const value = haystack[midpoint]
  if (value === needle) return true
  if (haystack.length === 1) return false
  if (value < needle) return bs_list(haystack.slice(midpoint), needle)
  if (value > needle) return bs_list(haystack.slice(0, midpoint), needle)

  return false
}
```

with a loop

```TypeScript
export default function bs_list(haystack: number[], needle: number): boolean {
  let low = 0;
  let high = haystack.length

  do {
    const midpoint = Math.floor(low + (high - low) / 2)
    const value = haystack[midpoint]
    if (value === needle) return true
    else if (value > needle) high = midpoint
    else low = midpoint + 1
  } while (low < high)

  return false
}
```

- Time Complexity is `log(N)`

### Crystal Balls Exercise

Given 2 crystal balls that will break when dropped from high enough. Determine the exact spot where they will break in the best way.

- Going one by one is O(n) : [1, 2, BREAK*, …]
- Going with a binary search is O(n) : B1: […, BREAK_1, …] | B2: [1, 2, BREAK_2*, …, BREAK_1, …] we still need to loop for the second one since we only have 2 balls
- So the most optimized way would be to jump in X steps

```JavaScript
export default function two_crystal_balls(breaks: boolean[]): number {

  const jumps = Math.sqrt(breaks.length)
  let ball1 = 0

  for (; ball1 < breaks.length; ball1 += jumps)
    if(breaks[ball1]) break

  for (let ball2 = ball1 - jumps; ball2 < ball1; ball2++)
    if(breaks[ball2]) return ball2

  return -1

}
```

- Time Complexity is `O(√N)`

## Sorting

### Bubble sort

- It just goes to the next element in the list and asks "hey are you smaller than me?" if yes, switch positions.
- A singular iteration will always put the largest item at the last spot. So next bubble sort does not need to include the last position of the biggest element.
- The runtime is `O(n^2)`

```JavaScript
export default function bubble_sort(arr: number[]): void {
  for (let searched = 1; searched < arr.length; searched++)
    for (let i = 0, il = arr.length - searched; i < il; i++)
      if (arr[i] > arr[i + 1]) {
        const temp = arr[i]
        arr[i] = arr[i + 1]
        arr[i + 1] = temp
      }
}
```
### Divide and Conquer

- Split the input into sub-sets, go over the sub-sets to solve it faster.
- An array of 1 element is always sorted

### Quick Sort

```
           [0, 31]
           /  16  \
      [0, 15]   [17, 31]
      /  8  \    / 24  \
       ………………etc………………
    /\/\/\/\/\/\/\/\/\/\/\
all of the last pieces will be sorted
```

- Runtime O(logN) – O(n^2) (depending on the input, i.e. a reverse sorted array [5,4,3,2,1])

Simple implementation (runtime O(n) - O(n^2)):
```TypeScript
export default function quick_sort(arr: number[]): number[] {
  if (arr.length <= 1) return arr

  const pivot = arr[arr.length - 1]
  const leftArr: number[] = []
  const rightArr: number[] = []
  for (let i = 0; i < arr.length - 1; i++) {
    if (arr[i] < pivot) leftArr.push(arr[i])
    else rightArr.push(arr[i])
  }

  return [...quick_sort(leftArr), pivot, ...quick_sort(rightArr)]
}
```

In-Place replacement implementation (runtime O(logN) - O(n^2)):
```TypeScript
function qs(arr: number[], lo: number, hi: number): void {
  if (lo >= hi) return

  const pivotIndex = partition(arr, lo, hi) // does the weak sort, put it in the spot, returns index
  qs(arr, lo, pivotIndex - 1)
  qs(arr, pivotIndex + 1, hi)
}

function partition(arr: number[], lo: number, hi: number): number {
  const pivot = arr[hi]
  let index = lo - 1

  console.log(pivot)
  for (let i = lo; i < hi; i++) {
    if (arr[i] <= pivot) {
      index++
      const tmp = arr[i]
      console.log(tmp)
      arr[i] = arr[index]
      arr[index] = tmp
    }
  }

  ++index
  arr[hi] = arr[index]
  arr[index] = pivot

  return index
}

export default function quick_sort(arr: number[]): void {
  qs(arr, 0, arr.length - 1)
}
```

### Merge Sort

```TypeScript
const sortArrays = (a: number[], b: number[]): number[] => {
  const c: number[] = []

  while (a.length && b.length)
    if (a[0] < b[0]) c.push(a.shift()) else c.push(b.shift())

  return [...c, ...a, ...b]
}

export default function mergeSort(arr: number[]): number[] {
  if (arr.length <= 1) return arr

  const middle = Math.floor(arr.length / 2)
  const leftArr = arr.slice(0, middle)
  const rightArr = arr.slice(middle)

  return sortArrays(mergeSort(leftArr), mergeSort(rightArr))
}
```

- Complexity O(n^log(n))

## Linked Lists

- Good for insertions / removals in the first or last node
- Tricky to traverse

### Queue

First in, first out (FiFo)

```
head            tail
1 ->  2 ->  3 ->  4
```

```TypeScript
type Node<T> = {
    value: T
    next?: Node<T> 
}

export default class Queue<T> {
    public length: number
    private head?: Node<T>
    private tail?: Node<T>

    constructor() {
        this.head = this.tail = undefined
        this.length = 0
    }

    // add node to the queue: point the current tail to the new node, set the tail to the new node
    enqueue(item: T): void {
        const node = { next: undefined, value: item }
        this.length++
        if (!this.tail) {
            this.tail = this.head = node
            return
        }
        this.tail.next = node
        this.tail = node
    }

    // remove node from the queue: set the current head to the next node, remove the previous heads connection
    dequeue(): T | undefined {
        if (!this.head) return undefined
        this.length--

        const head = this.head
        this.head = this.head.next

        // free
        head.next = undefined

        if(this.length === 0) this.tail = undefined

        return head.value
    }

    // get the heads value
    peek(): T | undefined {
        return this.head?.value
    }
}
```

### Stack

Last in, First out (LiFo)

```
3    head
v
2
v
1    tail
```

```TypeScript
type Node<T> = {
    value: T
    next?: Node<T>
}

export default class Stack<T> {
    public length: number;
    private head?: Node<T>

    constructor() {
        this.head = undefined
        this.length = 0
        return
    }

    // Add to the stack: point new node to next head, set head to new node
    push(item: T): void {
        const node = { value: item, next: undefined } as Node<T>
        this.length++
        if (!this.head) {
            this.head = node
            return
        }
        node.next = this.head
        this.head = node
    }

    // Remove from stack: set head to next node, remove pointer of prev node
    pop(): T | undefined {
        if (!this.head) return undefined
        this.length--
        const prevHead = this.head
        this.head = prevHead.next
        prevHead.next = undefined
        return prevHead.value
    }

    peek(): T | undefined {
        return this.head?.value
    }
}
```

### Double Linked Lists

```TypeScript
type Node<T> = {
  value: T
  prev?: Node<T>
  next?: Node<T>
}

export default class DoublyLinkedList<T> {
  public length: number
  private head?: Node<T>
  private tail?: Node<T>

  constructor() {
    this.length = 0
    this.head = this.tail = undefined
  }

  prepend(item: T): void {
    const node = { value: item } as Node<T>
    ++this.length
    if (!this.head) {
      this.head = this.tail = node
      return
    }

    node.next = this.head
    this.head.prev = node
    this.head = node
  }

  insertAt(item: T, idx: number): void {
    if (idx > this.length) throw new Error("Oops")
    if (idx === this.length) this.append(item)
    if (idx === 0) this.prepend(item)
    else {
      const curr = this.getAt(idx)
      const node = { value: item } as Node<T>
      node.next = curr
      node.prev = curr?.prev
      if (node.prev) node.prev.next = node
      if (curr) curr.prev = node

      ++this.length
    }
  }

  append(item: T): void {
    ++this.length
    const node = { value: item } as Node<T>
    if (!this.tail) {
      this.head = this.tail = node
      return
    }

    node.prev = this.tail
    this.tail.next = node
    this.tail = node
  }

  remove(item: T): T | undefined {
    let curr = this.head
    for (
      let index = 0;
      curr && curr.value !== item && index < this.length;
      index++
    )
      curr = curr.next
    return this.removeNode(curr)
  }

  get(idx: number): T | undefined {
    return this.getAt(idx)?.value
  }

  getAt(idx: number): Node<T> | undefined {
    let current = this.head
    for (let i = 0; i < idx && current; i++) current = current.next
    return current
  }

  removeAt(idx: number): T | undefined {
    const curr = this.getAt(idx)
    return this.removeNode(curr)
  }

  removeNode(node?: Node<T>): T | undefined {
    if (!node) return

    --this.length
    if (this.length === 0) {
      const out = this.head?.value
      this.head = this.tail = undefined
      return out
    }
    if (node.prev) node.prev.next = node.next
    if (node.next) node.next.prev = node.prev
    if (node === this.head) this.head = node.next
    if (node === this.tail) this.tail = node.prev
    node.prev = node.next = undefined

    return node.value
  }
}
```

## Recursion

Basic example:

```TypeScript
function foo(n: number): number {
  if(n === 1) return 1
  return n + foo(n - 1)
}

/** 
 *          ReturnAddress | ReturnValue | Arguments
 * foo(3)   Entry         | 3+?    3    | 3
 * foo(2)   foo(3)^       | 2+?  1 ^    | 2
 * foo(1)   foo(2)^       | 1    ^      | 1
 * 
 * foo(3) => Outcome is 3+3=6
 */
```

A Recursion can always be broken down in 3 steps:
- Pre (what can be done before)
- Recurse (calling of the function)
- Post (doing something else after recursion)

- Complexity O(N)

### Examples
#### Maze solver

```TypeScript
[
  "#####E#",
  "#     #",
  "#S#####"
]
```

Base cases
- It’s a wall
- Off the map
- It‘s the end
- If we have seen it

```TypeScript
const directions = [
    [-1, 0],
    [1, 0],
    [0, -1],
    [0, 1],
];

// Base Case
const walk = (
    maze: string[],
    seen: boolean[][],
    path: Point[],
    wall: string,
    end: Point,
    curr: Point,
): boolean => {
    // Off the map
    if (
        curr.x < 0 ||
        curr.x >= maze[0].length ||
        curr.y < 0 ||
        curr.y >= maze.length
    )
        return false;
    // On a wall
    if (maze[curr.y][curr.x] === wall) return false;
    // Already Seen
    if (seen[curr.y][curr.x]) return false;
    // The end
    if (curr.x === end.x && curr.y === end.y) {
        path.push(curr);
        return true;
    }

    // Pre
    seen[curr.y][curr.x] = true;
    path.push(curr);
    // Curr
    for (let index = 0; index < directions.length; index++) {
        const [x, y] = directions[index];
        if (walk(maze, seen, path, wall, end, { x: curr.x + x, y: curr.y + y }))
            return true;
    }
    // Post
    path.pop();

    return false;
};

export default function solve(
    maze: string[],
    wall: string,
    start: Point,
    end: Point,
): Point[] {
    const seen: boolean[][] = [];
    const path: Point[] = [];
    for (let index = 0; index < maze.length; index++)
        seen.push(new Array(maze[index].length).fill(false));

    walk(maze, seen, path, wall, end, start);
    return path;
}
```

#### Green houses

- The task is to get a district energy positive by installing solar panels.
- You can add either buy a solar panel X that turns the energy of one house to 0
- Or you buy a solar panel Y that converts the energy consumption of the house to energy production
- X and Y costs the same for any house
- Sum of energy of all houses should be <= 0
- => What is the cheapest way to make all houses non-positive?

Base Case:
- Use X
- Use Y

We actually need run through all possibilities, calculate all possible costs and return the cheapest. In a graph it would look like this: 

```
              0
           /     \
         X         Y
        /\         /\
       X  Y       X  Y
      /\  /\     /\  /\
     X Y  X Y   X Y  X Y
            …etc…
```

A self-building tree branching into infinity based of the amount of houses to walk through. Each house has 2 possibilities, hence it‘s a binary tree

```TypeScript
const traverse = (
  total: number,
  cost: number,
  A: number[],
  X: number,
  Y: number,
  index: number,
  isX: boolean
) => {
  if (!A[index]) return cost

  if (isX) {
    total -= A[index]
    cost += X
  } else {
    total -= A[index] * 2
    cost += Y
  }

  if (total <= 0) return cost

  index++
  const totalLeft = traverse(total, cost, A, X, Y, index, true) // left (X)
  const totalRight = traverse(total, cost, A, X, Y, index, false) // right (Y)

  return Math.min(totalLeft, totalRight)
}

function solution(A: number[], X: number, Y: number): number {
  
  // - Sort houses by biggest consumption as it makes most sense to get rid of those first
  A.sort((a, b) => b - a)
  
  // - Calculate total consumption
  const totalConsumption = A.reduce((prev, curr) => prev + curr)

  // - Traverse all possibilities
  const costsX = traverse(totalConsumption, 0, A, X, Y, 0, true)
  const costsY = traverse(totalConsumption, 0, A, X, Y, 0, false)
  
  // - Return the cheapest
  return Math.min(costsX, costsY)
}

console.log(solution([4, 2, 7], 4, 100)) // 12
console.log(solution([5, 3, 8, 3, 2], 2, 5)) // 7
console.log(solution([4, 2, 7], 4, 100)) // 12
console.log(solution([2, 2, 1, 2, 2], 2, 3)) // 8
console.log(solution([4, 1, 5, 3], 5, 2)) // 4
```

## Trees

- For example the DOM
- `Root` is the top most node
- `Height` is the longest path from the Root to the furthest away node
- `Leaf` is a node with no children
- Binary Tree is a tree that only has two nodes (left/right)
- Balanced Tree when the children have all the same height

### Tree Traversal

- Visit Node
- Recurse
- Pre-Order: Root at the beginning
- In-Order: Root in the middle
- Post-Order: Root in the end

#### Depth first search

- Like a `stack`
- Complexity O(N)
- Calling functions as/like a stack
- Will go as deep into the tree as possible on the left
- Preserves the shape of the tree!
- Post order:

```
      7
  23      3
5   4   18  21
Start at 7 => [7]
Add left child => [23]
                  [7]
Add left child => [5]
                  [23]
                  [7]
No more left, no more right =>    => (5)
                              [23]
                               [7]
Add right child => [4] => (5)
                  [23]
                   [7]
No more left, no more right => [] => (5,4)
                              [23]
                              [7]
No more left, no more right => [] => (5,4,23)
                              [7]
Add right child => [3] => (5,4,23)
                   [7]
Add left child => [18] => (5,4,23)
                   [3]
                   [7]
No more left, no more right => [] => (5,4,23,18)
                              [3]
                              [7]
Add right child => [21] => (5,4,23,18)
                    [3]
                    [7]
No more left, no more right => [] => (5,4,23,18,21)
                              [3]
                              [7]
No more left, no more right => [] => (5,4,23,18,21,3)
                              [7]
No more left, no more right => [] => (5,4,23,18,21,3,7)
```

##### Pre Order

```
      7
  23      3
5   4   18  21
=> [7, 23, 5, 4, 3, 18, 21]
```

```TypeScript
const traversal = (node: BinaryNode<number> | null, visited: number[]): number[] => {
  // base-case
  if (!node) return visited
  // pre
  visited.push(node.value)
  // recurse
  traversal(node.left, visited)
  traversal(node.right, visited)
  // post
  return visited
}

export default (head: BinaryNode<number>): number[] => traversal(head, [])
```
##### In Order

```
      7
  23      3
5   4   18  21
=> [5, 23, 4, 7, 18, 3, 21]
```

```TypeScript
const traversal = (node: BinaryNode<number> | null, visited: number[]): number[] => {
  // base-case
  if (!node) return visited
  // pre
  // recurse
  traversal(node.left, visited)
  visited.push(node.value)
  traversal(node.right, visited)
  // post
  return visited
}

export default (head: BinaryNode<number>): number[] => traversal(head, [])
```

##### Post Order

```
      7
  23      3
5   4   18  21
=> [5, 4, 23, 18, 21, 3, 7]
```

```TypeScript
const traversal = (node: BinaryNode<number> | null, visited: number[]): number[] => {
  // base-case
  if (!node) return visited
  // pre
  // recurse
  traversal(node.left, visited)
  traversal(node.right, visited)
  // post
  visited.push(node.value)
  return visited
}

export default (head: BinaryNode<number>): number[] => traversal(head, [])
```

##### Compare Trees

- Compare whether two trees are equal in values and shape

```TypeScript
const traversal = (
  a: BinaryNode<number> | null | undefined,
  b: BinaryNode<number> | null | undefined,
): boolean => {
  // base-case
  if (a?.value !== b?.value) return false
  if (!a?.left && !b?.left) return true
  // recurse
  return traversal(a?.left, b?.left) && traversal(a?.right, b?.right)
}

export default (
  a: BinaryNode<number> | null,
  b: BinaryNode<number> | null,
): boolean => traversal(a, b)
```

##### Lowest Common Ancestor

```TypeScript
const common = (head: BinaryNode<number>, x: number, y: number): number => {
  let lca: BinaryNode<number> = head

  const walk = (
    node: BinaryNode<number> | null,
    x: number,
    y: number,
  ): boolean => {
    // base-case
    const val = node?.value
    if (!val) return false
    // recurse
    const hasLeft = walk(node.left, x, y)
    const hasRight = walk(node.right, x, y)
    const current = val === x || val === y
    // post
    if (val === x && val === y) lca = node
    if (Number(hasLeft) + Number(hasRight) + Number(current) >= 2) lca = node
    return hasLeft || hasRight || current
  }

  walk(head, x, y)
  return lca.value
}

/**
 *          20
 *     10          50
 *   5    15     30    100
 *     7        29  45
 */
console.log(common(tree, 7, 15)) // 10
```

##### Binary Search Tree

- Given a binary tree whose left children are always smaller or equal to the parent and the right children always greater:

```TypeScript
const traversal = (
  node: BinaryNode<number> | null,
  needle: number,
): boolean => {
  // base-case
  if (!node) return false
  if (node.value === needle) return true
  // recurse
  if (needle < node.value) return traversal(node.left, needle)
  else return traversal(node.right, needle)
}

export default function dfs(head: BinaryNode<number>, needle: number): boolean {
  return traversal(head, needle)
}
```

#### Breath first search

- Like a `queue`
- Complexity theoretically O(N) but with JavaScript Arrays `[]` it’s O(N^2) because of shift/unshift
- Calling functions as/like a queue
- If children, add children to the queue
- If no children, de queue
- Will go level by level of the tree

```
      7
  23      8
5   4   21  15
Start at 7 => 7
Add children => 7->23->8
Dequeue => 23->8 => (7)
Visit 23 => 23->8 => (7)
Add children => 23->8->5->4 => (7)
Dequeue => 8->5->4 => (7,23)
Visit 8 => 8->5->4 => (7,23)
Add children => 8->5->4->21->15 => (7,23)
Visit & Dequeue => 5->4->21->15 => (7,23,8)
Visit & Dequeue => 4->21->15 => (7,23,8,5)
Visit & Dequeue => 21->15 => (7,23,8,5,4)
Visit & Dequeue => 15 => (7,23,8,5,4,21)
Visit & Dequeue => => (7,23,8,5,4,21,15)
```

##### BFS Loop

```TypeScript
const loop = (head: BinaryNode<number>): number[] => {
  const queue = [head]
  const path = []
  while (queue.length) {
    const curr = queue.shift()
    if (!curr?.value) continue
    path.push(curr.value)
    if (curr?.left) queue.push(curr.left)
    if (curr?.right) queue.push(curr.right)
  }
  return path
}

export default (head: BinaryNode<number>): number[] => loop(head)
```

##### BFS Recursive

```TypeScript
const recursive = (queue: BinaryNode<number>[], path: number[] = []): number[] => {
  const curr: BinaryNode<number> | undefined = queue.shift()
  // base-case
  if (!curr?.value) return path
  // pre
  path.push(curr.value)
  // recursion
  if (curr?.left) queue.push(curr.left)
  if (curr?.right) queue.push(curr.right)
  return recursive(queue, path)
}

export default (head: BinaryNode<number>): number[] => recursive([head])
```

##### BFS Level by Level

```JavaScript
/**
 *          20
 *     10          50
 *   5    15     30    100
 * => [[20],[10,50],[5,15,30,100]]
 */
const loopN = (head: BinaryNode<number>): number[][] => {
  const queue = [head]
  const levels: { [k:string]: number[] } = {}
  let level = 0
  while (queue.length) {
    let store = []
    for (let i = 0, il = queue.length; i < il; i++) {
      const curr = queue.shift()
      if (curr?.left) queue.push(curr.left)
      if (curr?.right) queue.push(curr.right)
      if (curr?.value) store.push(curr.value)
    }
    levels[level] = store
    level++
  }
  return Object.values(levels)
}
```

## Heap / Priority Queue

- It is a binary tree where every child and grand child is smaller (MaxHeap) or larger (MinHeap) than the current node.
- Whenever a node is added, we must adjust the tree
- Whenever a node is deleted, we must adjust the tree
- There is no traversing the tree
- Maintains a weak ordering
- The heap is always complete at each level (there is no gaps on one side)
- Self balancing
- Can be used for priority

```
                50 (0)
            /           \
     71 (1)               100 (2)
     /    \               /     \
  101 (3)  80 (4)     200 (5)    101 (6)
  /
200 (7)

- [50,71,100,101,80,200,101,200]
- Left hand child: 2*i+1
- Right hand child: 2*i+2
- Parent: Math.floor((i-1)/2)

      | (i-1)/2
      0
    /   \
2*i+1   2*i+2
```

// how to get the medium? (= using two heaps)

- MinHeap: means that the root node must be the smallest
- MaxHeap: Means that the root node must be the largest

```TypeScript
export default class MinHeap {
  public length: number
  private data: number[]

  constructor() {
    this.data = []
    this.length = 0
  }

  insert(value: number): void {
    this.data[this.length] = value
    this.heapifyUp(this.length)
    this.length++
  }
  // also some times called `poll` or `pop`
  delete(): number {
    if (this.length === 0) return -1
    const out = this.data[0]
    this.length--

    if (this.length === 0) {
      this.data = []
      return out
    }
    this.data[0] = this.data[this.length]
    this.heapifyDown(0)
    return out
  }

  private heapifyDown(idx: number): void {
    if (idx >= this.length) return
    const leftChild = this.leftChild(idx)
    const rightChild = this.rightChild(idx)
    if (leftChild >= this.length || rightChild >= this.length) return

    const lV = this.data[leftChild]
    const rV = this.data[rightChild]
    const value = this.data[idx]

    if (rV > lV && value > lV) {
      this.swap(idx, leftChild)
      this.heapifyDown(leftChild)
    } else if (lV > rV && value > rV) {
      this.swap(idx, rightChild)
      this.heapifyDown(rightChild)
    }
  }

  private heapifyUp(idx: number): void {
    if (idx === 0) return
    const parent = this.parent(idx)
    const parentValue = this.data[parent]
    const value = this.data[idx]

    if (parentValue > value) {
      this.swap(idx, parent)
      this.heapifyUp(parent)
    }
  }

  private swap(a: number, b: number) {
    const tmp = this.data[a]
    this.data[a] = this.data[b]
    this.data[b] = tmp
  }

  private parent(idx: number): number {
    return Math.floor((idx - 1) / 2)
  }
  private leftChild(idx: number): number {
    return 2 * idx + 1
  }
  private rightChild(idx: number): number {
    return 2 * idx + 2
  }
}
```

- Runtime O(logN) (add/delete)

## Trie / Prefix Tree / Digital Tree

- Autocomplete problems
- Cashing problems

```TypeScript
// To be done
```

## Graphs

- Anything with 3 nodes is a graph (it needs a cycle)
- A connected graph is where each node can reach each other node
- Nodes are called Point or `Vertex`
- Big O is commonly stated in terms of V: vertices and E: edges
- i.e. O(V*E) = on every vertex we check every edge
- Breath First Search and Depth First Searches can be used, one just needs to store the already seen nodes

### Adjacency List

A way to represent a graph is to write an `Adjacency List`:

```
0 - > 1
^ \   ^
|  _\||
3 < - 2
```

Can be represented as a list of edges (Adjacency List (what am I adjacent to? > Edge)):
```
[
  [{to: 1, weight: 10}, {to: 2, weight: 5}],
  [],
  [{to: 1, weight: …}, {to: 2, weight: …}],
  [{to: 0, weight: …}]
]
```

### Adjacency Matrix

```
0 - > 1
^ \   ^
|  _\||
3 < - 2
```

Can be represented in an `Adjacency Matrix` (memory intensive O(V^2), not used in maps):
```
[
    0, 1, 2, 3
0, [0,10, 5, 0]
1, [0, 0, 0, 0]
2, [0, …, 0, …]
3, […, 0, 0, …]
]
```

The numbers in the matrix represent `0` for no connection or the weight if there is a connection.

### Searching the Graph

- DFS and BFS both work, one just needs a seen array (and to share a path also a previous array starting at -1)
- Seen: [f,…]
- Prev: [-1,…]
- Queue: [0,…]
- The Complexity is `O(V+E)` (vertices + edges) since worst case each of them they will be checked once

#### Breath First Search Matrix
```TypeScript
/**
 * Example:
    >(1)<--->(4) ---->(5)
   /          |       /|
(0)     ------|------- |
   \   v      v        v
    >(2) --> (3) <----(6)
export const matrix2: WeightedAdjacencyMatrix = [
    [0, 3, 1,  0, 0, 0, 0], // 0
    [0, 0, 0,  0, 1, 0, 0],
    [0, 0, 7,  0, 0, 0, 0],
    [0, 0, 0,  0, 0, 0, 0],
    [0, 1, 0,  5, 0, 2, 0],
    [0, 0, 18, 0, 0, 0, 1],
    [0, 0, 0,  1, 0, 0, 1],
];
 */
export default function bfs(
  graph: WeightedAdjacencyMatrix,
  source: number,
  needle: number,
): number[] | null {
  const queue: number[] = [source]
  const seen: boolean[] = Array(graph.length).fill(false)
  const prev: number[] = Array(graph.length).fill(-1)
  seen[source] = true

  while (queue.length) {
    const curr = queue.shift() as number
    if (!curr && curr !== 0) continue
    if (curr === needle) break

    const adjs = graph[curr]
    for (let index = 0; index < adjs.length; index++) {
      const element = adjs[index]
      if (element === 0 || seen[index]) continue
      seen[index] = true
      prev[index] = curr
      queue.push(index)
    }
  }

  if (prev[needle] === -1) return null

  let curr = needle
  const out: number[] = []
  while (prev[curr] !== -1) {
    out.push(curr)
    curr = prev[curr]
  }

  out.push(source)
  return out.reverse()
}
```

#### Depth First Search List
```TypeScript
/**
 * Example:
     >(1)<--->(4) ---->(5)
    /          |       /|
 (0)     ------|------- |
    \   v      v        v
     >(2) --> (3) <----(6)
export const list2: WeightedAdjacencyList = [[
  [{to: 1, weight: 3}, {to: 2, weight: 1}],
  [{to: 4, weight: 1}],
  [{to: 2, weight: 7}],
  [],
  [{to: 1, weight: 1}, {to: 3, weight: 5}, {to: 5, weight: 2}],
  [{to: 2, weight: 18}, {to: 6, weight: 1}],
  [{to: 3, weight: 1}, {to: 6, weight: 1}],
]
 */

console.log(dfs(list2, 0, 6)) // 0, 1, 4, 5, 6

function recursion(
  graph: WeightedAdjacencyList,
  curr: number,
  needle: number,
  seen: boolean[],
  path: number[],
): boolean {
  if (seen[curr] || (!curr && curr !== 0)) return false
  seen[curr] = true

  path.push(curr)
  if (curr === needle) return true

  for (let index = 0; index < graph[curr].length; index++) {
    const element = graph[curr][index]
    if (recursion(graph, element.to, needle, seen, path)) return true
  }

  path.pop()

  return false
}

export default function dfs(
  graph: WeightedAdjacencyList,
  source: number,
  needle: number,
): number[] | null {
  if (needle === source) return null
  const path: number[] = []
  const seen = new Array(graph.length).fill(false)
  if (recursion(graph, source, needle, seen, path)) return path
  return null
}
```

### Dijkstra Shortest Path in Graph

```TypeScript
const hasUnvisited = (seen: boolean[], dists: number[]): boolean =>
  seen.some((bool, index) => !bool && dists[index] < Infinity)

const getLowestUnvisited = (seen: boolean[], dists: number[]): number => {
  let idx = -1
  let lowestDistance = Infinity

  for (let index = 0; index < seen.length; index++) {
    if (seen[index]) continue
    if (lowestDistance > dists[index]) {
      lowestDistance = dists[index]
      idx = index
    }
  }
  return idx
}

export default function dijkstra_list(
  source: number,
  sink: number,
  arr: WeightedAdjacencyList,
): number[] {
  const seen = new Array(arr.length).fill(false)
  const prev = new Array(arr.length).fill(-1)
  const dists = new Array(arr.length).fill(Infinity)
  dists[source] = 0

  // could be replaced by min-heap
  while (hasUnvisited(seen, dists)) {
    const curr = getLowestUnvisited(seen, dists)
    seen[curr] = true

    const adjs = arr[curr]
    for (let index = 0; index < adjs.length; index++) {
      const edge = adjs[index]
      if (seen[edge.to]) continue
      const dist = dists[curr] + edge.weight
      if (dist < dists[edge.to]) {
        dists[edge.to] = dist
        prev[edge.to] = curr
      }
    }
  }

  const out: number[] = []
  let curr = sink
  while (prev[curr] !== -1) {
    out.push(curr)
    curr = prev[curr]
  }

  out.push(source)
  return out.reverse()
}
```
