# Java Object Class

[cite_start]The Object class is the parent class for all classes in Java[cite: 3]. [cite_start]It is often referred to as the "mother of all classes," and even user-defined classes inherit from it[cite: 4, 5].

## Key Methods
The Object class provides several important methods:

* **`clone()`**: Creates a clone of the object and returns the same object.
* **`equals(Object obj)`**: Compares two objects and returns `true` if both references hold the same object.
* **`finalize()`**: Called by the garbage collector when an object is being collected[cite: 8]. [cite_start]It is similar to the destructor method in C++.
* **`getClass()`**: Returns the runtime class of the object.
* **`hashCode()`**: Returns the hash code value of the object[cite: 11]. [cite_start]No two programs created by Java have the same hash codes.
* **`notify()`**: Wakes up a single thread].
***`notifyAll()`**: Wakes up all threads.
* **`toString()`**: Returns the string representation of the object. When an object is printed, this method is called automatically; it should be overridden to print specific object details.
* **`wait()`**: Causes the current thread to wait until another thread invokes `notify`.
    * **`wait(long timeout)`**: Causes the current thread to wait until another thread invokes `notify`.
    * **`wait(long timeout, int nanos)`**: Causes the current thread to wait until the thread invokes `notify` or a certain amount of real-time has elapsed.
