# Threading
* Captured Variables
* Foreground and Background Threads
* Thread Priority:
    * Thread Priority is still dominated by the parent process's priority
    * For parent process's priority, avoid _RealTime_ priority
* Exception handling
* Thread Pooling
    * Creation oa a thread consumes around 1MB memory and takes a few hundred of miliseconds
* Different ways to access ThreadPool:
```cs
// Use nongeneric Task
Task task = Task.Factory.StartNew(targetMethod);
ResultType result = task.Result;

// Use QueueUserWorkitem
ThreadPool.QueueUserWorkItem(targetMethod);
ThreadPool.QueueUserWorkItem(targetMethod, param);

static void targetMethod(object param)
{
    // Cannot return a value
}

// Use Asynchronous Delegate
Func<ParamType, ReturnType> method = targetFunc;
IAsyncResult expectedResult = method.BeginInvoke(method, object[] args);
    // ...Do some parallel work
    // Get the result back
ReturnType result = method.EndInvoke(expectedResult);

// With the help from a Call Back method
Func<string,int> method = Work;
method.BeginInvoke("test", Done, method);

static int Work(string) 
{
    // Doe some work
}

static void Done(IAsyncResult res)
{
    var target = (Func<string, int>) res.AsyncState;
    int result = target.EndInvoke(res);
    // Do some work with the result
}
```

## Optimizting the ThreadPool

## Synchronization
* Lock takes less than a hundred of nano-second
* Mutex is in the micro-second range - however, Mutex can be _machine wide_ as well as _application wide_
    * Mutex can be made _machine wide_ by sharing the same _global_ naming across all the applications on a machine.
* Semaphore (SemaphoreSlime) incurs about 1 ms or 250ns in calling WaitOne or Release

## Threadsafe:
* In the .NetFramework: _static members are thread-safe; instance members are not_. Thhis is also a codsing convention that should be followed.
* "Enumerator" V.S "Enumerable" - Tow threads can simultaneously enumberate over a collection because each gets a separate enumerator object.
* Use an object with multiple immutable fields to reduce the number of locks that are needed to be used.
```cs
public class Status
{
    public readonly string a;
    public readonly int b,c,d; // multiple fields
    public Status(string a, int b, int c, int d)
    {
        this.a = a;
        this.b = b;
        this.c = c;
        this.d = d
    }
}

public class Program
{
    readonly object _statusLocker = new object();
    Status _status;
    string statusStr;
    int b,c,d;

    public static void main()
    {
        // ... Do something, need to update status
        // In this way we can change 5 fields by just acquires one brief lock!!
        lock(_statusLocker) _status = new Status(params);
        this.statusStr = _status.a;
        this.b = _status.b;
        this.c = _status.c;
        // ...
    }
}
```

### Memory Barrier
```cs
Thread.MemoryBarrier(); // Takes few tens of nanoseconds
```
* Single instruction Compare and Swap in C# is done by:
```cs
// set obj to targetVal if and only if obj == conditionVal before the assignment
Interlocked.CompareExchange(ref obj, targetVal, conditionVal);
```

## Signaling with Event Wait Handles -- Conditional Variable?
* Interestingly, When an Set is called upon an EventWaitHandle, the EventWaitHandle object will keep open until a WaitOne is called upon it. At this moment, the Thread that calls WaitOne will be unblocked, and EventWaitHandle will block all the following Thread. This is different that a CPP conditional variable, as there is no concern about a Thread misses a signal.

## CountdownEvent
* Enabling waiting on more than one thread.
```cs
static CountdownEvent _countdown = new CountdownEvent(3); // parameter specifies the number of signals it expects to receive
new Thread(Func).Start("1");
new Thread(Func).Start("2");
new Thread(Func).Start("3");
_countdown.Wait(); // it will keeps blocking the current thread, until its countdown value reaches zero

static void Func(object name)
{
    // Do something
    _countdown.Signal();
}
```

## Not know
* What is LINQ?
* What is a delegate in C#?