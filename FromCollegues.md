## Exception Handling in C#
* In C# the exception handling can be subtle as it is very easy to lose the callstack to the original exception. For example:
```cs
// Usually, when trying to propagate the Exception from child thread to the main threa, people need to rethrow the Exception.
catch(Exception e)
{
 throw new MyException(e, "my new exception message");
}

catch(Exception e)
{
  // Do the logging or some other stuff then rethrow the Exception
  throw; // or throw e;
}
```
* All of the catch clauses above will cause the lose of the callstack that leads to the original Exception and only left with the stack after the catch clause. This can make the Debugging process very difficult.
* The solution to this is to use *Exception Filter*:
```cs
catch(Exception e) when (LogException(e))
{
  // Log the exception
}

private bool LogException(Exception e)
{
  // Determine whether the Exception can be properly handeled. If so, just return true and log it in the catch clause.
  // If do not know how to handle the exception, propagate it back.
}
```
* This piece of advice is given by Nikko.
