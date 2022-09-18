[back to overwiev](/../..)

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
- [Linked Lists](#linked-lists)
- - [Queue](#queue)
- - [Stack](#stack)

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
