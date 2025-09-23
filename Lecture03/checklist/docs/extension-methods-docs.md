# Extension Methods - Documentation Guide

This document provides comprehensive documentation links and learning resources for understanding and implementing extension methods in C#.

## Conceptual Understanding

Start here to understand the fundamental concepts of extension methods:

- **[Extension Methods](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)** - Core concepts and syntax
  
  Complete introduction to extension methods, covering syntax, static class requirements, and basic usage patterns.

- **[Extension Methods Overview](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/how-to-implement-and-call-a-custom-extension-method)** - Implementation fundamentals
  
  Step-by-step guide on how to implement and call custom extension methods with practical examples.

## Implementation Guides

Practical documentation for creating and using extension methods:

- **[Extension Method Best Practices](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/extension-methods)** - Design guidelines and patterns
  
  Official Microsoft guidelines for extension method design, including naming conventions and when to use them.

- **[LINQ Extension Methods](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/standard-query-operators-overview)** - Real-world examples
  
  Understanding LINQ as a practical example of extension methods, showing how they extend IEnumerable<T>.

## Best Practices & Advanced Patterns

Design guidelines and sophisticated techniques:

- **[Extension Method Design Patterns](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods#general-usage-guidelines)** - Usage guidelines
  
  Detailed guidelines on when and how to use extension methods effectively, including performance considerations.

- **[Generic Extension Methods](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/generics/generic-methods)** - Advanced scenarios
  
  Creating generic extension methods that work with multiple types, including constraints and type inference.

## Quick Reference

### When to Use Extension Methods:

- **Extending built-in types**: Add functionality to `string`, `int`, `DateTime`, etc.
- **Third-party types**: Extend types you don't own (libraries, frameworks)
- **Collection operations**: Create domain-specific operations on `IEnumerable<T>`
- **Fluent interfaces**: Chain method calls for better readability
- **Utility functions**: Group related functionality around specific types

### When NOT to Use Extension Methods:

- **Your own types**: If you own the class, add the method directly
- **Simple static utilities**: Use static methods in utility classes instead
- **Performance-critical code**: Extension methods have slight overhead
- **Complex logic**: Keep extension methods simple and focused

### Design Guidelines:

- **Static class and method**: Must be in static class with static methods
- **First parameter with `this`**: `public static void MyExtension(this MyType obj)`
- **Meaningful names**: Use clear, descriptive method names
- **Simple operations**: Keep extension methods focused and simple
- **Null safety**: Always check for null parameters
- **Documentation**: Provide XML documentation for public extension methods

### Common Patterns:

```csharp
// String extensions
public static bool IsValidEmail(this string email)

// Collection extensions  
public static IEnumerable<T> WhereNotNull<T>(this IEnumerable<T?> source)

// Type conversion extensions
public static int ToInt32OrDefault(this string value, int defaultValue = 0)

// Fluent interface extensions
public static TBuilder With<TBuilder, TValue>(this TBuilder builder, Action<TValue> configure)
```

### Key Principle:

Using extension method on your own class usually is not a meaningful usage (you can just add method directly to your class, you own the code). Focus on extending built-in types, third-party types, or creating domain-specific collection operations.

### Performance Considerations:

- Extension methods have slight call overhead compared to instance methods
- Compiler resolves extension methods after instance methods
- Generic extension methods may impact JIT optimization
- Consider static utility methods for frequently called operations

---

*This documentation guide complements the practical examples in [extension-methods-examples.ipynb](../examples/extension-methods-examples.ipynb)*