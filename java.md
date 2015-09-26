java hints

JDK The software for programmers who want to write Java programs;
JRE The software for consumers who want to run Java programs;

A class is immutable if none of its methods ever mutate its objects. For exam-
ple, the String class is immutable.

the static belongs to the class, not to any individual object.

Static Constants static final , Math.PI , System.out.

static methods can't not access non-static fields in their class but non static method can access static field in their class.

Overloading occurs if several methods have the
same name (in this case, the GregorianCalendar constructor method) but different parame-
ters. The compiler must sort out which method to call. It picks the correct method by
matching the parameter types in the headers of the various methods with the types of
the values used in the specific method call. A compile-time error occurs if the compiler
cannot match the parameters or if more than one match is possible. (This process is
called overloading resolution.)
The return type is not part of the method signature. That is, you cannot have two methods
with the same names and parameter types but different return types.

The initialization block runs first, and
then the body of the constructor is executed.

**construction process. Here is what happens in detail when a con-structor is called :**
	1. All data fields are initialized to their default value ( 0 , false , or null ).
	2. All field initializers and initialization blocks are executed, in the order in which they
	occur in the class declaration.
	3. If the first line of the constructor calls a second constructor, then the body of the sec-
	ond constructor is executed.
	4. The body of the constructor is executed.

Static initialization occurs when the class is first loaded , and has default values 0 , false , null.


some objects utilize a resource other than memory, such as a file or a handle
to another object that uses system resources. In this case, it is important that the resource
be reclaimed and recycled when it is no longer needed.
The finalize method will be called before the
garbage collector sweeps away the object. In practice, do not rely on the finalize method for
recycling any resources that are in short supply—you simply cannot know when this
method will be called.

The fact that an object variable can refer to multiple actual types is called polymorphism.

```
Manager boss = new Manager(. . .);
Employee[] staff = new Employee[3];
staff[0] = boss;
boss.setBonus(5000); // OK
staff[0].setBonus(5000); // ERROR
Manager m = staff[i]; // ERROR
Manager[] managers = new Manager[10];
Employee[] staff = managers; // OK
staff[0] = new Employee("Harry Hacker", ...); why staff[0].setBonus(5000); will make error
```
an interface is not a class but a set of requirements for classes that want to conform to the interface.


Notice that all exceptions descend from Throwable , but the hierarchy immediately splits
into two branches: Error and Exception .

The Error hierarchy describes internal errors and resource exhaustion inside the Java
runtime system. You should not throw an object of this type.

The Exception hierarchy also splits into two branches: exceptions that derive from RuntimeException and those that do not.

A RuntimeException happens because you made a programming error. Any other exception occurs because a bad thing, such as an I/O error,happened to your otherwise good program.

**RuntimeException** is your responability to avoid them by checking say for array size before accessing it .i.e. A bad cast , out-of-bounds array access ArrayIndexOutOfBoundsException , null pointer access NullPointerException.

The Java Language Specification calls any exception that derives from the class Error or the class RuntimeException an **unchecked exception** , All other exceptions are called **checked exception** The compiler checks that you provide exception handlers for all checked exceptions.

a method must declare all the checked exceptions that it might throw.Unchecked exceptions are either beyond your control ( Error ) or result from conditions that you should not have allowed in the first place ( RuntimeException )

finally clause : the code throws an exception that is caught in a catch clause, in our case, an IOException . For this, the program executes all code in the try block, up to the point at which the exception was thrown. The remaining code in the try block is skipped. The program then executes the code in the matching catch clause, then the code in the finally clause.

**Threads**

**java.lang.Thread**

 * Thread(Runnable target) __constructs a new thread that calls the run() method of the specified target.__
 * void start() __starts this thread, causing the run() method to be called. This method will return immediately. The new thread runs concurrently.__
 * void run() __calls the run method of the associated Runnable.__ and will start in same current thread.
 * void interrupt() __sends an interrupt request to a thread. The interrupted status of the thread is set to true . If the thread is currently blocked by a call to sleep , then an InterruptedException is thrown.__
 * static boolean interrupted() __tests whether the current thread (that is, the thread that is executing this instruction) has been interrupted. Note that this is a static method. The call has a side effect—it resets the interrupted status of the current thread to false.__
 * boolean isInterrupted() __tests whether a thread has been interrupted. Unlike the static interrupted method, this call does not change the interrupted status of the thread.__
 * static Thread currentThread() __returns the Thread object representing the currently executing thread.__
 * void setDaemon(boolean isDaemon) __marks this thread as a daemon thread or a user thread. This method must be called before the thread is started.__

**java.lang.Runnable**
 * void run() __must be overriden and supplied with instructions for the task that you want to
have executed__

```Java
Ball b = new Ball();
panel.add(b);
Runnable r = new BallRunnable(b, panel);
Thread t = new Thread(r);
t.start();
```

There is a way to force a thread to terminate. However, the interrupt method can be used to request termination of a thread.

When the interrupt method is called on a thread that blocks on a call such as sleep or wait , the blocking call is terminated by an **InterruptedException**.

```Java
public void run() {
        try {
            while (!Thread.currentThread().isInterrupted() && more work to do) {
                //do more work
            }
        } catch (InterruptedException e) {
                // thread was interrupted during sleep or wait
								//quite commonly, a thread will simply want to interpret an interruption as a request for termination
								Thread().currentThread().interrupt();
        } finally {
               // cleanup,if required
        }
        // exiting the run method terminates the thread
    }
```

**Threads can be in one of six states:**
 * New
 * Runnable
 * Blocked
 * Waiting
 * Timed waiting
 * Terminated
__To determine the current state of a thread, simply call the__ **getState method**.

**A daemon thread** is simply a thread that has no other role in life than to serve others. Examples are timer threads that send regular “timer ticks” to other threads or threads that clean up stale cache entries

__The basic outline for protecting a code block with a ReentrantLock is:__
```Java
public class Bank
{
    public void transfer(int from, int to, int amount)
    {
        bankLock.lock(); // a ReentrantLock object
        try
        {
						// critical section
						//This construct guarantees that only one thread at a time can enter the critical section.
					  System.out.print(Thread.currentThread());
            accounts[from] -= amount;
            System.out.printf(" %10.2f from %d to %d", amount, from, to);
            accounts[to] += amount;
            System.out.printf(" Total Balance: %10.2f%n", getTotalBalance());
        }
        finally
        {
            bankLock.unlock(); // make sure the lock is unlocked even if an exception is thrown
        }
    }
    private Lock bankLock = new ReentrantLock(); // ReentrantLock implements the Lock interface
}
```

If two threads try to access the same Bank object, then the lock serves to serialize the access. However, if two threads **access different Bank objects**, then each thread acquires a different lock and **neither thread is blocked**. This is as it should be, because the threads cannot interfere with another when they manipulate different Bank instances.

The lock is called reentrant because a **thread can repeatedly acquire a lock that it already owns**. The lock keeps a **hold count** that keeps track of the nested calls to the lock method. The thread has to **call unlock for every call to lock in order to relinquish the lock**. Because of this feature, code that is protected by a lock can call another method that uses the same locks. __For example, the transfer method calls the getTotalBalance method, which also locks the bankLock object, which now has a hold count of 2. When the getTotalBalance method exits, the hold count is back to 1. When the transfer method exits, the hold count is 0, and the thread relinquishes the lock.__

**Lock Conditions:**
	__//TODO__

**synchronized:**
If a method is declared with the synchronized keyword, then the object’s lock protects the entire
method. That is, to call the method, a thread must acquire the **intrinsic object lock.**
```Java
public synchronized void method()
{
	// method body
}
```
is the equivalent of
```Java
public void method()
{
	this.intrinsicLock.lock();
	try{
			//method body
	}finally {
		this.intrinsicLock.unlock();
	}
}
```

The intrinsic object lock has a single associated condition. The wait method adds a thread to the wait set, and the notifyAll / notify methods unblock waiting threads. In other words, calling __wait or notifyAll__ is the equivalent of __intrinsicCondition.await(); intrinsicCondition.signalAll();__

**java.lang.Object**
 * void notifyAll() __unblocks the threads that called wait on this object. This method can only be called from within a synchronized method or block. The method throws an IllegalMonitorStateException if the current thread is not the owner of the object’s lock.__
 * void notify() __unblocks one randomly selected thread among the threads that called wait on this object. This method can only be called from within a synchronized method or block. The method throws an IllegalMonitorStateException if the current thread is not the owner of the object’s lock.__
 * void wait() __causes a thread to wait until it is notified. This method can only be called from within a synchronized method. It throws an IllegalMonitorStateException if the current thread is not the owner of the object’s lock.__

**A Callable** is similar to a Runnable , but it returns a value. The Callable interface is a parameterized type, with a single method call .
```Java
	public interface Callable<V>
	{
		V call() throws Exception;
	}
```
The type parameter is the type of the returned value. For example, a Callable<Integer> represents an asynchronous computation that eventually returns an Integer object.

**A Future** holds the result of an asynchronous computation. You can start a computation,give someone the Future object, and forget about it. The owner of the Future object can obtain the result when it is ready.
```Java
	public interface Future<V>
	{
		V get() throws . . .;
		V get(long timeout, TimeUnit unit) throws . . .;
		void cancel(boolean mayInterrupt);
		boolean isCancelled();
		boolean isDone();
	}
```
**The FutureTask** wrapper is a convenient mechanism for turning a Callable into both a Future and a Runnable —it implements both interfaces. For example:
```Java
Callable<Integer> myComputation = . . .;
FutureTask<Integer> task = new FutureTask<Integer>(myComputation);
Thread t = new Thread(task); // it's a Runnabl
t.start();
. . .
Integer result = task.get(); // it's a Future
```
