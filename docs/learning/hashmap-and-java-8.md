---
title: HashMap in Java & Java 8 Enhancements
layout: default
nav_order: 2
parent: Learning
---
## **Internal Working of HashMap in Java & Java 8 Enhancements**

### **Introduction**
A `HashMap` in Java is a widely used data structure that allows efficient storage and retrieval of key-value pairs. It uses **hashing** to achieve an average time complexity of **O(1)** for basic operations like insertion, deletion, and lookup.

With Java 8, significant improvements were made to optimize performance, especially in handling collisions.

---
## **1. How HashMap Works Internally?**

A **HashMap** consists of an array of **Node<K, V>**, where each node represents a key-value pair. Let's break it down:

### **Key Components**

- **Array (Bucket Array):** The underlying structure is an array of `Node<K, V>`, which stores key-value pairs.
- **Node<K, V>:** A static nested class representing each entry:
  
  ```java
  static class Node<K, V> {
      final int hash;
      final K key;
      V value;
      Node<K,V> next;
  }
  ```

- **Hash Function:** Determines the bucket (array index) where an entry is stored.
  
  ```java
  index = hash(key) & (n - 1); // n = table.length
  ```
  
- **Collision Handling:** When multiple keys map to the same bucket, Java handles collisions using chaining.
  
  **Before Java 8:** Used a **Linked List**.
  
  **Java 8 Enhancement:** Uses a **Balanced Tree (Red-Black Tree)** for better performance.

---
## **2. Java 8 Enhancements in HashMap**

Java 8 introduced significant improvements to **HashMap**, particularly in handling hash collisions and improving performance.

### **1. Tree-Based Bucket (Red-Black Tree)**
- When the number of elements in a bucket exceeds **8** (a threshold), Java 8 replaces the linked list with a **Red-Black Tree**.
- This improves worst-case performance from **O(n) → O(log n)**.

### **2. Better Hash Distribution**
- Java 8 improved the **hash function** to reduce collisions.
- It avoids poor `hashCode()` implementations by applying bitwise operations for uniform distribution.

### **3. Rehashing Enhancements**
- When **load factor (default 0.75)** exceeds the threshold, the `resize()` method doubles the array size and redistributes elements.
- Instead of recomputing hashes, Java 8 optimizes rehashing using **(oldIndex & oldCapacity) == 0** to determine the new index efficiently.

---
## **3. Performance Analysis**

| Scenario | Before Java 8 | After Java 8 |
|----------|--------------|-------------|
| Best Case | **O(1)** | **O(1)** |
| Worst Case (High Collisions) | **O(n)** (Linked List) | **O(log n)** (Red-Black Tree) |

---
## **Conclusion**
Java 8 significantly optimized `HashMap` by introducing **Red-Black Trees** for better collision handling, improving **hash distribution**, and enhancing **rehashing performance**. These changes ensure better efficiency, especially in high-collision scenarios.

Would you like to see a hands-on code example demonstrating these changes? Let me know in the comments! 🚀

