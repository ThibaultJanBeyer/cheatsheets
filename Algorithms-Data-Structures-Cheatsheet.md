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
