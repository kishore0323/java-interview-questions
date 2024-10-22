# Multithreading Interview Questions and Answers

## Q. What are the states in the lifecycle of a Thread?
A java thread can be in any of following thread states during it’s life cycle i.e. New, Runnable, Blocked, Waiting, Timed Waiting or Terminated. These are also called life cycle events of a thread in java. 

* New
* Runnable
* Running
* Non-Runnable (Blocked)
* Terminated

**1. New**: The thread is in new state if you create an instance of Thread class but before the invocation of start() method.  
**2. Runnable**: The thread is in runnable state after invocation of start() method, but the thread scheduler has not selected it to be the running thread.  
**3. Running**: The thread is in running state if the thread scheduler has selected it.  
**4. Non-Runnable (Blocked)**: This is the state when the thread is still alive, but is currently not eligible to run.  
**5. Terminated**: A thread is in terminated or dead state when its run() method exits.  

## Q. What are the different ways of implementing thread? 
There are two ways to create a thread:
* extends Thread class
* implement Runnable interface

**1. Extends Thread class**  
Create a thread by a new class that extends Thread class and create an instance of that class. The extending class must override run() method which is the entry point of new thread.
```java
public class MyThread extends Thread {
     
    public void run() {
      System.out.println("Thread started running..");
    }
    public static void main( String args[] ) {
       MyThread mt = new  MyThread();
       mt.start();
    }
}
```
Output
```
Thread started running..
```
**2. Implementing the Runnable Interface**  
After implementing runnable interface, the class needs to implement the run() method, which is `public void run()`.

* run() method introduces a concurrent thread into your program. This thread will end when run() returns.
* You must specify the code for your thread inside run() method.
* run() method can call other methods, can use other classes and declare variables just like any other normal method.
```java
class MyThread implements Runnable {

    public void run() {
        System.out.println("Thread started running..");
    }
    public static void main(String args[]) {
        MyThread mt = new MyThread();
        Thread t = new Thread(mt);
        t.start();
    }
}
```

**Difference between Runnable vs Thread**  

* Implementing Runnable is the preferred way to do it. Here, you’re not really specializing or modifying the thread’s behavior. You’re just giving the thread something to run. That means composition is the better way to go.
* Java only supports single inheritance, so you can only extend one class.
* Instantiating an interface gives a cleaner separation between your code and the implementation of threads.
* Implementing Runnable makes your class more flexible. If you extend Thread then the action you’re doing is always going to be in a thread. However, if you implement Runnable it doesn’t have to be. You can run it in a thread, or pass it to some kind of executor service, or just pass it around as a task within a single threaded application.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the difference between Process and Thread?
Both processes and threads are independent sequences of execution. The typical difference is that threads run in a **shared memory space**, while processes run in **separate memory spaces**.

**Process**  

* An executing instance of a program is called a process.
* Some operating systems use the term **task** to refer to a program that is being executed.
* A process is always stored in the main memory also termed as the primary memory or random access memory.
* Therefore, a process is termed as an active entity. It disappears if the machine is rebooted.
* Several process may be associated with a same program.
* On a multiprocessor system, multiple processes can be executed in parallel.
* On a uni-processor system, though true parallelism is not achieved, a process scheduling algorithm is applied and the processor is scheduled to execute each process one at a time yielding an illusion of concurrency.


**Thread**  

* A thread is a subset of the process.
* It is termed as a **lightweight process**, since it is similar to a real process but executes within the context of a process and shares the same resources allotted to the process by the kernel.
* Usually, a process has only one thread of control – one set of machine instructions executing at a time.
* A process may also be made up of multiple threads of execution that execute instructions concurrently.
* Multiple threads of control can exploit the true parallelism possible on multiprocessor systems.
* On a uni-processor system, a thread scheduling algorithm is applied and the processor is scheduled to run each thread one at a time.
* All the threads running within a process share the same address space, file descriptors, stack and other process related attributes.
* Since the threads of a process share the same memory, synchronizing the access to the shared data within the process gains unprecedented importance.

## Q. What is difference between user Thread and daemon Thread?
Daemon threads are low priority threads which always run in background and user threads are high priority threads which always run in foreground. User Thread or Non-Daemon are designed to do specific or complex task where as daemon threads are used to perform supporting tasks.

**Difference Between Daemon Threads And User Threads In Java**  

* User threads are created by the application (user) to perform some specific task. Where as daemon threads are mostly created by the JVM to perform some background tasks like garbage collection.

* JVM will wait for user threads to finish their tasks. JVM will not exit until all user threads finish their tasks. On the other side, JVM will not wait for daemon threads to finish their tasks. It will exit as soon as all user threads finish their tasks.
* User threads are high priority threads, They are designed mainly to execute some important task in an application. Where as daemon threads are less priority threads. They are designed to serve the user threads.
* User threads are foreground threads. They always run in foreground and perform some specific task assigned to them. Where as daemon threads are background threads. They always run in background and act in a supporting role to user threads.
* JVM will not force the user threads to terminate. It will wait for user threads to terminate themselves. On the other hand, JVM will force the daemon threads to terminate if all the user threads have finished their task.

**create Daemon Thread**  
```java
/**
 * Java Program to demonstrate difference beween a daemon and a user thread .
 * 
 */
public class DaemonThreadDemo {

    public static void main(String[] args) throws InterruptedException {

        // main thread is a non-daemon thread
        String name = Thread.currentThread().getName();
        boolean isDaemon = Thread.currentThread().isDaemon();

        System.out.println("name: " + name + ", isDaemon: " + isDaemon);

        // Any new thread spawned from main is also non-daemon or user thread
        // as seen below:
        Runnable task = new Task();
        Thread t1 = new Thread(task, "T1");
        System.out.println("Thread spawned from main thread");
        System.out.println("name: " + t1.getName() + ", isDaemon: " + t1.isDaemon());

        // though you can make a daemon thread by calling setDaemon()
        // before starting it as shown below:
        t1.setDaemon(true);
        t1.start();

        // let's wait for T1 to finish
        t1.join();
    }

    private static class Task implements Runnable {

        @Override
        public void run() {
            Thread t = Thread.currentThread();
            System.out.println("Thread made daemon by calling setDaemon() method");
            System.out.println("name: " + t.getName() + ", isDaemon: " + t.isDaemon());

            // Any new thread created from daemon thread is also daemon
            Thread t2 = new Thread("T2");
            System.out.println("Thread spawned from a daemon thread");
            System.out.println("name: " + t2.getName() + ", isDaemon: " + t2.isDaemon());
        }
    }
}
```
Output
```
name: main, isDaemon: false
Thread spawned from main thread
name: T1, isDaemon: false
Thread made daemon by calling setDaemon() method
name: T1, isDaemon: true
Thread spawned from a daemon thread
name: T2, isDaemon: true
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. How does thread communicate with each other?
**Inter-thread communication** is a mechanism in which a thread is paused running in its critical section and another thread is allowed to enter (or lock) in the same critical section to be executed. It is implemented by following methods of Object class:

* wait()
* notify()
* notifyAll()

```java
class ThreadA {

    public static void main(String [] args) {
      ThreadB b = new ThreadB();
      b.start();
      synchronized(b) {
       try {
            System.out.println("Waiting for b to complete...");
            b.wait();
       } catch (InterruptedException e) {}
            System.out.println("Total is: " + b.total);
      }
    }
}

class ThreadB extends Thread {
 int total;

 public void run() {
   synchronized(this) {
    for(int i = 0; i < 100; i++) {
        total += i;
    }
    notify();
   }
  }
}
```
## Q. What do you understand about Thread Priority?
Every thread in Java has a priority that helps the thread scheduler to determine the order in which threads scheduled. The threads with higher priority will usually run before and more frequently than lower priority threads. By default, all the threads had the same priority, i.e., they regarded as being equally distinguished by the scheduler, when a thread created it inherits its priority from the thread that created it.

Default priority of a thread is 5 (NORM_PRIORITY). The value of MIN_PRIORITY is 1 and the value of MAX_PRIORITY is 10.

* public static int MIN_PRIORITY
* public static int NORM_PRIORITY
* public static int MAX_PRIORITY

```java
class TestMultiPriority1 extends Thread {  
  
    public void run() {  
        System.out.println("Running thread name is:" + Thread.currentThread().getName());  
        System.out.println("Running thread priority is:" + Thread.currentThread().getPriority());  
    }  

    public static void main(String args[]) {  
        TestMultiPriority1 m1 = new TestMultiPriority1();  
        TestMultiPriority1 m2 = new TestMultiPriority1();  
        m1.setPriority(Thread.MIN_PRIORITY);  
        m2.setPriority(Thread.MAX_PRIORITY);  
        m1.start();  
        m2.start();  
    }  
}     
```
Output
```
Running thread name is: Thread-0
Running thread priority is: 10
Running thread name is: Thread-1
Running thread priority is: 1    
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is Thread Scheduler and Time Slicing?
**Thread scheduler** in java is the part of the JVM that decides which thread should run. The thread scheduler mainly uses preemptive or time slicing scheduling to schedule the threads.

**Preemptive scheduling**: The highest priority task executes until it enters the waiting or dead states or a higher priority task comes into existence.

**Time slicing**: A task executes for a predefined slice of time and then reenters the pool of ready tasks. The scheduler then determines which task should execute next, based on priority and other factors.

Example: Thread Scheduler
```java
class FirstThread extends Thread {
	public void run(){
		for(int i = 0; i < 10; ++i) {
			System.out.println("I am in first thread");
			try{
				Thread.sleep(1000);
			}
			catch(InterruptedException ie) {
				System.out.println("Exception occurs ");
			}
		}
	}
}

class SecondThread {
	public static void main(String[] args){
		FirstThread ft = new FirstThread();
		ft.start();
		for(int j = 1; j < 10; ++j) {
			System.out.println("I am in second thread");
		}
	}
}
```
Output
```
cmd> java SecondThread
I am in second thread
I am in first thread
I am in second thread
I am in second thread
I am in second thread
I am in second thread
I am in second thread
I am in second thread
I am in second thread
I am in second thread
I am in first thread
I am in first thread
I am in first thread
I am in first thread
I am in first thread
I am in first thread
I am in first thread
I am in first thread
I am in first thread
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is context-switching in multi-threading?
Context Switching is the process of storing and restoring of CPU state so that Thread execution can be resumed from the same point at a later point of time. Context Switching is the essential feature for multitasking operating system and support for multi-threaded environment.

## Q. What is Deadlock? How to analyze and avoid deadlock situation?
**Deadlock** is a programming situation where two or more threads are blocked forever, this situation arises with at least two threads and two or more resources.
```java
class HelloClass {
	public synchronized void first(HiClass hi) {
		try {
			Thread.sleep(1000);
		}
		catch(InterruptedException ie) {}
		System.out.println(" HelloClass is calling 	HiClass second() method");
		hi.second();
	}

	public synchronized void second() {
		System.out.println("I am inside second method of HelloClass");
	}
}

class HiClass {
	public synchronized void first(HelloClass he) {
		try {
			Thread.sleep(1000);
		}
		catch(InterruptedException ie){}
		System.out.println(" HiClass is calling HelloClass second() method");
		he.second();
	}

	public synchronized void second() {
		System.out.println("I am inside second method of HiClass");
	}
}

class DeadlockClass extends Thread {
	HelloClass he = new HelloClass();
	HiClass hi = new HiClass();

	public void demo() {
		this.start();
		he.first(hi);
	} 
	public void run() {
		hi.first(he);
	}

	public static void main(String[] args) {
		DeadlockClass dc = new DeadlockClass();
		dc.demo();
	}
}
```
Output
```
cmd> java DeadlockClass
HelloClass is calling HiClass second() method
HiClass is calling HelloClass second() method
```
**Avoid deadlock**  

**1. Avoid Nested Locks**: This is the most common reason for deadlocks, avoid locking another resource if you already hold one. It’s almost impossible to get deadlock situation if you are working with only one object lock. For example, here is the another implementation of run() method without nested lock and program runs successfully without deadlock situation.
```java
public void run() {
    String name = Thread.currentThread().getName();
    System.out.println(name + ' acquiring lock on ' + obj1);
    synchronized (obj1) {
        System.out.println(name + ' acquired lock on ' + obj1);
        work();
    }
    System.out.println(name + ' released lock on ' + obj1);
    System.out.println(name + ' acquiring lock on ' + obj2);
    synchronized (obj2) {
        System.out.println(name + ' acquired lock on ' + obj2);
        work();
    }
    System.out.println(name + ' released lock on ' + obj2);
    System.out.println(name + ' finished execution.');
}
```
**2. Lock Only What is Required**: You should acquire lock only on the resources you have to work on, for example in above program I am locking the complete Object resource but if we are only interested in one of it’s fields, then we should lock only that specific field not complete object.

**3. Avoid waiting indefinitely**: You can get deadlock if two threads are waiting for each other to finish indefinitely using thread join. If your thread has to wait for another thread to finish, it’s always best to use join with maximum time you want to wait for thread to finish.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is Thread Pool? How can we create Thread Pool in Java?
A thread pool reuses previously created threads to execute current tasks and offers a solution to the problem of thread cycle overhead and resource thrashing. Since the thread is already existing when the request arrives, the delay introduced by thread creation is eliminated, making the application more responsive.

Java provides the Executor framework which is centered around the Executor interface, its sub-interface –**ExecutorService** and the class-**ThreadPoolExecutor**, which implements both of these interfaces. By using the executor, one only has to implement the Runnable objects and send them to the executor to execute.

To use thread pools, we first create a object of ExecutorService and pass a set of tasks to it. ThreadPoolExecutor class allows to set the core and maximum pool size.The runnables that are run by a particular thread are executed sequentially.
```java
// Java program to illustrate  
// ThreadPool 
import java.text.SimpleDateFormat;  
import java.util.Date; 
import java.util.concurrent.ExecutorService; 
import java.util.concurrent.Executors; 
  
// Task class to be executed (Step 1) 
class Task implements Runnable    
{ 
    private String name; 
      
    public Task(String s) { 
        name = s; 
    } 
      
    // Prints task name and sleeps for 1s 
    // This Whole process is repeated 5 times 
    public void run() { 
        try { 
            for (int i = 0; i<=5; i++) { 
                if (i == 0) { 
                    Date d = new Date(); 
                    SimpleDateFormat ft = new SimpleDateFormat("hh:mm:ss"); 
                    System.out.println("Initialization Time for"
                            + " task name - "+ name +" = " +ft.format(d));    
                    //prints the initialization time for every task  
                } 
                else { 
                    Date d = new Date(); 
                    SimpleDateFormat ft = new SimpleDateFormat("hh:mm:ss"); 
                    System.out.println("Executing Time for task name - "+ 
                            name +" = " +ft.format(d));    
                    // prints the execution time for every task  
                } 
                Thread.sleep(1000); 
            } 
            System.out.println(name+" complete"); 
        } 
        catch(InterruptedException e) { 
            e.printStackTrace(); 
        } 
    } 
} 
public class Test 
{ 
     // Maximum number of threads in thread pool 
    static final int MAX_T = 3;              
  
    public static void main(String[] args) { 
        // creates five tasks 
        Runnable r1 = new Task("task 1"); 
        Runnable r2 = new Task("task 2"); 
        Runnable r3 = new Task("task 3"); 
        Runnable r4 = new Task("task 4"); 
        Runnable r5 = new Task("task 5");       
          
        // creates a thread pool with MAX_T no. of  
        // threads as the fixed pool size(Step 2) 
        ExecutorService pool = Executors.newFixedThreadPool(MAX_T);   
         
        // passes the Task objects to the pool to execute (Step 3) 
        pool.execute(r1); 
        pool.execute(r2); 
        pool.execute(r3); 
        pool.execute(r4); 
        pool.execute(r5);  
          
        // pool shutdown ( Step 4) 
        pool.shutdown();     
    } 
} 
```
Output
```
Initialization Time for task name - task 2 = 02:32:56
Initialization Time for task name - task 1 = 02:32:56
Initialization Time for task name - task 3 = 02:32:56
Executing Time for task name - task 1 = 02:32:57
Executing Time for task name - task 2 = 02:32:57
Executing Time for task name - task 3 = 02:32:57
Executing Time for task name - task 1 = 02:32:58
Executing Time for task name - task 2 = 02:32:58
Executing Time for task name - task 3 = 02:32:58
Executing Time for task name - task 1 = 02:32:59
Executing Time for task name - task 2 = 02:32:59
Executing Time for task name - task 3 = 02:32:59
Executing Time for task name - task 1 = 02:33:00
Executing Time for task name - task 3 = 02:33:00
Executing Time for task name - task 2 = 02:33:00
Executing Time for task name - task 2 = 02:33:01
Executing Time for task name - task 1 = 02:33:01
Executing Time for task name - task 3 = 02:33:01
task 2 complete
task 1 complete
task 3 complete
Initialization Time for task name - task 5 = 02:33:02
Initialization Time for task name - task 4 = 02:33:02
Executing Time for task name - task 4 = 02:33:03
Executing Time for task name - task 5 = 02:33:03
Executing Time for task name - task 5 = 02:33:04
Executing Time for task name - task 4 = 02:33:04
Executing Time for task name - task 4 = 02:33:05
Executing Time for task name - task 5 = 02:33:05
Executing Time for task name - task 5 = 02:33:06
Executing Time for task name - task 4 = 02:33:06
Executing Time for task name - task 5 = 02:33:07
Executing Time for task name - task 4 = 02:33:07
task 5 complete
task 4 complete
```
**Risks in using Thread Pools**   

* Deadlock
* Thread Leakage
* Resource Thrashing
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Why wait(), notify() and notifyAll() must be called from inside of the synchronized block or method.?
`wait()` forces the thread to release its lock. This means that it must own the lock of an object before calling the `wait()` method of that (same) object. Hence the thread must be in one of the object's synchronized methods or synchronized block before calling wait().

When a thread invokes an object's `notify()` or `notifyAll()` method, one (an arbitrary thread) or all of the threads in its waiting queue are removed from the waiting queue to the entry queue. They then actively contend for the object's lock, and the one that gets the lock goes on to execute.

## Q. What is the difference between wait() and sleep() method?
**1.  Class  belongs**:  The wait() method belongs to `java.lang.Object` class, thus can be called on any Object. The sleep() method belongs to `java.lang.Thread` class, thus can be called on Threads.  

**2. Context**:  The wait() method can only be called from Synchronized context i.e. using synchronized block or synchronized method. The sleep() method can be called from any context.  

**3. Locking**:  The wait() method releases the lock on an object and gives others chance to execute. The sleep() method does not releases the lock of an object for specified time or until interrupt. 

**4. Wake up condition**:  A waiting thread can be awake by notify() or notifyAll() method. A sleeping can be awaked by interrupt or time expires.  

**5. Execution**:  Each object has each wait() method for inter-communication between threads. The sleep() method is static method belonging to Thread class. There is a common mistake to write t.sleep(1000) because sleep() is a class method and will pause the current running thread not t.  
```java
synchronized(LOCK) {
    Thread.sleep(1000); // LOCK is held
}


synchronized(LOCK) {
    LOCK.wait(); // LOCK is not held
}
```
## Q. What is static synchronization?
Static synchronization is achieved by static synchronized methods. Static synchronized method locked on **class** and non-static synchronized method locked on **current object** i.e. static and non-static synchronized methods can run at same time. It can produce inconsistency problem.

If static synchronized method is called a class level lock is acquired and then if an object is tries to access non-static synchronized method at the same time it will not be accessible because class level lock is already acquired.
```java
/**
 * This program is used to show the multithreading 
 * example with synchronization using static synchronized method.
 * @author codesjava
 */
class PrintTable {    
    public synchronized static void printTable(int n) {
	   System.out.println("Table of " + n);
       for(int i = 1; i <= 10; i++) {
           System.out.println(n*i);  
           try {  
        	 Thread.sleep(500);  
           } catch(Exception e) {
        	 System.out.println(e);
           }  
        }         
    }  
}  
 
class MyThread1 extends Thread {    
    public void run() { 
    	PrintTable.printTable(2);  
    }        
}  
 
class MyThread2 extends Thread {   
	public void run() {  
		PrintTable.printTable(5);  
	}  
}  
 
public class MultiThreadExample {  
    public static void main(String args[]) {
 
    	//creating threads.
	    MyThread1 t1 = new MyThread1();  
	    MyThread2 t2 = new MyThread2();  
 
	    //start threads.
	    t1.start();  
	    t2.start();  
    }  
}
```
Output
```
Table of 2
2
4
6
8
10
12
14
16
18
20
Table of 5
5
10
15
20
25
30
35
40
45
50
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. How is the safety of a thread achieved?
* Immutable objects are by default thread-safe because there state can not be modified once created. Since String is immutable in Java, its inherently thread-safe.
* Read only or final variables in Java are also thread-safe in Java.
* Locking is one way of achieving thread-safety in Java.
* Static variables if not synchronized properly becomes major cause of thread-safety issues.
* Example of thread-safe class in Java: Vector, Hashtable, ConcurrentHashMap, String etc.
* Atomic operations in Java are thread-safe e.g. reading a 32 bit int from memory because its an atomic operation it can't interleave with other thread.
* local variables are also thread-safe because each thread has there own copy and using local variables is good way to writing thread-safe code in Java.
* In order to avoid thread-safety issue minimize sharing of objects between multiple thread.
* Volatile keyword in Java can also be used to instruct thread not to cache variables and read from main memory and can also instruct JVM not to reorder or optimize code from threading perspective.

## Q. What is difference between start() and run() method of thread class?
* When program calls `start()` method a **new Thread** is created and code inside `run()` method is executed in new Thread while if you call `run()` method directly **no new Thread is created** and code inside `run()` will execute on current Thread.

* `start()` Can't be invoked more than one time otherwise throws `java.lang.IllegalStateException`  but `run()` can be invoked multiple times
```java
class MyThread extends Thread
{
    @Override
    public void run() {
        System.out.println("I am executed by " +currentThread().getName());
    }
}
 
public class ThreadExample
{
    public static void main(String[] args) {

        MyThread myThread = new MyThread();
 
        // Calling run() method directly 
        myThread.run();
 
        // Calling start() method. It creates a new thread which executes run() method
        myThread.start();
    }
}
```
Output
```
I am executed by main
I am executed by Thread-0
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Why we use Vector class?
Vector implements a dynamic array that means it can grow or shrink as required. Like an array, it contains components that can be accessed using an integer index. They are very similar to ArrayList but Vector is **synchronised** and have some legacy method which collection framework does not contain. It extends AbstractList and implements List interfaces.
```java
import java.util.*; 
class Vector_demo { 

    public static void main(String[] arg) { 

        // create default vector 
        Vector v = new Vector(); 
        v.add(10); 
        v.add(20); 
        v.add("Numbers");   
        System.out.println("Vector is " + v); 
    } 
}
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is Thread Group? Why it’s advised not to use it?
ThreadGroup creates a group of threads. It offers a convenient way to manage groups of threads as a unit. This is particularly valuable in situation in which you want to suspend and resume a number of related threads.

* The thread group form a tree in which every thread group except the initial thread group has a parent.
* A thread is allowed to access information about its own thread group but not to access information about its thread group’s parent thread group or any other thread group.
```java
// Java code illustrating Thread Group 
import java.lang.*; 
class NewThread extends Thread  
{ 
    NewThread(String threadname, ThreadGroup tgob) { 
        super(tgob, threadname); 
        start(); 
    } 
    public void run() { 
  
        for (int i = 0; i < 1000; i++) { 
            try { 
                Thread.sleep(10); 
            } 
            catch (InterruptedException ex) { 
                System.out.println("Exception encounterted"); 
            } 
        } 
    } 
}  
public class ThreadGroupExample  
{ 
    public static void main(String arg[]) { 
        // creating the thread group 
        ThreadGroup tg = new ThreadGroup("Parent Thread Group"); 
  
        NewThread t1 = new NewThread("One", tg); 
        System.out.println("Starting One"); 
        NewThread t2 = new NewThread("Two", tg); 
        System.out.println("Starting two"); 
  
        // checking the number of active thread 
        System.out.println("Number of active thread: "
                           + tg.activeCount()); 
    } 
} 
```
## Q. How do you stop a thread in java?
A thread is automatically destroyed when the run() method has completed. But it might be required to kill/stop a thread before it has completed its life cycle. Modern ways to suspend/stop a thread are by using a boolean flag and `Thread.interrupt()` method.
```java
class MyThread extends Thread
{
    //Initially setting the flag as true
    private volatile boolean flag = true;
     
    //This method will set flag as false
    public void stopRunning() {
        flag = false;
    }
     
    @Override
    public void run() {
         
        //This will make thread continue to run until flag becomes false
        while (flag) {
            System.out.println("I am running....");
        }
        System.out.println("Stopped Running....");
    }
}
 
public class MainClass 
{   
    public static void main(String[] args) {

        MyThread thread = new MyThread();
        thread.start();
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } 
        //call stopRunning() method whenever you want to stop a thread
        thread.stopRunning();
    }   
}
```
Output 
```
I am running….
I am running….
I am running….
I am running….
I am running….
I am running….
I am running….
I am running….
Stopped Running….
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Can we call run() method of a Thread class?
No, you can not directly call run method to start a thread. You need to call start method to create a new thread.
If you call run method directly, it won’t create a new thread and it will be in same stack as main.
```java
class CustomThread extends Thread {
 
 public void run() {
  for (int i = 0; i < 5; i++) {
    try {
        Thread.sleep(300);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
     System.out.println("Thread is running :"+i);
  }
 }
}
 
public class StartThreadAgainMain {
 
 public static void main(String[] args) {
   CustomThread ct1 = new CustomThread();
   CustomThread ct2 = new CustomThread();
   ct1.run();
   ct2.run();
 }
}
```
Output
```
Thread is running :0
Thread is running :1
Thread is running :2
Thread is running :3
Thread is running :4
Thread is running :0
Thread is running :1
Thread is running :2
Thread is running :3
Thread is running :4
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is difference between Yield and Sleep method in Java?
**1. Currently executing thread state**: sleep()  method causes the currently executing thread to sleep for the number of milliseconds specified in the argument. yield() method temporarily pauses the currently executing thread to give a chance to the remaining waiting threads of the same priority to execute.
If there is no waiting thread or all the waiting threads of low priority then the current thread will continue its execution.

**2. Interrupted Exception**: Sleep method throws the Interrupted exception if another thread interrupts the sleeping thread.  yield method does not throw Interrupted Exception.

**3. Give up monitors**:  Thread.sleep() method does not cause cause currently executing thread to give up any monitors while yield() method give up the monitors.

## Q. What is ThreadLocal?
The Java ThreadLocal class enables you to create variables that can only be read and written by the same thread. Thus, even if two threads are executing the same code, and the code has a reference to the same ThreadLocal variable, the two threads cannot see each other's ThreadLocal variables. Thus, the Java ThreadLocal class provides a simple way to make code thread safe that would not otherwise be so.

**Creating a ThreadLocal**   
```java
private ThreadLocal threadLocal = new ThreadLocal();
```
**Set ThreadLocal Value**
```java
threadLocal.set("A thread local value");
```
**Get ThreadLocal Value**
```java
String threadLocalValue = (String) threadLocal.get();
```
**Remove ThreadLocal Value**
```java
threadLocal.remove();
```
Example
```java
// Java code illustrating get() and set() method 
public class ThreadLocalExample { 
  
    public static void main(String[] args) { 
  
        ThreadLocal<Number> tlObj = new ThreadLocal<Number>(); 
  
        // setting the value 
        tlObj.set(100);  
        System.out.println("value = " + tlObj.get()); 
    } 
} 
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is Java Thread Dump, How can we get Java Thread dump of a Program?
A Java thread dump is a way of finding out what every thread in the JVM is doing at a particular point in time. This is especially useful if your Java application sometimes seems to hang when running under load, as an analysis of the dump will show where the threads are stuck.

You can generate a thread dump under Unix/Linux by running `kill -QUIT <pid>`, and under Windows by hitting `Ctl + Break`.
Thread dump is the list of all the threads, every entry shows information about thread which includes following in the order of appearance.

**Java Thread Dump Example**  
```
2019-12-26 22:28:39
Full thread dump Java HotSpot(TM) 64-Bit Server VM (23.5-b02 mixed mode):

"Attach Listener" daemon prio=5 tid=0x00007fb7d8000000 nid=0x4207 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Timer-0" daemon prio=5 tid=0x00007fb7d4867000 nid=0x5503 waiting on condition [0x00000001604d9000]
   java.lang.Thread.State: TIMED_WAITING (sleeping)
	at java.lang.Thread.sleep(Native Method)
	at com.journaldev.threads.MyTimerTask.completeTask(MyTimerTask.java:19)
	at com.journaldev.threads.MyTimerTask.run(MyTimerTask.java:12)
	at java.util.TimerThread.mainLoop(Timer.java:555)
	at java.util.TimerThread.run(Timer.java:505)

"Service Thread" daemon prio=5 tid=0x00007fb7d482c000 nid=0x5303 runnable [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C2 CompilerThread1" daemon prio=5 tid=0x00007fb7d482b800 nid=0x5203 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C2 CompilerThread0" daemon prio=5 tid=0x00007fb7d4829800 nid=0x5103 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Signal Dispatcher" daemon prio=5 tid=0x00007fb7d4828800 nid=0x5003 runnable [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Finalizer" daemon prio=5 tid=0x00007fb7d4812000 nid=0x3f03 in Object.wait() [0x000000015fd26000]
   java.lang.Thread.State: WAITING (on object monitor)
	at java.lang.Object.wait(Native Method)
	- waiting on <0x0000000140a25798> (a java.lang.ref.ReferenceQueue$Lock)
	at java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:135)
	- locked <0x0000000140a25798> (a java.lang.ref.ReferenceQueue$Lock)
	at java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:151)
	at java.lang.ref.Finalizer$FinalizerThread.run(Finalizer.java:177)

"Reference Handler" daemon prio=5 tid=0x00007fb7d4811800 nid=0x3e03 in Object.wait() [0x000000015fc23000]
   java.lang.Thread.State: WAITING (on object monitor)
	at java.lang.Object.wait(Native Method)
	- waiting on <0x0000000140a25320> (a java.lang.ref.Reference$Lock)
	at java.lang.Object.wait(Object.java:503)
	at java.lang.ref.Reference$ReferenceHandler.run(Reference.java:133)
	- locked <0x0000000140a25320> (a java.lang.ref.Reference$Lock)

"main" prio=5 tid=0x00007fb7d5000800 nid=0x1703 waiting on condition [0x0000000106116000]
   java.lang.Thread.State: TIMED_WAITING (sleeping)
	at java.lang.Thread.sleep(Native Method)
	at com.journaldev.threads.MyTimerTask.main(MyTimerTask.java:33)

"VM Thread" prio=5 tid=0x00007fb7d480f000 nid=0x3d03 runnable 
"GC task thread#0 (ParallelGC)" prio=5 tid=0x00007fb7d500d800 nid=0x3503 runnable 
"GC task thread#1 (ParallelGC)" prio=5 tid=0x00007fb7d500e000 nid=0x3603 runnable 
"GC task thread#2 (ParallelGC)" prio=5 tid=0x00007fb7d5800000 nid=0x3703 runnable 
"GC task thread#3 (ParallelGC)" prio=5 tid=0x00007fb7d5801000 nid=0x3803 runnable 
"GC task thread#4 (ParallelGC)" prio=5 tid=0x00007fb7d5801800 nid=0x3903 runnable 
"GC task thread#5 (ParallelGC)" prio=5 tid=0x00007fb7d5802000 nid=0x3a03 runnable 
"GC task thread#6 (ParallelGC)" prio=5 tid=0x00007fb7d5802800 nid=0x3b03 runnable 
"GC task thread#7 (ParallelGC)" prio=5 tid=0x00007fb7d5803800 nid=0x3c03 runnable 
"VM Periodic Task Thread" prio=5 tid=0x00007fb7d481e800 nid=0x5403 waiting on condition 

JNI global references: 116
```
* **Thread Name**: Name of the Thread
* **Thread Priority**: Priority of the thread
* **Thread ID**: Represents the unique ID of the Thread
* **Thread Status**: Provides the current thread state, for example RUNNABLE, WAITING, BLOCKED. While analyzing deadlock look for the blocked threads and resources on which they are trying to acquire lock.
* **Thread callstack**: Provides the vital stack information for the thread. This is the place where we can see the locks obtained by Thread and if it’s waiting for any lock.

**Tools**  

* jstack
* JVisualVM
* JMC
* ThreadMXBean
* APM Tool – App Dynamics
* JCMD
* VisualVM Profiler  
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What will happen if we don’t override Thread class run() method?
If we don't override Thread class run() method in our defined thread then Thread class run() method will be executed and we will not get any output because Thread class run() is with an empty implementation.

It is highly recommended to override run() method because it improves the performance of the system. If we override run() method in the user-defined thread then in run() method we will define a job and Our created thread is responsible to execute run() method.
```
abstract class NotOverridableRunMethod extends Thread {
	abstract public void run();
}

class ParentMain {
	public static void main(String[] args) {
		OverrideRunMethod orn = new OverrideRunMethod();
		orn.start();
		System.out.println("Thread class run() method will be executed with empty implementation");
	}
}
```
Output
```
cmd> java ParentMain
Thread class run() method will be executed with empty implementation
I am in run() method 
```

## Q. What is difference between the Thread class and Runnable interface for creating a Thread?
A thread can be defined in two ways. First, by extending a **Thread class** that has already implemented a Runnable interface. Second, by directly implementing a **Runnable interface**.

**Difference**  

|THREAD	CLASS                               |RUNNABLE INTERFACE                                             |
|-------------------------------------------|---------------------------------------------------------------|
|Each thread creates a unique object and gets associated with it.|	Multiple threads share the same objects.|
|As each thread create a unique object, more memory required.|As multiple threads share the same object less memory is used.|
|In Java, multiple inheritance not allowed hence, after a class extends Thread class, it can not extend any other class.|	If a class define thread implementing the Runnable interface it has a chance of extending one class.|
|A user must extend thread class only if it wants to override the other methods in Thread class.|If you only want to specialize run method then implementing Runnable is a better option.|
|Extending Thread class introduces tight coupling as the class contains code of Thread class and also the job assigned to the thread|	Implementing Runnable interface introduces loose coupling as the code of Thread is separate form the job of Threads.|

## Q. What does join() method?
`java.lang.Thread` class provides the join() method which allows one thread to wait until another thread completes its execution. 
```java
public class ThreadJoinExample {

    public static void main(String[] args) {
        Thread t1 = new Thread(new MyRunnable(), "t1");
        Thread t2 = new Thread(new MyRunnable(), "t2");
        Thread t3 = new Thread(new MyRunnable(), "t3");
        
        t1.start();
        
        //start second thread after waiting for 2 seconds or if it's dead
        try {
            t1.join(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        t2.start();
        
        //start third thread only when first thread is dead
        try {
            t1.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        t3.start();
        
        //let all threads finish execution before finishing main thread
        try {
            t1.join();
            t2.join();
            t3.join();
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        
        System.out.println("All threads are dead, exiting main thread");
    }

}

class MyRunnable implements Runnable {

    @Override
    public void run() {
        System.out.println("Thread started: "+Thread.currentThread().getName());
        try {
            Thread.sleep(4000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("Thread ended: "+Thread.currentThread().getName());
    }
}
```
Output
```
Thread started: t1
Thread started: t2
Thread ended: t1
Thread started: t3
Thread ended: t2
Thread ended: t3
All threads are dead, exiting main thread
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is race-condition?
Race condition in Java occurs in a multi-threaded environment **when more than one thread try to access a shared resource** (modify, write) at the same time. Since multiple threads try to race each other to finish executing a method thus the name **race condition**.

**Example of race condition in Java**   
```java
class Counter  implements Runnable{
    private int c = 0;

    public void increment() {
        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        c++;
    }

    public void decrement() {    
        c--;
    }

    public int getValue() {
        return c;
    }
    
    @Override
    public void run() {
        //incrementing
        this.increment();
        System.out.println("Value for Thread After increment " 
        + Thread.currentThread().getName() + " " + this.getValue());
        //decrementing
        this.decrement();
        System.out.println("Value for Thread at last " 
        + Thread.currentThread().getName() + " " + this.getValue());        
    }
}

public class RaceConditionExample {
    public static void main(String[] args) {
        Counter counter = new Counter();
        Thread t1 = new Thread(counter, "Thread-1");
        Thread t2 = new Thread(counter, "Thread-2");
        Thread t3 = new Thread(counter, "Thread-3");
        t1.start();
        t2.start();
        t3.start();
    }    
}
```
Output
```
Value for Thread After increment Thread-2 3
Value for Thread at last Thread-2 2
Value for Thread After increment Thread-1 2
Value for Thread at last Thread-1 1
Value for Thread After increment Thread-3 1
Value for Thread at last Thread-3 0
```

**Using synchronization to avoid race condition in Java**  
To fix the race condition we need to have a way to restrict resource access to only one thread at a time. We have to use `synchronized` keyword to synchronize the access to the shared resource. 
```java
//This class' shared object will be accessed by threads
class Counter  implements Runnable{
    private int c = 0;

    public  void increment() {
        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        c++;
    }

    public  void decrement() {    
        c--;        
    }

    public  int getValue() {
        return c;
    }
    
    @Override
    public void run() {
        synchronized(this) {
            // incrementing
            this.increment();
            System.out.println("Value for Thread After increment " 
             + Thread.currentThread().getName() + " " + this.getValue());
            //decrementing
            this.decrement();
            System.out.println("Value for Thread at last " + Thread.currentThread().getName() 
                + " " + this.getValue());
        }        
    }
}

public class RaceConditionExample {
    public static void main(String[] args) {
        Counter counter = new Counter();
        Thread t1 = new Thread(counter, "Thread-1");
        Thread t2 = new Thread(counter, "Thread-2");
        Thread t3 = new Thread(counter, "Thread-3");
        t1.start();
        t2.start();
        t3.start();
    }    
}
```
Output
```
Value for Thread After increment Thread-2 1
Value for Thread at last Thread-2 0
Value for Thread After increment Thread-3 1
Value for Thread at last Thread-3 0
Value for Thread After increment Thread-1 1
Value for Thread at last Thread-1 0
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is Lock interface in Java Concurrency API? What is the Difference between ReentrantLock and Synchronized?
A `java.util.concurrent.locks.Lock` is a thread synchronization mechanism just like synchronized blocks. A Lock is, however, more flexible and more sophisticated than a synchronized block. Since Lock is an interface, you need to use one of its implementations to use a Lock in your applications. `ReentrantLock` is one such implementation of Lock interface.
```java
Lock lock = new ReentrantLock();
 
lock.lock();
 
//critical section
lock.unlock();
```

**Difference between Lock Interface and synchronized keyword**  

* Having a timeout trying to get access to a `synchronized` block is not possible. Using `Lock.tryLock(long timeout, TimeUnit timeUnit)`, it is possible.
* The `synchronized` block must be fully contained within a single method. A Lock can have it’s calls to `lock()` and `unlock()` in separate methods.

```java
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ConcurrencyLockExample implements Runnable {

	private Resource resource;
	private Lock lock;
	
	public ConcurrencyLockExample(Resource r) {
		this.resource = r;
		this.lock = new ReentrantLock();
	}
	
	@Override
	public void run() {
		try {
			if(lock.tryLock(10, TimeUnit.SECONDS)) {
			  resource.doSomething();
			}
		} catch (InterruptedException e) {
			e.printStackTrace();
		} finally {
			//release lock
			lock.unlock();
		}
		resource.doLogging();
	}
}
```
## Q. What is the difference between the Runnable and Callable interface?
Runnable and Callable interface both are used in the multithreading environment. Runnable is the core interface provided for representing multi-threaded tasks and Callable is an improved version of Runnable that was added in Java 1.5.

**Difference between Callable and Runnable in Java**  

**1. Checked Exception**: Callable's call() method can throw checked exception while  Runnable run() method can  not throw checked exception.

**2. Return value**: Return type of Runnable run() method is void , so it can not return any value. while Callable can return the Future object, which represents the life cycle of a task and provides methods to check if the task has been completed or canceled.

**3. Implementation**: Callable needs to implement call() method while Runnable needs to implement run() method.

**4. Execution**: Limitation of Callable interface lies in java is that one can not pass it to Thread as one pass the Runnable instance. There is no constructor defined in the Thread class which accepts a Callable interface. 

Example: Callable Interface
```java
// Java program to illustrate Callable and FutureTask 
// for random number generation 
import java.util.Random; 
import java.util.concurrent.Callable; 
import java.util.concurrent.FutureTask; 
  
class CallableExample implements Callable 
{ 
  
  public Object call() throws Exception { 
    Random generator = new Random(); 
    Integer randomNumber = generator.nextInt(5); 
    Thread.sleep(randomNumber * 1000); 
    return randomNumber; 
  } 
} 
  
public class CallableFutureTest 
{ 
  public static void main(String[] args) throws Exception { 
  
    // FutureTask is a concrete class that 
    // implements both Runnable and Future 
    FutureTask[] randomNumberTasks = new FutureTask[5]; 
  
    for (int i = 0; i < 5; i++) { 
      Callable callable = new CallableExample(); 
  
      // Create the FutureTask with Callable 
      randomNumberTasks[i] = new FutureTask(callable); 
  
      // As it implements Runnable, create Thread 
      // with FutureTask 
      Thread t = new Thread(randomNumberTasks[i]); 
      t.start(); 
    } 
  
    for (int i = 0; i < 5; i++) { 
      // As it implements Future, we can call get() 
      System.out.println(randomNumberTasks[i].get()); 
    } 
  } 
}
```
Output
```
4
2
3
3
0
```
Example: Runnable Interface
```java
// Java program to illustrate Runnable 
// for random number generation 
import java.util.Random; 
import java.util.concurrent.Callable; 
import java.util.concurrent.FutureTask; 
  
class RunnableExample implements Runnable 
{ 
    // Shared object to store result 
    private Object result = null; 
  
    public void run() { 
        Random generator = new Random(); 
        Integer randomNumber = generator.nextInt(5); 
  
        // As run cannot throw any Exception 
        try { 
            Thread.sleep(randomNumber * 1000); 
        } catch (InterruptedException e) { 
            e.printStackTrace(); 
        } 
  
        // Store the return value in result when done 
        result = randomNumber; 
  
        // Wake up threads blocked on the get() method 
        synchronized(this) { 
            notifyAll(); 
        } 
    } 
  
    public synchronized Object get() throws InterruptedException  { 
        while (result == null) 
            wait(); 
  
        return result; 
    } 
} 

public class RunnableTest 
{ 
    public static void main(String[] args) throws Exception { 
        RunnableExample[] randomNumberTasks = new RunnableExample[5]; 
  
        for (int i = 0; i < 5; i++) { 
            randomNumberTasks[i] = new RunnableExample(); 
            Thread t = new Thread(randomNumberTasks[i]); 
            t.start(); 
        } 
  
        for (int i = 0; i < 5; i++) 
            System.out.println(randomNumberTasks[i].get()); 
    } 
} 
```
Output
```
0
4
3
1
4
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the Thread’s interrupt flag? How does it relate to the InterruptedException?
If any thread is in sleeping or waiting state (i.e. sleep() or wait() is invoked), calling the interrupt() method on the thread, breaks out the sleeping or waiting state throwing InterruptedException. If the thread is not in the sleeping or waiting state, calling the interrupt() method performs normal behaviour and doesn't interrupt the thread but sets the interrupt flag to true.

Example: **Interrupting a thread that stops working**
```java
// Java Program to illustrate the concept of interrupt() method 
// while a thread stops working 
class ThreadsInterruptExample extends Thread { 

    public void run() { 
        try { 
            Thread.sleep(1000); 
            System.out.println("Task"); 
        } catch (InterruptedException e) { 
            throw new RuntimeException("Thread interrupted"); 
        } 
    } 
    public static void main(String args[]) { 
        ThreadsInterruptExample t1 = new ThreadsInterruptExample(); 
        t1.start(); 
        try { 
            t1.interrupt(); 
        } 
        catch (Exception e) { 
            System.out.println("Exception handled"); 
        } 
    } 
} 
```
Output
```
Exception in thread "Thread-0" java.lang.RuntimeException: Thread interrupted
```
## Q. What is Java Memory Model (JMM)? Describe its purpose and basic ideas.
The Java memory model used internally in the JVM divides memory between thread stacks and the heap. Each thread running in the Java virtual machine has its own thread stack. The thread stack contains information about what methods the thread has called to reach the current point of execution. 

The thread stack also contains all local variables for each method being executed (all methods on the call stack). A thread can only access it's own thread stack. Local variables created by a thread are invisible to all other threads than the thread who created it. Even if two threads are executing the exact same code, the two threads will still create the local variables of that code in each their own thread stack. Thus, each thread has its own version of each local variable.

All local variables of primitive types ( boolean, byte, short, char, int, long, float, double) are fully stored on the thread stack and are thus not visible to other threads. One thread may pass a copy of a pritimive variable to another thread, but it cannot share the primitive local variable itself.

The heap contains all objects created in your Java application, regardless of what thread created the object. This includes the object versions of the primitive types (e.g. Byte, Integer, Long etc.). It does not matter if an object was created and assigned to a local variable, or created as a member variable of another object, the object is still stored on the heap.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Describe the conditions of livelock and starvation?
**Livelock** occurs when two or more processes continually repeat the same interaction in response to changes in the other processes without doing any useful work. These processes are not in the waiting state, and they are running concurrently. This is different from a deadlock because in a deadlock all processes are in the waiting state.
```java
var l1 = .... // lock object like semaphore or mutex etc 
var l2 = .... // lock object like semaphore or mutex etc 
      
    // Thread1 
    Thread.Start( ()=> { 
              
    while (true) { 
          
        if (!l1.Lock(1000)) { 
            continue; 
        } 
        if (!l2.Lock(1000)) { 
            continue; 
        } 
          
        // do some work 
    }); 
  
    // Thread2 
    Thread.Start( ()=> { 
            
      while (true) { 
            
        if (!l2.Lock(1000)) { 
            continue; 
        }   
        if (!l1.Lock(1000)) { 
            continue; 
        }   
        // do some work 
    }); 
```

**starvation** describes a situation where a greedy thread holds a resource for a long time so other threads are blocked forever. The blocked threads are waiting to acquire the resource but they never get a chance. Thus they starve to death.

Starvation can occur due to the following reasons:

* Threads are blocked infinitely because a thread takes long time to execute some synchronized code (e.g. heavy I/O operations or infinite loop).

* A thread doesn’t get CPU’s time for execution because it has low priority as compared to other threads which have higher priority.

* Threads are waiting on a resource forever but they remain waiting forever because other threads are constantly notified instead of the hungry ones.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. How do I share a variable between 2 Java threads?
We should declare such variables as static and volatile.

**Volatile variables** are shared across multiple threads. This means that individual threads won’t cache its copy in the thread local. But every object would have its own copy of the variable so threads may cache value locally.

We know that static fields are shared across all the objects of the class, and it belongs to the class and not the individual objects. But, for static and non-volatile variable also, threads may cache the variable locally.

## Q. What are the main components of concurrency API?
The concurrency API are designed and optimized specifically for synchronized multithreaded access. They are grouped under the `java.util.concurrent` package.

**Main Components**  

* Executor
* ExecutorService
* ScheduledExecutorService
* Future
* CountDownLatch
* CyclicBarrier
* Semaphore
* ThreadFactory
* BlockingQueue
* DelayQueue
* Locks
* Phaser

## Q. What is difference between CyclicBarrier and CountDownLatch in Java?
Both CyclicBarrier and CountDownLatch are used to implement a scenario where one Thread waits for one or more Thread to complete their job before starts processing. The differences are:

**1. Definition**:  CountDownLatch  is a synchronization aid that allows one or more threads to wait until a set of operations being performed in other threads completes.

CyclicBarrier is a synchronization aid that allows a set of threads to all wait for each other to reach a common barrier point.

**2. Reusable**: A CountDownLatch is initialized with given count. count reaches zero by calling of countDown() method. The count can not be reset.

CyclicBarrier is used to reset count. The barrier is called cyclic because it can be reused after the waiting threads are released (or count  become zero).  

## Q. What is Semaphore in Java concurrency?
A Semaphore is a thread synchronization construct that can be used either to send signals between threads to avoid missed signals, or to guard a critical section like you would with a lock. Java 5 comes with semaphore implementations in the java.util.concurrent package.   

**1. Simple Semaphore**: 
```java
public class Semaphore {
  private boolean signal = false;

  public synchronized void take() {
    this.signal = true;
    this.notify();
  }

  public synchronized void release() throws InterruptedException{
    while(!this.signal) wait();
    this.signal = false;
  }
}
```
The take() method sends a signal which is stored internally in the Semaphore. The release() method waits for a signal. When received the signal flag is cleared again, and the release() method exited.

**2. Counting Semaphore**: 
```java
public class CountingSemaphore {
  private int signals = 0;

  public synchronized void take() {
    this.signals++;
    this.notify();
  }

  public synchronized void release() throws InterruptedException{
    while(this.signals == 0) wait();
    this.signals--;
  }
}
```
**3. Bounded Semaphore**   
```java
public class BoundedSemaphore {
  private int signals = 0;
  private int bound   = 0;

  public BoundedSemaphore(int upperBound){
    this.bound = upperBound;
  }

  public synchronized void take() throws InterruptedException{
    while(this.signals == bound) wait();
    this.signals++;
    this.notify();
  }

  public synchronized void release() throws InterruptedException{
    while(this.signals == 0) wait();
    this.signals--;
    this.notify();
  }
}
```
**4. Using Semaphores as Locks**  
```java
BoundedSemaphore semaphore = new BoundedSemaphore(1);

...

semaphore.take();

try{
  //critical section
} finally {
  semaphore.release();
}
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is Callable and Future in Java concurrency?
Future and FutureTask in Java allows to write asynchronous code. A Future interface provides methods **to check if the computation is complete, to wait for its completion and to retrieve the results of the computation**. The result is retrieved using Future’s get() method when the computation has completed, and it blocks until it is completed. We need a callable object to create a future task and then we can use Java Thread Pool Executor to process these asynchronously.

```java
import java.util.concurrent.Callable; 
import java.util.concurrent.ExecutionException; 
import java.util.concurrent.ExecutorService; 
import java.util.concurrent.Executors; 
import java.util.concurrent.Future; 
import java.util.logging.Level; 
import java.util.logging.Logger; 
/** 
* Java program to show how to use Future in Java. Future allows to write 
* asynchronous code in Java, where Future promises result to be available in 
* future  
**/ 
public class FutureExample { 

    private static final ExecutorService threadpool = Executors.newFixedThreadPool(3); 

    public static void main(String args[]) throws InterruptedException, ExecutionException { 
        
        FactorialCalculator task = new FactorialCalculator(10); 
        System.out.println("Submitting Task ..."); 
        
        Future future = threadpool.submit(task); 
        
        System.out.println("Task is submitted"); 
        
        while (!future.isDone()) { 
            System.out.println("Task is not completed yet...."); 
            Thread.sleep(1); //sleep for 1 millisecond before checking again 
        } 
        System.out.println("Task is completed, let's check result"); 
        long factorial = future.get(); 
        System.out.println("Factorial of 1000000 is : " + factorial); 
        
        threadpool.shutdown(); 
    } 
    private static class FactorialCalculator implements Callable { 
        
        private final int number; 
        
        public FactorialCalculator(int number) { 
            this.number = number; 
        } 
        @Override public Long call() { 
            long output = 0; 
            try { 
                output = factorial(number); 
            } catch (InterruptedException ex) { 
                Logger.getLogger(Test.class.getName()).log(Level.SEVERE, null, ex); 
            } 
            return output; 
        } 
        private long factorial(int number) throws InterruptedException { 
            if (number < 0) { 
                throw new IllegalArgumentException("Number must be greater than zero"); 
            } 
            long result = 1; 
            while (number > 0) { 
                Thread.sleep(1); // adding delay for example 
                result = result * number; 
                number--; 
            } 
            return result; 
        } 
    } 
} 
```
Output
```
Submitting Task ... 
Task is submitted Task is not completed yet.... 
Task is not completed yet.... 
Task is not completed yet.... 
Task is completed, let's check result 
Factorial of 1000000 is : 3628800
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is blocking method in Java?
Blocking methods in Java are those methods which block the executing thread until their operation finished. Example of blocking method is `InputStream read()` method which blocks until all data from InputStream has been read completely.

```java
public class BlcokingCallTest {

    public static void main(String args[]) throws FileNotFoundException, IOException  {
      System.out.println("Calling blocking method in Java");
      int input = System.in.read();
      System.out.println("Blocking method is finished");
    }  
}
```
**Examples of blocking methods in Java:**  

* **InputStream.read()** which blocks until input data is available, an exception is thrown or end of Stream is detected.
* **ServerSocket.accept()** which listens for incoming socket connection in Java and blocks until a connection is made.
* **InvokeAndWait()** wait until code is executed from Event Dispatcher thread.

## Q. What is atomic variable in Java?
Atomic variables allow us to perform atomic operations on the variables. The most commonly used atomic variable classes in Java are `AtomicInteger`, `AtomicLong`, `AtomicBoolean`, and `AtomicReference`. These classes represent an int, long, boolean and object reference respectively which can be atomically updated. The main methods exposed by these classes are:

*  `get()`: gets the value from the memory, so that changes made by other threads are visible; equivalent to reading a volatile variable
*  `set()`: writes the value to memory, so that the change is visible to other threads; equivalent to writing a volatile variable
*  `lazySet()`: eventually writes the value to memory, may be reordered with subsequent relevant memory operations. One use case is nullifying references, for the sake of garbage collection, which is never going to be accessed again. In this case, better performance is achieved by delaying the null volatile write
*  `compareAndSet()`: same as described in section 3, returns true when it succeeds, else false
*  `weakCompareAndSet()`: same as described in section 3, but weaker in the sense, that it does not create happens-before  orderings. This means that it may not necessarily see updates made to other variables

```java
public class SafeCounterWithoutLock {

    private final AtomicInteger counter = new AtomicInteger(0);
     
    public int getValue() {
        return counter.get();
    }
    public void increment() {
        while(true) {
            int existingValue = getValue();
            int newValue = existingValue + 1;
            if(counter.compareAndSet(existingValue, newValue)) {
                return;
            }
        }
    }
}
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is Executors Framework?
The Executor framework helps to **decouple a command submission from command execution**. In the java.util.concurrent package there are three interfaces:

**1. Executor** — Used to submit a new task.

**2. ExecutorService** — A subinterface of Executor that adds methods to manage lifecycle of threads used to run the submitted tasks and methods to produce a Future to get a result from an asynchronous computation.

**3. ScheduledExecutorService** — A subinterface of ExecutorService, to execute commands periodically or after a given delay.

Example: **Java ExecutorService**  
```java
ExecutorService executorService = Executors.newFixedThreadPool(10);

executorService.execute(new Runnable() {
    public void run() {
        System.out.println("Asynchronous task");
    }
});

executorService.shutdown();
```
First an ExecutorService is created using the Executors newFixedThreadPool() factory method. This creates a thread pool with 10 threads executing tasks.

Second, an anonymous implementation of the Runnable interface is passed to the execute() method. This causes the Runnable to be executed by one of the threads in the ExecutorService.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What are the available implementations of ExecutorService in the standard library?
The ExecutorService interface has three standard implementations:

* **ThreadPoolExecutor**: for executing tasks using a pool of threads. Once a thread is finished executing the task, it goes back into the pool. If all threads in the pool are busy, then the task has to wait for its turn.
* **ScheduledThreadPoolExecutor**: allows to schedule task execution instead of running it immediately when a thread is available. It can also schedule tasks with fixed rate or fixed delay.
* **ForkJoinPool**: is a special ExecutorService for dealing with recursive algorithms tasks. If you use a regular ThreadPoolExecutor for a recursive algorithm, you will quickly find all your threads are busy waiting for the lower levels of recursion to finish. The ForkJoinPool implements the so-called work-stealing algorithm that allows it to use available threads more efficiently.

## Q. What kind of thread is the Garbage collector thread?
* Daemon thread
## Q. How can we pause the execution of a Thread for specific time?
* **Thread.sleep(...)**: causes the currently executing thread to sleep (cease execution) for the specified number of milliseconds plus the specified number of nanoseconds, subject to the precision and accuracy of system timers and schedulers. The thread does not lose ownership of any monitors;
* **Thread.yield()**: causes the currently executing thread object to temporarily pause and allow other threads to execute;
* **Monitors**: you call wait on the object you want to lock and you release the lock by calling notify on the same object.

## Q. What is difference between Executor.submit() and Executer.execute() method?
**execute()**:  

* Takes Runnable object as parameter.
* Returns void.
* Useful when you want to execute a task asynchronously in thread pool but doesn't bother about it's result.
* Example: Delegating a request (for which no response required) to another service, sending an email.

**submit()**:  

* Takes Runnable or Callable object as parameter.
* Returns Future object.
* Useful when the calling thread needs the output from the task executed. Using Future object, you can get result, check whether the task is completed without failure or can request cancelling the task before its completion.
* Example: Parallel stream search in Java 8.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is Phaser in Java concurrency?
The Phaser allows us to build logic in which **threads need to wait on the barrier before going to the next step of execution**. Phaser is similar to other synchronization barrier utils like CountDownLatch and CyclicBarrier. 

**CountDownLatch vs CyclicBarrier vs Phaser**  

**1. CountDownLatch**:

* Created with a fixed number of threads
* Cannot be reset
* Allows threads to wait(CountDownLatch#await()) or continue with its execution(CountDownLatch#countDown()).

**2. CyclicBarrier**:  

* Can be reset.
* Does not a provide a method for the threads to advance. The threads have to wait till all the threads arrive.
* Created with fixed number of threads.

**3. Phaser**: 

* Number of threads need not be known at Phaser creation time. They can be added dynamically.
* Can be reset and hence is, reusable.
* Allows threads to wait(Phaser#arriveAndAwaitAdvance()) or continue with its execution(Phaser#arrive()).
* Supports multiple Phases(hence the name phaser).

**PhaserExample.java**  
```java
import java.util.concurrent.Phaser;
 
public class PhaserExample
{
    public static void main(String[] args) throws InterruptedException
    {
          Phaser phaser = new Phaser();
          phaser.register();//register self... phaser waiting for 1 party (thread)
          int phasecount = phaser.getPhase();
          
          System.out.println("Phasecount is "+phasecount);
          new PhaserExample().testPhaser(phaser,2000);//phaser waiting for 2 parties
          new PhaserExample().testPhaser(phaser,4000);//phaser waiting for 3 parties
          new PhaserExample().testPhaser(phaser,6000);//phaser waiting for 4 parties
          //now that all threads are initiated, we will de-register main thread 
          //so that the barrier condition of 3 thread arrival is meet.
          phaser.arriveAndDeregister();
                  Thread.sleep(10000);
                  phasecount = phaser.getPhase();
          System.out.println("Phasecount is "+phasecount);
 
    }
 
    private void testPhaser(final Phaser phaser,final int sleepTime) {
        phaser.register();
        new Thread() {
            @Override
            public void run() {
                        try {
                            System.out.println(Thread.currentThread().getName()+" arrived");
                            phaser.arriveAndAwaitAdvance();//threads register arrival to the phaser.
                            Thread.sleep(sleepTime);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    System.out.println(Thread.currentThread().getName()+" after passing barrier");
                }
            }.start();
    }
}
```
Output
```
Phasecount is 0
Thread-0 arrived
Thread-2 arrived
Thread-1 arrived
Thread-0 after passing barrier
Thread-1 after passing barrier
Thread-2 after passing barrier
Phasecount is 1
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. How to stop a Thread in Java?
A thread is automatically destroyed when the run() method has completed. But it might be required to kill/stop a thread before it has completed its life cycle. Modern ways to suspend/stop a thread are by using a **boolean flag** and **Thread.interrupt()** method.

Example: Stop a thread Using a boolean variable
```java
/**
* Java program to illustrate 
* stopping a thread using boolean flag 
*
**/
class MyThread extends Thread {

    // Initially setting the flag as true 
    private volatile boolean flag = true;
     
    // This method will set flag as false
    public void stopRunning() {
        flag = false;
    }
     
    @Override
    public void run() {
                 
        // This will make thread continue to run until flag becomes false 
        while (flag) {
            System.out.println("I am running....");
        }
        System.out.println("Stopped Running....");
    }
}
 
public class MainClass {

    public static void main(String[] args) {

        MyThread thread = new MyThread();
        thread.start();
         
        try {
            Thread.sleep(100);
        } 
        catch (InterruptedException e) {
            e.printStackTrace();
        }
         
        // call stopRunning() method whenever you want to stop a thread
        thread.stopRunning();
    }   
}
```
Output:
```java
I am running….
I am running….
I am running….
I am running….
I am running….
Stopped Running….
```

Example: Stop a thread Using interrupt() Method
```java
/**
* Java program to illustrate 
* stopping a thread using interrupt() method 
*
**/
class MyThread extends Thread {

    @Override
    public void run() {

        while (!Thread.interrupted()) {
            System.out.println("I am running....");
        }
        System.out.println("Stopped Running.....");
    }
}
 
public class MainClass {

    public static void main(String[] args) {

        MyThread thread = new MyThread(); 
        thread.start();
         
        try {
            Thread.sleep(100);
        } 
        catch (InterruptedException e) {
            e.printStackTrace();
        }

        // interrupting the thread         
        thread.interrupt();
    }   
}
```
Output:
```java
I am running….
I am running….
I am running….
I am running….
I am running….
Stopped Running….
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Why implementing Runnable is better than extending thread?
| Features	           |implements Runnable	   | extends Thread |
|----------------------|-----------------------|----------------|
|Inheritance option	   |extends any java class | No             |
|Reusability	       |Yes	                   | No             |
|Object Oriented Design|Good,allows composition| Bad            |
|Loosely Coupled	   |Yes 	               | No             |
|Function Overhead	   |No	                   | Yes            |

Example:
```java
/**
* Java program to illustrate defining Thread 
* by extending Thread class 
*
**/
// Here we cant extends any other class 
class ThreadExample extends Thread  
{ 
    public void run() { 
        System.out.println("Run method executed by child Thread"); 
    } 
    public static void main(String[] args) { 
        ThreadExample obj = new ThreadExample(); 
        obj.start(); 
        System.out.println("Main method executed by main thread"); 
    } 
} 
```
Output
```
Main method executed by main thread
Run method executed by child Thread
```
```java
/**
* Java program to illustrate defining Thread 
* by implements Runnable interface 
*
**/
class RunnableExample 
{ 
    public static void m1() { 
        System.out.println("Runnable interface Example"); 
    } 
} 
  
// Here we can extends any other class 
class Test extends RunnableExample implements Runnable 
{ 
    public void run() { 
        System.out.println("Run method executed by child Thread"); 
    } 
    public static void main(String[] args) { 
        Test t = new Test(); 
        t.m1(); 
        Thread t1 = new Thread(t); 
        t1.start(); 
        System.out.println("Main method executed by main thread"); 
    } 
} 
```
Output
```
Runnable interface Example
Main method executed by main thread
Run method executed by child Thread
```
<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Tell me about join() and wait() methods?
The `wait()` and `join()` methods are used to pause the current thread. The `wait()` is used in with `notify()` and `notifyAll()` methods, but `join()` is used in Java to wait until one thread finishes its execution. 

The `wait()` is mainly used for shared resources, a thread notifies other waiting thread when a resource becomes free. On the other hand `join()` is used for waiting a thread to die.

## Q. How to implement thread-safe code without using the synchronized keyword? 
* **Atomic updates**: A technique in which you call atomic instructions like compare and set provided by the CPU
* **java.util.concurrent.locks.ReentrantLock**: A lock implementation that provides more flexibility than synchronized blocks
* **java.util.concurrent.locks.ReentrantReadWriteLock**: A lock implementation in which reads do not block reads
* **java.util.concurrent.locks.StampedLock** a nonreeantrant Read-Write lock with the possibility of optimistically reading values.
* **java.lang.ThreadLocal**: No need for synchronization if the mutable state is confined to a single thread. This can be done by using local variables or `java.lang.ThreadLocal`.

#### Q. What is difference between ArrayBlockingQueue & LinkedBlockingQueue in Java Concurrency?
#### Q. What is PriorityBlockingQueue in Java Concurrency?
#### Q. What is DelayQueue in Java Concurrency?
#### Q. What is SynchronousQueue in Java?
#### Q. What is Exchanger in Java concurrency?
#### Q. What is Busy Spinning? Why will you use Busy Spinning as wait strategy?

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>


Java 7 introduced a new type of `ExecutorService` ([Java Doc](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ExecutorService.html)) called **Fork/Join Framework** ([Tutorial](https://docs.oracle.com/javase/tutorial/essential/concurrency/forkjoin.html)), which excels in handling recursive algorithms.  Different from other implementations of the `ExecutorService`, the Fork/Join Framework uses a work-stealing algorithm ([Paper](http://gee.cs.oswego.edu/dl/papers/fj.pdf)), which maximise the threads utilisation, and provides a simpler way to deal with tasks which spawn other tasks (referred to as _subtasks_).

All code listed below is available at: [https://github.com/javacreed/java-fork-join-example](https://github.com/javacreed/java-fork-join-example).  Most of the examples will not contain the whole code and may omit fragments which are not relevant to the example being discussed.  The readers can download or view all code from the above link.

This article provides a brief description of what is referred to as traditional executor service (so to called them) and how these work.  It then introduces the Fork/Join Framework and describes how this differentiates from the traditional executor service.  The third section in this article shows a practical example of the Fork/Join Framework and demonstrates most of its main components.

## Executor Service

A bank, or post office, has several counters from where the customers are served at the branches.  When a counter finishes with the current customer, the next customer in the queue will take its place and the person behind the counter, also referred to as the _employee_, starts serving the new customer.  The employee will only serve one customer at a given point in time and the customers in the queue need to wait for their turn.  Furthermore, the employees are very patient and will never ask their customers to leave or step aside, even if these are waiting for something else to happen.  The following image shows a simple view of the customers waiting and the employees serving the customers at the head of the queue.

![image](https://github.com/user-attachments/assets/c888ca02-cb85-436c-a370-0f6f85d9ae44)


Something similar happens in a multithreaded program where the `Thread`s ([Java Doc](http://docs.oracle.com/javase/7/docs/api/java/lang/Thread.html)) represents the employees and the tasks to be carried out are the customers.  The following image is identical to the above, with just the labels updated to use the programming terminology.

![image](https://github.com/user-attachments/assets/9854c61a-73a3-49a7-a320-1451c4d5a9dc)


This should help you relate these two aspects and better visualise the scenario being discussed.

Most of the thread pools ([Tutorial](https://docs.oracle.com/javase/tutorial/essential/concurrency/pools.html)) and executor services work in this manner.  A `Thread` is assigned a _task_ and will only move to the next _task_ once the one at hand is finished.  Tasks can take quite a long time to finish and may block waiting for something else to happen.  This works well in many cases, but fails badly with problems that need to be solved recursively.

Let us use the same analogy that we used before with the customer waiting in the queue.  Say that _Customer 1_, who is being served by _Employee 1_, needs some information from _Customer 6_, who is not yet in the queue.  He or she (_Customer 1_) calls their friend (_Customer 6_) and waits for him or her (_Customer 6_) to come to the bank.  In the meantime, _Customer 1_ stays at the counter occupying _Employee 1_.  As mentioned before, the employees are very patient and will never send a customer back to the queue or ask them to step aside until all his or her dependencies are resolved.  _Customer 6_ arrives and queues as shown below.

![image](https://github.com/user-attachments/assets/df4c1676-78b8-4813-b130-7f542f1c97f4)

With _Customer 1_ still occupying an employee, and for the sake of this argument the other customers, _Customer 2_ and _Customer 3_, too do the same (that is wait for something which is queued), then we have a deadlock.  All employees are occupied by customers that are waiting for something to happen.  Therefore the employees will never be free to serve the other customers.

In this example we saw a weakness of the traditional executor services when dealing with tasks, which in turn depend on other tasks created by them (referred to as subtasks).  This is very common in recursive algorithms such as Towers of Hanoi ([Wiki](http://en.wikipedia.org/wiki/Tower_of_Hanoi)) or exploring a tree like data structure (calculating the total size of a directory).  The Fork/Join Framework was designed to address such problems as we will see in the following section.  Later on in this article we will also see an example of the problem discussed in this section.

## Fork/Join Framework

The main weakness of the traditional executor service implementations when dealing with tasks, which in turn depend on other subtasks, is that a thread is not able to put a task back to the queue or to the side and then serves/executes an new task.  The Fork/Join Framework addresses this limitation by introducing another layer between the tasks and the threads executing them, which allows the threads to put blocked tasks on the side and deal with them when all their dependencies are executed.  In other words, if _Task 1_ depends on _Task 6_, which task (_Task 6_) was created by _Task 1_, then _Task 1_ is placed on the side and is only executed once _Task 6_ is executed.  This frees the thread from _Task 1_, and allows it to execute other tasks, something which is not possible with the traditional executor service implementations.

This is achieved by the use of _fork_ and _join_ operations provided by the framework (hence the name Fork/Join).  _Task 1_ forks _Task 6_ and then joins it to wait for the result.  The fork operation puts _Task 6_ on the queue while the join operation allows _Thread 1_ to put _Task 1_ on the side until _Task 6_ completes.  This is how the fork/join works, fork pushes new things to the queue while the join causes the current task to be sided until it can proceed, thus blocking no threads.

The Fork/Join Framework makes use of a special kind of thread pool called `ForkJoinPool` ([Java Doc](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ForkJoinPool.html)), which differentiates it from the rest.  `ForkJoinPool` implements a work-stealing algorithm and can execute `ForkJoinTask` ([Java Doc](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ForkJoinTask.html)) objects.  The `ForkJoinPool` maintains a number of threads, which number is typically based on the number of CPUs available.  Each thread has a special kind of queue, `Deque`s ([Java Doc](https://docs.oracle.com/javase/7/docs/api/java/util/Deque.html)), where all its tasks are placed.  This is quite an important point to understand.  The threads do not share a common queue, but each thread has its own queue as shown next.

![image](https://github.com/user-attachments/assets/93bbcee0-c63b-498a-b4ab-20c7714d3492)

The above image illustrates another queue that each thread has (lower part of the image).  This queue, so to call it, allows the threads to put aside tasks which are blocked waiting for something else to happen.  In other words, if the current task cannot proceed (as it performs a _join_ on a subtask), then it is placed on this queue until all of its dependencies are ready.

New tasks are added to the thread's queue (using the _fork_ operation) and each thread always processes the last task added to its queue.  This is quite important.  If the queue of a thread has two tasks, the last task added to the queue is processed first.  This is referred to as last in first out, LIFO ([Wiki](http://en.wikipedia.org/wiki/LIFO_%28computing%29)).  

![image](https://github.com/user-attachments/assets/cbab8e1d-c562-45e2-9fb3-c109ee227be8)

In the above image, _Thread 1_ has two tasks in its queue, where _Task 1_ was added to the queue before _Task 2_.  Therefore, _Task 2_ will be executed first by _Thread 1_ and then it executes _Task 1_.   Any idle threads can take tasks from the other threads queues if available, that is, work-stealing.  A thread will always steal oldest tasks from some other thread's queue as shown in the following image.

![image](https://github.com/user-attachments/assets/09e19d30-9931-4bf2-8c51-b84b45bc6c6a)

As shown in the above image, _Thread 2_ stole the oldest task, _Task 1_, from _Thread 1_.  As a rule of thumb, threads will always attempt to steal from their neighbouring thread to minimise the contention that may be created during the work stealing.

The order in which tasks are executed and stolen is quite important.  Ideally, work-stealing does not happen a lot as this has a cost.  When a task is moved from one thread to another, the context related with this task needs to be moved from one thread's stack to another.  The threads may be (and the Fork/Join framework spread work across all CPUs) on another CPU.  Moving thread context from one CPU to another can be even slower.  Therefore, the Fork/Join Framework minimises this as described next.

A recursive algorithm starts with a large problem and applies a divide-and-conquer technique to break down the problem into smaller parts, until these are small enough to be solved directly.  The first task added to the queue is the largest task.  The first task will break the problem into a set of smaller tasks, which tasks are added to the queue as shown next.

![image](https://github.com/user-attachments/assets/df51edd2-3491-42c2-bf7c-4a42bea3059e)

_Task 1_ represents our problem, which is divided into two tasks,  _Task 2_ is small enough to solve as is, but _Task 3_ needs to be divided further.  Tasks _Task 4_ and _Task 5_ are small enough and these require no further splitting.  This represents a typical recursive algorithm which can be split into smaller parts and then aggregates the results when ready.  A practical example of such algorithm is calculating the size of a directory.  We know that the size of a directory is equal to the size of its files.

![image](https://github.com/user-attachments/assets/48a7cc24-f8a5-404d-a426-a3471c616c77)

Therefore the size of _Dir 1_ is equal to the size of _File 2_ plus the size of _Dir 3_.  Since _Dir 3_ is a directory, its size is equal to the size of its content.  In other words, the size of _Dir 3_ is equal to the size of _File 4_ plus the size of _File 5_.

Let us see how this is executed.  We start with one task, that is, to compute the size of directory as shown in the following image.

![image](https://github.com/user-attachments/assets/59592e01-964c-42a5-9cac-d7a3e250021e)

_Thread 1_ will take _Task 1_ which tasks forks two other subtasks.  These tasks are added to the queue of _Thread 1_ as shown in the next image.

![image](https://github.com/user-attachments/assets/dc9d0fbb-ca77-47df-83ee-7474366d2987)

_Task 1_ is waiting for the subtasks, _Task 2_ and _Task 3_ to finish, thus is pushed aside which frees _Thread 1_.  To use better terminology, _Task 1_ joins the subtasks _Task 2_ and _Task 3_.  _Thread 1_ starts executing _Task 3_ (the last task added to its queue), while _Thread 2_ steals _Task 2_.  

![image](https://github.com/user-attachments/assets/aecdcea8-d850-4d6e-8922-0ee505beed7a)

Note that _Thread 1_ has already started processing its second task, while _Thread 3_ is still idle.  As we will see later on, the threads will not perform the same number of work and the first thread will always produce more than the last thread.  _Task 3_ forks two more subtasks which are added to the queue of the thread that is executing it.  Therefore, two more tasks are added to _Thread 1_'s queue.  _Thread 2_, ready from _Task 2_, steals again another task as shown next.

![image](https://github.com/user-attachments/assets/ddb65e58-cd5d-457b-a61d-19c010717e46)

In the above example, we saw that _Thread 3_ never executed a task.  This is because we only have very little subtasks.  Once _Task 4_ and _Task 5_ are ready, their results are used to compute _Task 3_ and then _Task 1_.

As hinted before, the work is not evenly distributed among threads.  The following chart shows how the work is distributed amongst threads when calculating the size of a reasonably large directory.

![image](https://github.com/user-attachments/assets/2c59a2d7-f4d8-4481-b61f-7e6429f66390)

In the above example four threads were used.  As expected, Thread 1 performs almost 40% of the work while Thread 4 (the last thread) performs slightly more than 5% of the work.  This is another important principle to understand.  The Fork/Join Framework will not distribute work amongst threads evenly and will try to minimise the number of threads utilised.  The second threads will only take work from the first thread is this is not cooping.  As mentioned before, moving tasks between threads has a cost which the framework tries to minimise.

This section described on some detail how the Fork/Join Framework works and how threads steal work from other threads' queue.  In the following section we will see several practical example of the Fork/Join Framework and will analyse the obtained results.

## Calculate Directory Total Size

To demonstrate the use of the Fork/Join Framework, we will calculate the size of a directory, which problem can be solved recursively.  The size of file can determined by the method `length()` ([Java Doc](http://docs.oracle.com/javase/7/docs/api/java/io/File.html#length())).  The size of a directory is equal to the size of all its files.  

We will use several approaches to calculate the size of a directory, some of which will use the Fork/Join Framework, and we will analyse the obtained results in each case.

## Using a Single Thread (no-concurrency)

The first example will not make use of threads and simple defines the algorithm to the used.

```java
package com.javacreed.examples.concurrency.part1;

import java.io.File;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class DirSize {

  private static final Logger LOGGER = LoggerFactory.getLogger(DirSize.class);

  public static long sizeOf(final File file) {
    DirSize.LOGGER.debug("Computing size of: {}", file);

    long size = 0;

    /* Ignore files which are not files and dirs */
    if (file.isFile()) {
      size = file.length();
    } else {
      final File[] children = file.listFiles();
      if (children != null) {
        for (final File child : children) {
          size += DirSize.sizeOf(child);
        }
      }
    }

    return size;
  }
}
```

The class `DirSize ` has a single method called `sizeOf()`, which method takes a `File` ([Java Doc](http://docs.oracle.com/javase/7/docs/api/java/io/File.html)) instance as its argument.  If this instance is a file, then the method returns the file's length otherwise, if this is a directory, this method calls the method `sizeOf()` for each of the files within the directory and returns the total size.

The following example shows how to run this example, using the file path as defined by `FilePath.TEST_DIR` constant.

```java
package com.javacreed.examples.concurrency.part1;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.javacreed.examples.concurrency.utils.FilePath;

public class Example1 {

  private static final Logger LOGGER = LoggerFactory.getLogger(Example1.class);

  public static void main(final String[] args) {
    final long start = System.nanoTime();
    final long size = DirSize.sizeOf(FilePath.TEST_DIR);
    final long taken = System.nanoTime() - start;

    Example1.LOGGER.debug("Size of '{}': {} bytes (in {} nano)", FilePath.TEST_DIR, size, taken);
  }
}
```

The above example will compute the size of the directory and will print all visited files before printing the total size.  The following fragment only shows the last line which is the size of the _downloads_ directory under a test folder (`C:\Test`) together with the time taken to compute the size.

```
...
16:55:38.045 [main] INFO Example1.java:38 - Size of 'C:\Test\': 113463195117 bytes (in 4503253988 nano)
```

To disable the logs for each file visited, simple change the log level to `INFO` (in the `log4j.properties`) and the logs will only show the final results.

```
log4j.rootCategory=warn, R
log4j.logger.com.javacreed=info, stdout
```

Please note that the logging will only make things slower.  In fact if you run the example without logs (or the logs set to `INFO`), the size of the directory is computed much faster.

In order to obtain a more reliable result, we will run the same test several times and return the average time taken as shown next.

```java
package com.javacreed.examples.concurrency.part1;

import java.util.concurrent.TimeUnit;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.javacreed.examples.concurrency.utils.FilePath;
import com.javacreed.examples.concurrency.utils.Results;

public class Example2 {

  private static final Logger LOGGER = LoggerFactory.getLogger(Example1.class);

  public static void main(final String[] args) {
    final Results results = new Results();
    for (int i = 0; i < 5; i++) {
      results.startTime();
      final long size = DirSize.sizeOf(FilePath.TEST_DIR);
      final long taken = results.endTime();
      Example2.LOGGER.info("Size of '{}': {} bytes (in {} nano)", FilePath.TEST_DIR, size, taken);
    }

    final long takenInNano = results.getAverageTime();
    Example2.LOGGER.info("Average: {} nano ({} seconds)", takenInNano, TimeUnit.NANOSECONDS.toSeconds(takenInNano));
  }
}
```

The same test is executed five times and the average result is printed last as shown next.

```
16:58:00.496 [main] INFO Example2.java:42 - Size of 'C:\Test\': 113463195117 bytes (in 4266090211 nano)
16:58:04.728 [main] INFO Example2.java:42 - Size of 'C:\Test\': 113463195117 bytes (in 4228931534 nano)
16:58:08.947 [main] INFO Example2.java:42 - Size of 'C:\Test\': 113463195117 bytes (in 4224277634 nano)
16:58:13.205 [main] INFO Example2.java:42 - Size of 'C:\Test\': 113463195117 bytes (in 4253856753 nano)
16:58:17.439 [main] INFO Example2.java:42 - Size of 'C:\Test\': 113463195117 bytes (in 4235732903 nano)
16:58:17.439 [main] INFO Example2.java:46 - Average: 4241777807 nano (4 seconds)
```

### RecursiveTask

The Fork/Join Framework provides two types of tasks, the `RecursiveTask` ([Java Doc](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/RecursiveTask.html)) and the `RecursiveAction` ([Java Doc](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/RecursiveAction.html)).  In this section we will only talk about the `RecursiveTask`.  The `RecursiveAction` is discussed later on.

A `RecursiveTask` is a task that when executed it returns a value.  Therefore, such task returns the result of the computation.  In our case, the task returns the size of the file or directory it represents.  The class `DirSize` was modified to make use of `RecursiveTask` as shown next.

```java
package com.javacreed.examples.concurrency.part2;

import java.io.File;
import java.util.ArrayList;
import java.util.List;
import java.util.Objects;
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.RecursiveTask;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class DirSize {

  private static final Logger LOGGER = LoggerFactory.getLogger(DirSize.class);

  private static class SizeOfFileTask extends RecursiveTask<Long> {

    private static final long serialVersionUID = -196522408291343951L;

    private final File file;

    public SizeOfFileTask(final File file) {
      this.file = Objects.requireNonNull(file);
    }

    @Override
    protected Long compute() {
      DirSize.LOGGER.debug("Computing size of: {}", file);

      if (file.isFile()) {
        return file.length();
      }

      final List<SizeOfFileTask> tasks = new ArrayList<>();
      final File[] children = file.listFiles();
      if (children != null) {
        for (final File child : children) {
          final SizeOfFileTask task = new SizeOfFileTask(child);
          task.fork();
          tasks.add(task);
        }
      }

      long size = 0;
      for (final SizeOfFileTask task : tasks) {
        size += task.join();
      }

      return size;
    }
  }

  public static long sizeOf(final File file) {
    final ForkJoinPool pool = new ForkJoinPool();
    try {
      return pool.invoke(new SizeOfFileTask(file));
    } finally {
      pool.shutdown();
    }
  }

  private DirSize() {}

}
```

Let us break this class into smaller parts and describe each separately.

1. The class has a private constructor because it was not meant to be initialised.  Therefore there is no point to initialise it (creating objects of this type) and in order to prevent someone to initialising it, we made the constructor `private`.  All methods are static and these can be called against the class directly.

    ```java
      private DirSize() {}
    ```
1. The method `sizeOf()` does not compute the size of the file or directory.  Instead it creates an instance of the `ForkJoinPool` and starts the computational process.  It waits for the directory size to be computed and finally it shut down the pool before exiting.

    ```java
      public static long sizeOf(final File file) {
        final ForkJoinPool pool = new ForkJoinPool();
        try {
          return pool.invoke(new SizeOfFileTask(file));
        } finally {
          pool.shutdown();
        }
      }
    ```

    The threads created by the `ForkJoinPool` are daemon threads by default.  Some articles advice against the need of shutting this pool down since these threads will not prevent the VM from shutting down.  With that said, I recommend to shut down and dispose of any objects properly when these are no longer needed.  These daemon threads may be left idle for long time even when these are not required anymore.

1. The method `sizeOf()` creates an instance `SizeOfFileTask`, which class extends `RecursiveTask<Long>`.  Therefore the invoke method will return the objects/value returned by this task.

    ```java
          return pool.invoke(new SizeOfFileTask(file));
    ```

    Note that the above code will block until the size of the directory is computed.  In other words the above code will wait for the task (and all the subtasks) to finish working before continuing.

1. The class `SizeOfFileTask` is an inner class within the `DirSize` class.

    ```java
      private static class SizeOfFileTask extends RecursiveTask<Long> {

        private static final long serialVersionUID = -196522408291343951L;

        private final File file;

        public SizeOfFileTask(final File file) {
          this.file = Objects.requireNonNull(file);
        }

        @Override
        protected Long compute() {
          /* Removed for brevity */
        }
      }
    ```

    It takes the file (which can be a directory) for which size is to be computed as argument of its sole constructor, which file cannot be `null`.  The `compute()` method is responsible from computing the work of this task.  In this case the size of the file or directory, which method is discussed next.

1. The `compute()` method determines whether the file passed to its constructor is a file or directory and acts accordingly.

    ```java
        @Override
        protected Long compute() {
          DirSize.LOGGER.debug("Computing size of: {}", file);

          if (file.isFile()) {
            return file.length();
          }

          final List<SizeOfFileTask> tasks = new ArrayList<>();
          final File[] children = file.listFiles();
          if (children != null) {
            for (final File child : children) {
              final SizeOfFileTask task = new SizeOfFileTask(child);
              task.fork();
              tasks.add(task);
            }
          }

          long size = 0;
          for (final SizeOfFileTask task : tasks) {
            size += task.join();
          }

          return size;
        }
    ```

    If the file is a file, then the method simply returns its size as shown next.

    ```java
          if (file.isFile()) {
            return file.length();
          }
    ```

    Otherwise, if the file is a directory, it lists all its sub-files and creates a new instance of `SizeOfFileTask` for each of these sub-files.

    ```java
          final List<SizeOfFileTask> tasks = new ArrayList<>();
          final File[] children = file.listFiles();
          if (children != null) {
            for (final File child : children) {
              final SizeOfFileTask task = new SizeOfFileTask(child);
              task.fork();
              tasks.add(task);
            }
          }
    ```

    For each instance of the created `SizeOfFileTask`, the `fork()` method is called.  The `fork()` method causes the new instance of `SizeOfFileTask` to be added to this thread's queue.  All created instances of `SizeOfFileTask` are saved in a list, called `tasks`.  Finally, when all tasks are forked, we need to wait for them to finish summing up their values.

    ```java
          long size = 0;
          for (final SizeOfFileTask task : tasks) {
            size += task.join();
          }

          return size;
    ```

    This is done by the `join()` method.  This ` join()` will force this task to stop, step aside if needs be, and wait for the subtask to finish.  The value returned by all subtasks is added to the value of the variable `size` which is returned as the size of this directory.

The Fork/Join Framework is more complex when compared with the simpler version which does not use multithreading.  This is a fair point, but the simpler version is 4 times slower.  The Fork/Join example took on average a second to compute the size, while the non-threading version took 4 seconds on average as shown next.

```
16:59:19.557 [main] INFO Example3.java:42 - Size of 'C:\Test\': 113463195117 bytes (in 2218013380 nano)
16:59:21.506 [main] INFO Example3.java:42 - Size of 'C:\Test\': 113463195117 bytes (in 1939781438 nano)
16:59:23.505 [main] INFO Example3.java:42 - Size of 'C:\Test\': 113463195117 bytes (in 2004837684 nano)
16:59:25.363 [main] INFO Example3.java:42 - Size of 'C:\Test\': 113463195117 bytes (in 1856820890 nano)
16:59:27.149 [main] INFO Example3.java:42 - Size of 'C:\Test\': 113463195117 bytes (in 1782364124 nano)
16:59:27.149 [main] INFO Example3.java:46 - Average: 1960363503 nano (1 seconds)
```

In this section we saw how multithreading help us in improving the performance of our program.  In the next section we will see the how inappropriate use of multithreading can make things worse.

### ExecutorService

In the previous example we saw how concurrency improved the performance of our algorithm.  When misused, multithreading can provide poor results as we will see in this section.  We will try to solve this problem using a traditional executor service.

**Please note that the code shown in this section is broken and does not work.  It will hang forever and is only included for demonstration purpose.**

The class `DirSize` was modified to work with `ExecutorService` and `Callable` ([Java Doc](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Callable.html)).

```java
package com.javacreed.examples.concurrency.part3;

import java.io.File;
import java.util.ArrayList;
import java.util.List;
import java.util.Objects;
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

/**
 * This example is broken and suffers from deadlock and is only included for documentation purpose.
 *
 * @author Albert Attard
 */
public class DirSize {

  private static class SizeOfFileCallable implements Callable<Long> {

    private final File file;
    private final ExecutorService executor;

    public SizeOfFileCallable(final File file, final ExecutorService executor) {
      this.file = Objects.requireNonNull(file);
      this.executor = Objects.requireNonNull(executor);
    }

    @Override
    public Long call() throws Exception {
      DirSize.LOGGER.debug("Computing size of: {}", file);
      long size = 0;

      if (file.isFile()) {
        size = file.length();
      } else {
        final List<Future<Long>> futures = new ArrayList<>();
        for (final File child : file.listFiles()) {
          futures.add(executor.submit(new SizeOfFileCallable(child, executor)));
        }

        for (final Future<Long> future : futures) {
          size += future.get();
        }
      }

      return size;
    }
  }

  public static <T> long sizeOf(final File file) {
    final int threads = Runtime.getRuntime().availableProcessors();
    DirSize.LOGGER.debug("Creating executor with {} threads", threads);
    final ExecutorService executor = Executors.newFixedThreadPool(threads);
    try {
      return executor.submit(new SizeOfFileCallable(file, executor)).get();
    } catch (final Exception e) {
      throw new RuntimeException("Failed to calculate the dir size", e);
    } finally {
      executor.shutdown();
    }
  }

  private static final Logger LOGGER = LoggerFactory.getLogger(DirSize.class);

  private DirSize() {}

}
```

The idea is very much the same as before.  The inner class `SizeOfFileCallable` extends `Callable<Long>` and delegates the computation of its subtasks to the instance of `ExecutorService` passed to its constructor.  This is not required when dealing with the `RecursiveTask`, was the latter automatically adds its subclasses to the thread's queue for execution.

We will not go through this in more detail to keep this article focused on the Fork/Join Framework.  As mentioned already, this method blocks once all threads are occupied as shown next.

```
17:22:39.216 [main] DEBUG DirSize.java:78 - Creating executor with 4 threads
17:22:39.222 [pool-1-thread-1] DEBUG DirSize.java:56 - Computing size of: C:\Test\
17:22:39.223 [pool-1-thread-2] DEBUG DirSize.java:56 - Computing size of: C:\Test\Dir 1
17:22:39.223 [pool-1-thread-4] DEBUG DirSize.java:56 - Computing size of: C:\Test\Dir 2
17:22:39.223 [pool-1-thread-3] DEBUG DirSize.java:56 - Computing size of: C:\Test\Dir 3
```

This example is executed on a Core i5 computer, which has four available processors (as indicated by `Runtime.getRuntime().availableProcessors()` [Java Doc](http://docs.oracle.com/javase/6/docs/api/java/lang/Runtime.html#availableProcessors())).  Once all four threads are occupied, this approach will block forever as we saw in the bank branch example in the beginning of this article.  All threads are occupied and thus cannot be used to solve the other tasks.  One can suggest using more threads.  While that may seem to be a solution, the Fork/Join Framework solved the same problem using only four threads.  Furthermore, threads are not cheap and one should not simply spawn thousands of threads just because he or she chooses an inappropriate technique.

While the phrase multithreading is overused in the programming community, the choice of multithreading technique is important as some options simply do not work in certain scenarios as we saw above.

### RecursiveAction

The Fork/Join Framework supports two types of tasks.  The second type of task is the `RecursiveAction`.  These types of tasks are not meant to return anything.  These are ideal for cases where you want to do an action, such as delete a file, without returning anything.  In general you cannot delete an empty directory.  First you need to delete all its files first.  In this case the `RecursiveAction` can be used where each action either deletes the file, or first deletes all directory content and then deletes the directory itself.

Following is the final example we have in this article.  It shows the modified version of the `DirSize`, which makes use of the `SizeOfFileAction` inner class to compute the size of the directory.

```java
package com.javacreed.examples.concurrency.part4;

import java.io.File;
import java.util.Objects;
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.ForkJoinTask;
import java.util.concurrent.RecursiveAction;
import java.util.concurrent.atomic.AtomicLong;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class DirSize {

  private static class SizeOfFileAction extends RecursiveAction {

    private static final long serialVersionUID = -196522408291343951L;

    private final File file;
    private final AtomicLong sizeAccumulator;

    public SizeOfFileAction(final File file, final AtomicLong sizeAccumulator) {
      this.file = Objects.requireNonNull(file);
      this.sizeAccumulator = Objects.requireNonNull(sizeAccumulator);
    }

    @Override
    protected void compute() {
      DirSize.LOGGER.debug("Computing size of: {}", file);

      if (file.isFile()) {
        sizeAccumulator.addAndGet(file.length());
      } else {
        final File[] children = file.listFiles();
        if (children != null) {
          for (final File child : children) {
            ForkJoinTask.invokeAll(new SizeOfFileAction(child, sizeAccumulator));
          }
        }
      }
    }
  }

  public static long sizeOf(final File file) {
    final ForkJoinPool pool = new ForkJoinPool();
    try {
      final AtomicLong sizeAccumulator = new AtomicLong();
      pool.invoke(new SizeOfFileAction(file, sizeAccumulator));
      return sizeAccumulator.get();
    } finally {
      pool.shutdown();
    }
  }

  private static final Logger LOGGER = LoggerFactory.getLogger(DirSize.class);

  private DirSize() {}

}
```

This class is very similar to its predecessors.  The main difference lies in the way the final value (the size of the file or directory) is returned.  Remember that the `RecursiveAction` cannot return a value.  Instead, all tasks will share a common counter of type `AtomicLong` and these will increment this common counter instead of returning the size of the file.

Let us break this class into smaller parts and go through each part individually.  We will skip the parts that were already explained before so not to repeat ourselves.

1. The method `sizeOf()` makes use of the `ForkJoinPool` as before.  The common counter, named `sizeAccumulator`, is initialised in this method too and passed to the first task.  This instance will be shared with all subtasks and all will increment this value.

    ```java
      public static long sizeOf(final File file) {
        final ForkJoinPool pool = new ForkJoinPool();
        try {
          final AtomicLong sizeAccumulator = new AtomicLong();
          pool.invoke(new SizeOfFileAction(file, sizeAccumulator));
          return sizeAccumulator.get();
        } finally {
          pool.shutdown();
        }
      }
    ```

    Like before, this method will block until all subtasks are ready, after which returns the total size.

1. The inner class `SizeOfFileAction` extends `RecursiveAction` and its constructor takes two arguments.

    ```java
      private static class SizeOfFileAction extends RecursiveAction {

        private static final long serialVersionUID = -196522408291343951L;

        private final File file;
        private final AtomicLong sizeAccumulator;

        public SizeOfFileAction(final File file, final AtomicLong sizeAccumulator) {
          this.file = Objects.requireNonNull(file);
          this.sizeAccumulator = Objects.requireNonNull(sizeAccumulator);
        }

        @Override
        protected void compute() {
          /* Removed for brevity */
        }
      }
    ```

    The first argument is the file (or directory) which size will be computed.  The second argument is the shared counter.

1. The compute method is slightly simpler here as it does not have to wait for the subtasks.  If the given file is a file, then it increments the common counter (referred to as `sizeAccumulator`).  Otherwise, if this file is a directory, it forks the new instances of `SizeOfFileAction` for each child file.

    ```java
        protected void compute() {
          DirSize.LOGGER.debug("Computing size of: {}", file);

          if (file.isFile()) {
            sizeAccumulator.addAndGet(file.length());
          } else {
            final File[] children = file.listFiles();
            if (children != null) {
              for (final File child : children) {
                ForkJoinTask.invokeAll(new SizeOfFileAction(child, sizeAccumulator));
              }
            }
          }
        }
    ```

    In this case the method `invokeAll()` ([Java Doc](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ForkJoinTask.html#invokeAll(java.util.concurrent.ForkJoinTask...))) is used to fork the tasks.

This approach takes approximately 11 seconds to complete making it the slowest of all three as shown next.

```
19:04:39.925 [main] INFO Example5.java:40 - Size of 'C:\Test': 113463195117 bytes (in 11445506093 nano)
19:04:51.433 [main] INFO Example5.java:40 - Size of 'C:\Test': 113463195117 bytes (in 11504270600 nano)
19:05:02.876 [main] INFO Example5.java:40 - Size of 'C:\Test': 113463195117 bytes (in 11442215513 nano)
19:05:15.661 [main] INFO Example5.java:40 - Size of 'C:\Test': 113463195117 bytes (in 12784006599 nano)
19:05:27.089 [main] INFO Example5.java:40 - Size of 'C:\Test': 113463195117 bytes (in 11428115064 nano)
19:05:27.226 [main] INFO Example5.java:44 - Average: 11720822773 nano (11 seconds)
```

This may be a surprise to many.  How is this possible, when multiple threads were used?  This is a common misconception.  Multithreading does not guarantee better performance.  In this case we have a design flaw which ideally we avoid.  The common counter named `sizeAccumulator` is shared between all threads and thus causes contention between threads.  This actually defeats the purpose of the divide and conquer technique as a bottleneck is created.

## Conclusion

This article provided a detailed explanation of the Fork/Join Framework and how this can be used.  It provided a practical example and compared several approaches.  The Fork/Join Framework is ideal for recursive algorithms but it does not distribute the load amongst the threads evenly.  The tasks and subtask should not block on anything else but join and should delegate work using fork.  Avoid any blocking IO operations within tasks and minimise the mutable share state especially modifying the variable as much as possible as this has a negative effect on the overall performance.


