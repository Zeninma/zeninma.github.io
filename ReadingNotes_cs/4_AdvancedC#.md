# Advanced C#

## Delegates

## Lambda Function and Captured Variable
* A lambda expression can reference the local variables and parameters of the method in which it's defined.
```cs
int factor = 2;
Func<int, int> multiplier = (int n) => n * factor;
factor = 10;
int x = multiplier(3); // result is 30 rather than 6
```
* The outer variables referenced by a lambda expression are called captured variables. A lambda expression that captures variables is called a closure.
* The captured variables are evaluated when the delegate is **actually invoked**, rather than when the variables were captured.
* The Lambda expression can also update the variables and the captured variable's life time will be extened to that of the delegate.
* Notice the capture of iteration variables!
```cs
Action[] actions = new Action[];
for(int i = 0; i < 3; ++i)
{
    actions[i] = () => Console.Write(i);
}
foreach (Action a in actions) a(); // This prints 333 instead of 012
```

## Try Statements and Exceptions
* classes that implement _System.IDisposable_ has the method _Dispose_ which to help clean up the unmanaged resources encapsulated by the object.
```cs
// The using statemetnt is equivalent to with in Python, which provides an elegant syntax for calling Dispose on an IDisposable object within a finally block
using (StreamReader reader = File.OpenText("file.txt"))
{
    ...
}
```
* Common Exception Types
* TryDoX and DoX - provide user with two options. i.e. Integer.Parse and Integer.TryParse
* The ATOMICITY PATTERN - In the finally block, reverse the half done mutation to avoid messy middle state.

## Enumeration and Iterators
* _enumerator_ is read-only, forward-only cursor over a sequence of values.
* An enumerable object either implements IEnumerable of IEnumerable<T> and has a method named GetEEnumerator that returns an enumerator.
* An iterrator can have multiple yield return statements.
```cs
static IEnumerable<string> ()
{
    yield return "One";
    yield return "Two";
}
```

## Nullable Type
* Unboxing of nullable type with _as_ operator
```cs
object o = "string";
int? x = o as int?;
x.HasValue; // is false as the cast fails
```
* The most common scenarios for nullable types is to represent unknown values. i.e when a class is mapped to a table with nullable columns.

## Operator Overloading

## Extension Method
* Allow an existing type to be exteded with new methods without altering the definition of the original type. It is a static method of a static class.
```cs
public static class StringHelper
{
    public static bool IsCapitalized (this string s)
    {
        // Do some manipulation around the string s.
    }
}

// Then type string will be extended with the IsCapitalized method
"abs".IsCapitalized();
// This method will be replaced with the following statement during the compile time.
StringHelper.IsCapitalized("abs");
```
* Interface can be extended as well. The extended method then can be applied to all the classes that implement the interface.

## Dynamic Binding
* By using the type dynamic, we are telling the compiler to relax that we are sure the method exists for the given type.
```cs
dynamic duck = GetSomeInstance();
duck.Quack(); // This statement would otherwise make the compiler complain.
```
Skip the rest of this section.

## Attributes