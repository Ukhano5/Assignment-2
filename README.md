# Assignment-2
ABBOTTABAD UNIVERSITY OF SCIENCE AND TECHNOLOGY 
Department of Computer Science 






 Class : BSSE-3 C

Subject:  DSA

Submitted By: Muhammad Umair 

Date: 13/11/2023 
Instructor: Jamal Abdul Ahad 



Question No 1 :

Design a python program that simulate a Web server handling incoming request using a queue. Model different types of request with varying processing time and simulate their processing order

import queue
import time
import random
import threading

# Function to simulate processing of a request
def process_request(request_type):
    processing_time = random.randint(1, 5)  # Simulate varying processing time
    time.sleep(processing_time)
    print(f"Processed {request_type} request in {processing_time} seconds")

# Function to handle incoming requests
def handle_requests(request_queue):
    while True:
        if not request_queue.empty():
            request_type = request_queue.get()
            process_request(request_type)

# Function to simulate incoming requests
def simulate_requests(request_queue):
    request_types = ["GET", "POST", "PUT", "DELETE"]

    while True:
        request_type = random.choice(request_types)
        request_queue.put(request_type)
        print(f"Incoming {request_type} request")
        time.sleep(random.uniform(0.5, 2))

# Create a queue to handle incoming requests
request_queue = queue.Queue()

# Create threads for handling and simulating requests
handling_thread = threading.Thread(target=handle_requests, args=(request_queue,), daemon=True)
simulation_thread = threading.Thread(target=simulate_requests, args=(request_queue,), daemon=True)

# Start the threads
handling_thread.start()
simulation_thread.start()

# Main thread waits for threads to finish
handling_thread.join()
simulation_thread.join()
OUT PUT 
Incoming GET request
Incoming POST request
Processed GET request in 3 seconds
Incoming PUT request
Processed POST request in 1 seconds
Processed PUT request in 5 seconds
Incoming DELETE request
Processed DELETE request in 2 seconds
Incoming GET request
Processed GET request in 2 seconds

Question No 2:
In what scenarios would you chose a linked list implementation over an array implementation for a  queue and vice versa

Answer :
Choosing between a linked list and an array implementation for a queue depends on the specific requirements and characteristics of the application. Here are some scenarios for each:

**Linked List Implementation for a Queue:**

1. **Dynamic Size:** If the size of the queue is not fixed and may change during runtime, a linked list is a good choice. Linked lists allow dynamic allocation and deallocation of memory without the need to predefine the size.

2. **Frequent Insertions and Deletions:** Linked lists are efficient for frequent insertions and deletions, especially in the middle, as they involve simple pointer adjustments. This can be beneficial if your queue involves frequent enqueue and dequeue operations.

3. **No Wasted Memory:** Linked lists can be more memory-efficient when the exact size of the queue is unknown, as they allocate memory dynamically, avoiding the need for a predefined size that may result in unused memory.

**Array Implementation for a Queue:**

1. **Fixed Size:** If the maximum size of the queue is known and remains constant, using an array can be a simpler and more memory-efficient option. Arrays allocate memory at the beginning and do not need additional memory for pointers.

2. **Random Access:** If there is a need for random access to elements in the queue (accessing elements by index), an array is a better choice. Arrays provide constant-time access to any element, while linked lists require traversing from the beginning.

3. **Cache Locality:** Arrays benefit from better cache locality compared to linked lists. Accessing elements in an array is more cache-friendly, which can lead to better performance in scenarios where cache efficiency is crucial. 

Question No 3 :
Discuss the time complexity of enqueue and dequeue operation in a basic queue how can you optimize  these operation for specific use case

Answer.. 
In a basic queue, the time complexity of enqueue and dequeue operations depends on the underlying implementation. Let's discuss the time complexity for a simple array-based queue and then explore optimization strategies.

**Basic Queue with an Array:**

1. **Enqueue Operation:**
   - Time Complexity: O(1)
   - Explanation: Adding an element to the end of an array takes constant time, as you just need to increment the rear index.

2. **Dequeue Operation:**
   - Time Complexity: O(1)
   - Explanation: Removing an element from the front of an array is also a constant-time operation. After dequeuing, you can simply increment the front index.

**Optimizations:**

1. **Circular Queue:**
   - To avoid wasting space in the array when elements are dequeued, you can use a circular queue. When the rear or front index reaches the end of the array, it wraps around to the beginning.

2. **Dynamic Resizing:**
   - If the size of the queue is not fixed and can grow or shrink, consider dynamic resizing. When the queue becomes full, allocate a larger array and copy elements. This helps avoid running out of space and can improve efficiency.

3. **Double-Ended Queue (Deque):**
   - If you need to enqueue and dequeue from both ends of the queue, consider using a double-ended queue (deque). This can be implemented with a doubly linked list or a dynamic array.

4. **Amortized Analysis:**
   - In scenarios where resizing or reallocation is involved, consider using amortized analysis to analyze the overall performance over a sequence of operations. Although resizing operations may be O(n), when spread across multiple enqueue and dequeue operations, the average cost per operation can be lower.

5. **Parallelization:**
   - For concurrent environments, you can explore parallelizing enqueue and dequeue operations. However, this requires careful synchronization to avoid race conditions.



Question No 4 : 

How can you use two stacks to implement a queue? Provide a step by step explanation of the enqueue and dequeue operation in this scenario

Answer.. 
Using two stacks to implement a queue is a common technique that allows you to simulate a queue using two stacks. One stack is used for enqueue operations, and the other is used for dequeue operations. Here's a step-by-step explanation for both enqueue and dequeue operations:

**Enqueue Operation:**

1. **Push onto Stack 1 (s1):**
   - When an element needs to be enqueued, push it onto Stack 1.

   ```
   Enqueue(1)
   |  1  |    |
   |____|____|
   |____|____|
   ```

2. **Enqueue(2):**
   - Push the second element onto Stack 1.

   ```
   Enqueue(1)   Enqueue(2)
   |  1  |      |  2  |
   |____|____|  |____|
   |____|____|  |____|
   ```

**Dequeue Operation:**

1. **Pop from Stack 2 (s2):**
   - If Stack 2 is empty, pop all elements from Stack 1 and push them onto Stack 2.

   ```
   Enqueue(1)   Enqueue(2)   Dequeue
   |  1  |      |  2  |      |  1  |
   |____|____|  |____|      |____|
   |____|____|  |____|      |____|
   ```

2. **Dequeue:**
   - Pop the element from Stack 2. This element is the front of the queue.

   ```
   Enqueue(1)   Enqueue(2)   Dequeue
   |  1  |      |  2  |      |      |
   |____|____|  |____|      |____|
   |____|____|  |____|      |____|
   ```

3. **Dequeue(2):**
   - If another dequeue operation is performed, pop the next element from Stack 2.

   ```
   Enqueue(1)   Enqueue(2)   Dequeue   Dequeue
   |  1  |      |  2  |      |      |      |
   |____|____|  |____|      |____|      |
   |____|____|  |____|      |____|      |
   ```

 This approach ensures that the oldest elements are always at the front of Stack 2, providing a first-in, first-out order. The amortized time complexity for both enqueue and dequeue operations is O(1), although a sequence of dequeue operations may require transferring elements from one stack to the other.
