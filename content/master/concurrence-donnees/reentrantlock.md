---
title: ReentrantLock
updated: 2022-01-20 16:14:01Z
created: 2022-01-20 15:40:02Z
latitude: 48.85820000
longitude: 2.33870000
altitude: 0.0000
tags:
  - concurrence
  - s1
---

```java
class ReentrantLock {
    private final Object sync = new Object(); // private monitor
    private Thread lockedBy = null;  // null => unlocked 
    private int lockCount = 0;

    public void lock() throws InterruptedException {
        synchronized (sync) {
            while (lockedBy != null && lockedBy != Thread.currentThread();)
                wait();
            lockedBy = callingThread; // (re)locked! 
            lockCount++;
        }
    }

    public void unlock() {
        synchronized (sync) {
            if (Thread.currentThread() == lockedBy)
                if (--lockCount == 0) {
                    lockedBy = null;      // unlocked! 
                    notify();
                }
        }
    }
}
```

pour le fifo, fo une LinkedBlockingQueue émércé