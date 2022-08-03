---
title: Reader Writer
updated: 2022-01-20 15:29:08Z
created: 2022-01-20 15:27:37Z
source: >-
  https://jojozhuang.github.io/programming/java-concurrency-wait-notify-notifyall/
tags:
  - concurrence
  - s1
---

Producer.

```Java
public class Producer implements Runnable {
    private Message msg;
    
    public Producer(Message msg) {
        this.msg = msg;
    }


    @Override
    public void run() {
        String name = Thread.currentThread().getName();
        System.out.println(name + ": started");
        try {
            Thread.sleep(1000);
            synchronized (msg) 
            {
                msg.setMsg("hello world!");
                System.out.println(name+": message is updated at: " + LocalDateTime.now().toString());
                msg.notify();
                //msg.notifyAll();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

Consumer.

```Java
public class Consumer implements Runnable{
    private Message msg;
 
    public Consumer(Message msg){
        this.msg = msg;
    }
 
    @Override
    public void run() {
        String name = Thread.currentThread().getName();
        synchronized (msg) {
            try{
                System.out.println(name+": waiting to get notified at: " + LocalDateTime.now().toString());
                msg.wait();
            }
            catch(InterruptedException e){
                e.printStackTrace();
            }
            System.out.println(name+": got notified at: " + LocalDateTime.now().toString());
            //process the message now
            System.out.println(name+": message[" + msg.getMsg() + "] is processed.");
        }
    }
}
```