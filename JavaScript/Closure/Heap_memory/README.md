# Heap memory and closure

When a closure is created in JavaScript, it retains a reference to any variables or parameters that are in scope at the time the closure is created. The variable itself is stored on the heap, but the closure object only contains a reference to that variable.

The `count` variable is defined within the `makeCounter()` function, and its value is stored on the heap. When the closure is created, it captures a reference to the `count` variable and stores that reference in the closure object. The closure object itself is also stored on the heap.

Here's a simple visualization of how this works:

```javascript
function makeCounter() {
  var count = 0;
  
  function counter() {
    count++;
    console.log(count);
  }
  
  return counter;
}

var c = makeCounter();
```

When the `makeCounter()` function is called, a new execution context is created on the stack, and the `count` variable is initialized to 0 in this execution context.

The `counter()` function is defined within the `makeCounter()` function, and a closure is returned that contains a reference to the `counter()` function and the `count` variable.

The closure is assigned to the c variable in the global scope. The `makeCounter()` function completes execution, and its execution context is popped off the stack. The `count` variable and the closure object are now stored on the heap.

When the closure is called, it increments the `count` variable, which is stored on the heap. The closure accesses the `count` variable through its reference in the closure object.


Here's a simplified visualization of the closure object itself:

```javascript
{
  counter: function () { /* copy of counter function */ },
  count: <reference to count variable on the heap>
}
```

The `count` variable is stored as a reference to the actual variable stored on the heap. On the other hand, the `counter` function is not a reference to the original function, but rather a copy of the function that is created when the closure is created.

## Why is it called `heap memory`?  Is it related to the heap data structure?

The heap data structure is used in memory management to keep track of free memory blocks that are available for dynamic allocation. The heap data structure is not used to prioritize memory blocks or manage memory access in any way; it's simply used as a data structure to organize and track available memory blocks.

When memory is allocated on the heap, the operating system finds a free memory block that is large enough to satisfy the allocation request. The free memory blocks are organized and tracked using a data structure that's similar to a heap data structure. This data structure allows the operating system to efficiently find and allocate free memory blocks as needed.

The heap data structure is used because it provides a fast and efficient way to insert and remove memory blocks as they're allocated and deallocated. When a memory block is allocated, it's removed from the heap data structure, and when it's deallocated, it's added back to the heap data structure. The heap data structure is used to keep track of which memory blocks are free and which are in use, and to quickly find free memory blocks that are large enough to satisfy allocation requests.

So in summary, the heap data structure is used in memory management to organize and track free memory blocks that are available for dynamic allocation. The heap data structure allows the operating system to efficiently find and allocate free memory blocks, and to keep track of which memory blocks are in use and which are free. It's not used to prioritize memory blocks or manage memory access in any way.
