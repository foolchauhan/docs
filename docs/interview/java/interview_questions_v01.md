---
title: Interview Questions for Java - V01
layout: default
parent: Java
grand_parent: Interview
---
# How to execute Threads sequentialy in java?

To execute threads sequentially in Java, ensuring one thread completes before the next starts, you can use the following approaches:

#### 1. Using join() Method
The `join()` method makes the current thread wait until the specified thread has finished executing.
```java
class SequentialThread extends Thread {
    public SequentialThread(String name) {
        super(name);
    }

    public void run() {
        System.out.println(Thread.currentThread().getName() + " is running");
    }
}

public class SequentialExecution {
    public static void main(String[] args) {
        Thread t1 = new SequentialThread("Thread-1");
        Thread t2 = new SequentialThread("Thread-2");
        Thread t3 = new SequentialThread("Thread-3");

        try {
            t1.start();
            t1.join();  // Waits for t1 to finish

            t2.start();
            t2.join();  // Waits for t2 to finish

            t3.start();
            t3.join();  // Waits for t3 to finish
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

#### 2. Using ExecutorService (Single Thread Executor)

A `SingleThreadExecutor` ensures tasks are executed in a strict sequential order.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class SequentialExecutor {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newSingleThreadExecutor();

        executor.submit(() -> System.out.println("Task 1 is running"));
        executor.submit(() -> System.out.println("Task 2 is running"));
        executor.submit(() -> System.out.println("Task 3 is running"));

        executor.shutdown(); // Shutdown after all tasks complete
    }
}
```

#### 3. Using synchronized and wait-notify (More Control)
This method uses a shared object to synchronize execution order.

```java
class OrderedThread implements Runnable {
    private final Object prevLock;
    private final Object currLock;

    public OrderedThread(Object prevLock, Object currLock) {
        this.prevLock = prevLock;
        this.currLock = currLock;
    }

    public void run() {
        synchronized (prevLock) {
            synchronized (currLock) {
                System.out.println(Thread.currentThread().getName() + " is running");
                currLock.notify();
            }
        }
    }
}

public class WaitNotifyExample {
    public static void main(String[] args) throws InterruptedException {
        Object lock1 = new Object();
        Object lock2 = new Object();
        Object lock3 = new Object();

        Thread t1 = new Thread(new OrderedThread(lock1, lock2), "Thread-1");
        Thread t2 = new Thread(new OrderedThread(lock2, lock3), "Thread-2");
        Thread t3 = new Thread(new OrderedThread(lock3, new Object()), "Thread-3");

        t1.start();
        Thread.sleep(100); // Ensure correct ordering
        t2.start();
        Thread.sleep(100);
        t3.start();
    }
}
```

### Which Approach to Use?

|Approach |Use Case|
|-----|------|
|join()	|Simple, ensures sequential execution of threads.|
|ExecutorService|	Best when managing multiple tasks in sequential order.|
|synchronized + wait-notify	|Fine-grained control over thread execution order.|

