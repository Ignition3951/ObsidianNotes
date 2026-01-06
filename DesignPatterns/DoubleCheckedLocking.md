# Double-Checked Locking in Java

## What is Double-Checked Locking?
Double-checked locking (DCL) is a design pattern used to reduce the overhead of synchronization when creating singleton objects in multi-threaded environments.

## Why it is Needed
- Synchronizing the entire method is thread-safe but slow.
- We want synchronization only during the first object creation.

## How It Works
1. Check if the instance is null (without locking).
2. Synchronize the block.
3. Check again if the instance is null.
4. Create the instance.

## Correct Java Implementation (Java 5+)

```java
class Singleton {

    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

## Why `volatile` is Important
- Prevents instruction reordering
- Ensures visibility of changes across threads
- Avoids partially constructed objects

## Advantages
- Thread-safe
- Better performance than synchronized methods
- Lazy initialization

## Disadvantages
- Slightly complex
- Easy to implement incorrectly without `volatile`

## Interview Tip
Double-checked locking minimizes synchronization cost while ensuring thread safety using the `volatile` keyword.
