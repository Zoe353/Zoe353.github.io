---
layout: post
title: "05/05/2020-Adv.-Java-Programming-Threads-in-Java"
markdown: kramdown
date: 2020-05-05
---

Threads allow multiple actions to be performed at the same time inside a single process. Whe nwe have a machine 
with multiple cores, we can run multiple tasks at the same time.  
In programming, a single process can have multiple threads working at the same time. Each thread has its own stack and its own local variables. Threads inside 
the same process are more closely connected, they share memory with other threads. All of the threads have the same access to global variables.  
There are two ways to use threads in Java.  
One of the ways is to use a class that extends the thread class.

```
public class ThreadExample extends Thread{
    @Override
    public void run(){
        int i = 1;
        while(i<=100){
            System.out.println(i + " " + this.getName());
            i++;
        }
    }
}
```

```
public class Main{
    public static void main(String[] args){
        ThreadExample thread1 = new ThreadExample(); // at his point, the thread is in a waiting state, wating to execute code.
        thread1.setName("the fisrt thread");
        thread1.start(); // when we call the start method, the thread is said to be alive.
        ThreadExample thread2 = new ThreadExample();
        thread2.setName("the second thread");
        thread2start();
    }
}
```  
A second way to handle thread in Java is the runnable interface. One benefit of implementing the interface is that we can extend other classes if we wan to.
```

public class RunnableExample implements Runnable{
    @Override
    public void run(){
        int i = 1;
        while(i<=100){
            System.out.println(i + " " + this.getName());
            i++;
        }
    }
}
```  

```
public class Main{
    public static void main(String[] args){
        Thread thread1 = new Thread(new RunnableExample());
        thread1.start();
        
        // when the run method is so simple, we could just do as below
        Thread thread2 = new Thread(new Runnable(){
            @Override
            public void run(){
                int i = 1;
                while(i<=100){
                    System.out.println(i + " " + Thread.currentThread.getName());
                    i++;
                }
            }
        });
        
        //Also, because Runnable interface is a functional interface, we could use lambda expression
        Thread thread2 = new Thread(()->{
            int i = 1;
            while(i<=100){
                System.out.println(i + " " + Thread.currentThread.getName());
                i++;
            }
        });
        thread2.start();
    }
}
```  
Synchronized methods, which is a simple and effective way to stop threads from interfering with object data at the same time.
Add the synchronized keyword right before the return type. This means that now only one thread can enter this method at a time.  
A deadlock can happen when two or more threads get blocked forever.






