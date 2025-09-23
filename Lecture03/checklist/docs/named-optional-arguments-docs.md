# Named and Optional Arguments - Documentation Guide

This document provides comprehensive documentation links and learning resources for understanding and implementing named and optional arguments in C#.

## Conceptual Understanding

Start here to understand the fundamental concepts:

- **[Named and optional arguments](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments)** - Core concepts and syntax
  
  Complete introduction to named and optional arguments, covering syntax, calling conventions, and basic usage patterns.

- **[Method Parameters](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/passing-parameters)** - Parameter passing fundamentals
  
  Foundational concepts of parameter passing in C#, including value types, reference types, and parameter modifiers.

## Implementation Guides

Practical documentation for using named and optional arguments:

- **[Optional Parameters Best Practices](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/how-to-use-named-and-optional-arguments-in-office-programming)** - Practical usage examples
  
  Real-world examples of using named and optional arguments, particularly in API design and Office programming scenarios.

- **[Method Overloading vs Optional Parameters](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/member-overloading)** - When to choose which approach
  
  Critical guidance on when to use optional parameters vs method overloading, including performance and API design considerations.

## Best Practices & Advanced Patterns

Design guidelines and advanced techniques:

- **[Parameter Design Guidelines](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/parameter-design)** - Microsoft's parameter design recommendations
  
  Official Microsoft guidelines for parameter design, covering naming, ordering, types, and optional parameter patterns.

- **[Caller Information Attributes](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/attributes/caller-information)** - Advanced optional parameter patterns
  
  Advanced techniques using CallerMemberName, CallerFilePath, and CallerLineNumber attributes for debugging and logging scenarios.

## Quick Reference

### When to Use Named Arguments:

- **Multiple parameters of same type**: `CreateUser(firstName: "John", lastName: "Doe")`
- **Non-obvious parameter meaning**: `DrawRectangle(x: 10, y: 20, width: 100, height: 50)`
- **Skip optional parameters**: `Method(required: value, optional3: value3)` (skipping optional1 and optional2)
- **Improve code readability**: Make parameter purpose clear at call site

### When to Use Optional Parameters:

- **Sensible defaults**: `public void Log(string message, LogLevel level = LogLevel.Info)`
- **API flexibility**: Allow simpler method calls while maintaining full functionality
- **Backward compatibility**: Add new parameters without breaking existing calls
- **Configuration methods**: `ConfigureService(timeout: 30, retries: 3, enableLogging: false)`

### Design Considerations:

- **Optional parameters must come after required parameters**
- **Default values must be compile-time constants**
- **Consider method overloading for complex scenarios**
- **Use named arguments for clarity, not just convenience**
- **Avoid too many optional parameters (consider configuration objects)**

### Key Principle:

Use named arguments for clarity when method has multiple parameters of same type or when parameter meaning isn't obvious. Use optional parameters for sensible defaults and API flexibility, not just to avoid overloads.

---

*This documentation guide complements the practical examples in [named-optional-arguments-examples.ipynb](../examples/named-optional-arguments-examples.ipynb)*