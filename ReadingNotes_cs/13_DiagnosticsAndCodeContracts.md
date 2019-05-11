## Diagnostics and COde Contracts
### preprocessor directives:
```cs
#if
#else
#endif
#elif

#define // only applies to a particular file
#undefine // only applies to a particular file
```
To define a symbol assembly-wide, specify _/define_ switch when compiling.

### Conditional Attribute
```cs
[Conditional ("LOGMODE")]
static void Log(string msg)
{
    // ...
}
```
Calls to the method will be entirely ignored during the compilation if _LONGMODE_ is not defined.