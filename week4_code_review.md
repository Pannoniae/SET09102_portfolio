# Code review

## The code
```csharp
using System;

namespace MyApplication
{
  class Program
  {
    static bool check(Int i) 
    {
        if (i % 2 == 0)
        {
            return true;
        }
        return false;
    }

    static void Main(string[] args)
    {
        //... Code not shown - ignore this line
    }  
  }
}
```

## Identifying the problems

### C# Guidelines and formatting
 - The indentation is inconsistent, it mixes two and four spaces.
 - The method `check` is not compliant with the guidelines about naming methods.

### Syntactical problems
 - The `Int` type does not exist in C# so the example would not compile.

### Best practices
 - The method name `check` is not informative,
it does not tell anything about either the semantic contents of the method, or about the context in which it is used.
 - The conditional return could be better expressed by simply returning the statement; the code is verbose for no reason.


## The improved version
```csharp
namespace MyApplication;
class Program
{
    static bool IsEven(int i)
    {
        return (i % 2 == 0);
    }

    static void Main(string[] args)
    {
        //... Code not shown - ignore this line
    }
}

```

## Explaining the improvements

 - `using System;` is not needed, the code does not use anything from the System namespace, so it was removed because it was dead code.
 - The code was converted to use file-scoped namespaces - it improves readability by reducing the global indentation level by one.
 - The `check` method was renamed `IsEven` - this is important both being conformant to C# style guidelines,
and to make it clear what the method does, according to clean code principles.
 - The method was simplified to return the expression directly, instead of the pattern `if ($expression) { return true; } else { return false; }`
 - The parameter type was changed from `Int` to `int`, since the former is syntactically incorrect.

My solution is better because it uses  less lines of code and it is much more readable.
It is familiar to most programmers and it follows established style.