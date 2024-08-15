## Multi-Threading Java Programs with Detailed Explanation Solutions 💻

#### Day 1 - 15-08-2024

#### **1. Print Numbers Using Multiple Threads**

**Program Description**:
This program demonstrates the creation and execution of two separate threads: one that prints even numbers and another that prints odd numbers. The program highlights basic multi-threading concepts, including thread creation, execution, and the usage of `Thread.sleep()` to simulate a delay.

```java
class EvenThread extends Thread {
    public void run() {
        // Loop to print even numbers from 2 to 10
        for (int i = 2; i <= 10; i += 2) {
            System.out.println("Even: " + i);
            try {
                Thread.sleep(100);  // Pauses the thread for 100ms to simulate processing delay
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class OddThread extends Thread {
    public void run() {
        // Loop to print odd numbers from 1 to 9
        for (int i = 1; i <= 9; i += 2) {
            System.out.println("Odd: " + i);
            try {
                Thread.sleep(100);  // Pauses the thread for 100ms to simulate processing delay
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

public class EvenOddThreads {
    public static void main(String[] args) {
        EvenThread evenThread = new EvenThread();  // Create the even number thread
        OddThread oddThread = new OddThread();  // Create the odd number thread

        evenThread.start();  // Start the even number thread
        oddThread.start();  // Start the odd number thread
    }
}
```

**Explanation**:

- **Thread.sleep(100)**: Introduces a delay to better visualize the concurrent execution of threads. Without this, the output might appear jumbled as both threads could execute nearly simultaneously.
- **Thread.start()**: Initiates the thread's execution by calling its `run()` method. It is important to note that directly calling `run()` will not create a new thread; it will execute sequentially like a normal method.

#### **2. Simulate a Simple Counter Using Threads**

**Program Description**:
This program introduces thread synchronization to prevent race conditions. It creates a simple counter that multiple threads increment concurrently. The `synchronized` keyword ensures that only one thread can access the increment method at a time, thus preventing inconsistencies in the counter's value.

```java
class Counter {
    private int count = 0;

    // Synchronized method to ensure thread-safe increment
    public synchronized void increment() {
        count++;  // Increment the count
    }

    public int getCount() {
        return count;  // Return the current count value
    }
}

class CounterThread extends Thread {
    private Counter counter;

    public CounterThread(Counter counter) {
        this.counter = counter;  // Assign the shared counter object
    }

    public void run() {
        for (int i = 0; i < 1000; i++) {
            counter.increment();  // Safely increment the counter
        }
    }
}

public class SynchronizedCounter {
    public static void main(String[] args) {
        Counter counter = new Counter();  // Shared counter object

        CounterThread t1 = new CounterThread(counter);  // First thread
        CounterThread t2 = new CounterThread(counter);  // Second thread

        t1.start();  // Start first thread
        t2.start();  // Start second thread

        try {
            t1.join();  // Wait for t1 to finish
            t2.join();  // Wait for t2 to finish
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // After both threads have finished, print the final counter value
        System.out.println("Final count: " + counter.getCount());
    }
}
```

**Explanation**:

- **Synchronized Method**: The `synchronized` keyword ensures that only one thread can execute the `increment()` method at a time, preventing multiple threads from corrupting the shared `count` variable.
- **Thread.join()**: Ensures that the main thread waits for both threads to finish before printing the final counter value. This is crucial for accurate results.
- **Race Condition**: Without synchronization, both threads might read the same value of `count` and increment it, resulting in a lost update. Synchronization prevents this by locking the method for one thread at a time.

#### **3. Producer-Consumer Problem Using `wait()` and `notify()`**

**Program Description**:
This program implements the classic Producer-Consumer problem, where one thread produces data (Producer) and another thread consumes it (Consumer). The program uses `wait()` and `notify()` for proper synchronization between the threads, ensuring that the Producer doesn't overflow the buffer and the Consumer doesn't consume from an empty buffer.

```java
import java.util.LinkedList;
import java.util.Queue;

class ProducerConsumer {
    private Queue<Integer> queue = new LinkedList<>();
    private final int CAPACITY = 5;  // Max size of the queue

    public void produce() throws InterruptedException {
        int value = 0;
        while (true) {
            synchronized (this) {
                while (queue.size() == CAPACITY) {
                    wait();  // Wait if the queue is full
                }
                queue.add(value);  // Add produced value to the queue
                System.out.println("Produced: " + value);
                value++;
                notify();  // Notify the consumer that data is available
                Thread.sleep(500);  // Simulate time taken to produce
            }
        }
    }

    public void consume() throws InterruptedException {
        while (true) {
            synchronized (this) {
                while (queue.isEmpty()) {
                    wait();  // Wait if the queue is empty
                }
                int value = queue.poll();  // Consume the first element in the queue
                System.out.println("Consumed: " + value);
                notify();  // Notify the producer that space is available
                Thread.sleep(500);  // Simulate time taken to consume
            }
        }
    }
}

public class ProducerConsumerDemo {
    public static void main(String[] args) {
        ProducerConsumer pc = new ProducerConsumer();

        Thread producerThread = new Thread(new Runnable() {
            public void run() {
                try {
                    pc.produce();  // Start producing items
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        Thread consumerThread = new Thread(new Runnable() {
            public void run() {
                try {
                    pc.consume();  // Start consuming items
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        producerThread.start();  // Start the producer thread
        consumerThread.start();  // Start the consumer thread
    }
}
```

**Explanation**:

- **wait()**: Causes the current thread to wait until another thread invokes `notify()` or `notifyAll()` on the same object. It releases the lock held by the thread until it regains control.
- **notify()**: Wakes up a single thread that is waiting on the object's monitor. If multiple threads are waiting, one of them is chosen to be awakened.
- **Synchronization**: Both `produce()` and `consume()` methods are synchronized to ensure that only one thread can modify the shared `queue` at a time, preventing data corruption.
- **Queue Size**: The `CAPACITY` variable limits the size of the queue, ensuring that the producer cannot overflow the queue and the consumer cannot consume from an empty queue.

#### **🔧 Commands for Running the Programs**

- **Compile** the Java files using: `javac FileName.java`
- **Run** the compiled program using: `java FileName`

---
