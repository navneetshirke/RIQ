````md
# 00017_mutex_race_conditions_ans.md

# Mutex & Race Conditions

## Answer

In Ruby, **mutexes** and **race conditions** are fundamental concepts in **thread-safe programming**. Understanding how to use mutexes properly is essential when dealing with **shared resources in multithreaded environments** to prevent inconsistent data and unpredictable behavior.

---

### 1. What is a Race Condition?

- A **race condition** occurs when **two or more threads access shared data concurrently**, and the final outcome depends on the **timing of thread execution**.

Example:

```ruby
counter = 0

threads = 10.times.map do
  Thread.new do
    1000.times { counter += 1 }
  end
end

threads.each(&:join)
puts counter  # May not be 10000 due to race conditions
````

* Here, multiple threads increment `counter` simultaneously, causing **lost updates** because the increment operation is **not atomic**.

---

### 2. What is a Mutex?

* A **Mutex (Mutual Exclusion)** is an object that **locks a critical section**, ensuring that only **one thread at a time** executes that block of code.

```ruby
mutex = Mutex.new
counter = 0

threads = 10.times.map do
  Thread.new do
    1000.times do
      mutex.synchronize { counter += 1 }
    end
  end
end

threads.each(&:join)
puts counter  # Always 10000
```

* `synchronize` ensures that the enclosed block is executed **exclusively** by one thread at a time.

---

### 3. How Mutex Works

1. A thread **acquires the lock** before executing the synchronized block.
2. Other threads trying to enter the block **wait** until the lock is released.
3. After the block finishes, the lock is **released** automatically.

---

### 4. Deadlocks

* Occurs when **two or more threads wait indefinitely** for each other’s locks.

Example:

```ruby
mutex1 = Mutex.new
mutex2 = Mutex.new

t1 = Thread.new do
  mutex1.synchronize { sleep 1; mutex2.synchronize { puts "Done" } }
end

t2 = Thread.new do
  mutex2.synchronize { sleep 1; mutex1.synchronize { puts "Done" } }
end

t1.join
t2.join  # Deadlock occurs
```

* Avoid deadlocks by:

  * Acquiring locks in **consistent order**
  * Using `try_lock` with **timeouts**
  * Keeping critical sections **small**

---

### 5. Thread Safety in Ruby

* Ruby MRI has a **Global Interpreter Lock (GIL)**, preventing true parallel execution of Ruby code.
* Even with GIL, **race conditions still occur** for shared resources because threads can be **interrupted between instructions**.
* Mutexes are required when **writing to shared variables** or performing **compound operations**.

---

### 6. Other Synchronization Primitives

1. **Queue**

   * Thread-safe FIFO queue for communication between threads.

```ruby
require 'thread'
queue = Queue.new
queue.push(1)
queue.pop  # => 1
```

2. **ConditionVariable**

   * For advanced signaling between threads.

3. **Ractor**

   * Uses **isolated memory**, eliminating most race conditions.

---

### 7. Practical Examples

**Thread-safe counter:**

```ruby
class SafeCounter
  def initialize
    @counter = 0
    @mutex = Mutex.new
  end

  def increment
    @mutex.synchronize { @counter += 1 }
  end

  def value
    @mutex.synchronize { @counter }
  end
end
```

**Shared resource in Rails background jobs:**

* When multiple threads process tasks concurrently, use mutexes for:

  * File writes
  * Shared caches
  * Global variables (avoid if possible)

---

### 8. Best Practices

* Keep **critical sections as small as possible**.
* Avoid **locking global objects** unnecessarily.
* Prefer **immutable data** or **Ractors** when possible.
* Use **thread-safe collections** (Queue, Concurrent::Hash) for complex shared data.
* Always consider potential **deadlocks** when multiple locks are needed.

---

### 9. Summary

* **Race condition**: Multiple threads accessing shared data simultaneously, causing unpredictable results.
* **Mutex**: Locks critical sections to ensure **atomicity** and **thread safety**.
* Thread safety is critical in Ruby **multithreaded applications**, background jobs, and web servers.
* Combine mutexes with **small critical sections, careful lock ordering, and thread-safe data structures** for robust concurrent programming.

---

**End of Mutex & Race Conditions**


