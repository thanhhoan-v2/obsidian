## Stack

### What is Stack Memory?

- **Thread-specific**: Each thread has its own stack
- **LIFO (Last In, First Out)**: Method calls are stacked on top of each other
- **Automatic memory management**: Memory is automatically allocated and deallocated
- **Fast access**: Direct memory access, very efficient

### What's Stored in Stack?

1. **Local variables** (primitives and object references)
2. **Method parameters**
3. **Method call information** (return addresses, partial results)
4. **Reference variables** (pointers to objects in heap

```java
public class StackExample {
    public static void main(String[] args) {
        int a = 10;          // Stored in stack
        String name = "John"; // Reference stored in stack, object in heap

        methodA(a, name);
    }

    static void methodA(int x, String str) {
        int b = 20;          // Stored in stack
        methodB(b);
    }

    static void methodB(int y) {
        int c = 30;          // Stored in stack
        // When methodB ends, 'c' is removed from stack
    }
}
```

## Heap

### What is Heap Memory?

- **Shared**: All threads share the same heap
- **Dynamic allocation**: Objects are created at runtime
- **Garbage collected**: Automatic memory cleanup
- **Larger capacity**: Much larger than stack

### What's Stored in Heap?

1. **Objects** (class instances)
2. **Instance variables** (object fields)
3. **Arrays**
4. **Static variables** (in method area, part of heap)

```java
public class HeapExample {
    static int staticVar = 100;    // Stored in heap (method area)
    int instanceVar = 200;         // Stored in heap (with object)

    public static void main(String[] args) {
        HeapExample obj1 = new HeapExample();  // Object in heap
        HeapExample obj2 = new HeapExample();  // Another object in heap

        int[] array = new int[1000];           // Array in heap

        String str = new String("Hello");      // String object in heap
    }
}
```

## Quick Reference

### Stack Memory:

- **Contains**: Local variables, method parameters, return addresses
- **Characteristics**: Thread-specific, LIFO, automatic cleanup
- **Size**: Small, limited
- **Access**: Very fast

### Heap Memory:

- **Contains**: Objects, instance variables, arrays
- **Characteristics**: Shared, garbage collected, dynamic
- **Size**: Large, configurable
- **Access**: Slower than stack

### Remember:

- **References** are stored in stack
- **Objects** are stored in heap
- **Primitive variables** are stored in stack

---

# Quiz

### 1. Where are local variables stored in Java?

A) Heap memory  
B) Stack memory  
C) Method area  
D) Program counter

**Answer: B) Stack memory** _Explanation: Local variables are stored in the stack memory of the thread executing the method._

### 2. What happens to stack memory when a method finishes execution?

A) It gets moved to heap memory  
B) It's automatically deallocated  
C) It remains until garbage collection  
D) It's stored in permanent generation

**Answer: B) It's automatically deallocated** _Explanation: Stack memory is automatically managed. When a method completes, its stack frame is popped and memory is freed._

### 3. Which of the following is stored in heap memory?

A) Local variables  
B) Method parameters  
C) Object instances  
D) Return addresses

**Answer: C) Object instances** _Explanation: All objects (instances of classes) are stored in heap memory, regardless of where they're created._

### 4. What is the primary characteristic of stack memory allocation?

A) FIFO (First In, First Out)  
B) LIFO (Last In, First Out)  
C) Random access  
D) Circular queue

**Answer: B) LIFO (Last In, First Out)** _Explanation: Stack follows LIFO principle - the last method called is the first to complete and be removed._

### 5. Consider this code:

```java
public void method() {
    int x = 10;
    String str = new String("Hello");
}
```

Where are `x` and `str` stored respectively? A) Both in heap memory  
B) Both in stack memory  
C) `x` in stack, `str` in heap  
D) `x` in stack, `str` reference in stack and object in heap

**Answer: D) `x` in stack, `str` reference in stack and object in heap** _Explanation: Primitive `x` is stored in stack. `str` is a reference variable (stored in stack) pointing to a String object (stored in heap)._

### 6. Which memory area is shared among all threads?

A) Stack memory  
B) Heap memory  
C) Program counter  
D) Native method stack

**Answer: B) Heap memory** _Explanation: Heap memory is shared among all threads in a Java application, while each thread has its own stack._

### 7. What causes a `StackOverflowError`?

A) Creating too many objects  
B) Infinite recursion  
C) Memory leak in heap  
D) Garbage collection failure

**Answer: B) Infinite recursion** _Explanation: StackOverflowError occurs when the stack memory is exhausted, typically due to infinite or very deep recursion._

### 8. In the heap memory, where are newly created objects initially placed?

A) Old generation  
B) Eden space (Young generation)  
C) Survivor space  
D) Permanent generation

**Answer: B) Eden space (Young generation)** _Explanation: New objects are initially created in the Eden space of the Young generation in heap memory._

### 9. Which garbage collection occurs in the Young generation?

A) Major GC  
B) Full GC  
C) Minor GC  
D) Parallel GC

**Answer: C) Minor GC** _Explanation: Minor GC occurs in the Young generation and is fast and frequent._

### 10. What is the typical size limit for stack memory per thread?

A) 512 KB  
B) 1 MB  
C) 8 MB  
D) 64 MB

**Answer: B) 1 MB** _Explanation: The typical default stack size per thread is around 1 MB, though this can vary by JVM and can be configured._

### 11. Consider this code:

```java
public class Test {
    static int staticVar = 100;
    int instanceVar = 200;
    
    public void method() {
        int localVar = 300;
    }
}
```

Where is `staticVar` stored? A) Stack memory  
B) Heap memory (Method area)  
C) Young generation  
D) Old generation

**Answer: B) Heap memory (Method area)** _Explanation: Static variables are stored in the Method area, which is part of heap memory._

### 12. Which statement about heap memory is FALSE?

A) It's used for dynamic memory allocation  
B) It's managed by garbage collection  
C) It's thread-specific  
D) It's larger than stack memory

**Answer: C) It's thread-specific** _Explanation: Heap memory is shared among all threads, not thread-specific. Stack memory is thread-specific._

### 13. What happens when heap memory is full?

A) StackOverflowError  
B) OutOfMemoryError  
C) NullPointerException  
D) IllegalStateException

**Answer: B) OutOfMemoryError** _Explanation: When heap memory is exhausted, the JVM throws an OutOfMemoryError._

### 14. In which memory area are method calls tracked?

A) Heap memory  
B) Stack memory  
C) Method area  
D) PC register

**Answer: B) Stack memory** _Explanation: Method calls are tracked in the call stack, which is part of stack memory._

### 15. Which of the following makes an object eligible for garbage collection?

A) Setting all references to null  
B) The object goes out of scope  
C) No active references point to it  
D) All of the above

**Answer: D) All of the above** _Explanation: An object becomes eligible for GC when there are no active references to it, which can happen through any of these scenarios._

### 16. What is stored in the Survivor spaces of heap memory?

A) Newly created objects  
B) Objects that survived one GC cycle  
C) Long-lived objects  
D) Static variables

**Answer: B) Objects that survived one GC cycle** _Explanation: Survivor spaces (S0 and S1) contain objects that survived at least one garbage collection cycle._

### 17. Consider this scenario:

```java
public void createObjects() {
    for(int i = 0; i < 1000; i++) {
        String s = new String("Test");
    }
}
```

What happens to the String objects after the loop? A) They remain in memory permanently  
B) They're moved to old generation  
C) They become eligible for garbage collection  
D) They're stored in string pool

**Answer: C) They become eligible for garbage collection** _Explanation: When the loop ends, the local reference `s` goes out of scope, making all created String objects eligible for GC._

### 18. Which memory access is faster?

A) Stack memory  
B) Heap memory  
C) Both are equally fast  
D) Depends on the JVM implementation

**Answer: A) Stack memory** _Explanation: Stack memory access is much faster than heap memory access due to its simple LIFO structure._

### 19. What is the relationship between stack and heap when creating objects?

A) Objects are created in stack, references in heap  
B) Objects are created in heap, references in stack  
C) Both objects and references are in heap  
D) Both objects and references are in stack

**Answer: B) Objects are created in heap, references in stack** _Explanation: When you create an object, the object itself goes to heap memory, but the reference variable is stored in stack memory._

### 20. In a multi-threaded application, which statement is TRUE?

A) All threads share the same stack  
B) Each thread has its own stack but shares heap  
C) Each thread has its own heap but shares stack  
D) Each thread has its own stack and heap

**Answer: B) Each thread has its own stack but shares heap** _Explanation: In multi-threading, each thread has its own stack memory, but all threads share the same heap memory._

## Key Takeaways

- **Stack:** Thread-specific, stores local variables and method calls, LIFO, automatic cleanup
- **Heap:** Shared among threads, stores objects, garbage collected, larger capacity
- **References:** Stored in stack, point to objects in heap
- **GC:** Automatic memory management for heap, not needed for stack