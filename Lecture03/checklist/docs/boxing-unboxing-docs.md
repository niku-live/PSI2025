# Boxing and Unboxing - Documentation Guide

This document provides comprehensive documentation links and learning resources for understanding and managing boxing/unboxing operations in C#.

## Conceptual Understanding

Start here to understand the fundamental concepts of boxing and unboxing:

- **[Boxing and Unboxing](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/types/boxing-and-unboxing)** - Core concepts
  
  Complete introduction to boxing and unboxing, including what happens under the hood and when these operations occur.

- **[Value Types and Reference Types](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-types)** - Type system fundamentals
  
  Understanding the difference between value types and reference types, which is essential for understanding boxing.

## Implementation Guides

Practical documentation for managing boxing/unboxing:

- **[Generic Collections](https://learn.microsoft.com/en-us/dotnet/standard/collections/commonly-used-collection-types)** - Avoiding boxing
  
  How to use generic collections to avoid unnecessary boxing when working with value types.

- **[Pattern Matching](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/functional/pattern-matching)** - Safe unboxing
  
  Modern techniques for safe type checking and unboxing using pattern matching instead of traditional casting.

## Best Practices & Advanced Patterns

Design guidelines and optimization techniques:

- **[Performance Implications](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/performance)** - Cost awareness
  
  Understanding the performance cost of boxing operations and how to minimize them in performance-critical code.

- **[Interface Implementation](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/interfaces/)** - When boxing is necessary
  
  Understanding when boxing is unavoidable (interface calls on value types) and how to handle it efficiently.

- **[Nullable Value Types](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/nullable-value-types)** - Boxing behavior
  
  How nullable value types interact with boxing and when they cause unexpected allocations.

## Quick Reference

### What is Boxing/Unboxing?

**Boxing**: Converting a value type to a reference type (object or interface)
- Value is copied from stack to heap
- New object is allocated on heap
- Performance cost: allocation + copying

**Unboxing**: Converting a reference type back to a value type
- Type compatibility check at runtime
- Value is copied from heap to stack
- Can throw InvalidCastException if types don't match

### Common Boxing Scenarios:

#### **Automatic Boxing (Often Unintentional)**
```csharp
// Boxing examples
object obj = 42;                    // int → object (boxing)
ArrayList list = new ArrayList();   
list.Add(123);                      // int → object (boxing)
string result = 10.ToString();      // Boxing occurs for method call
Console.WriteLine(42);              // Boxing for object parameter
```

#### **Interface Boxing**
```csharp
int number = 42;
IComparable comparable = number;     // Boxing to interface
IFormattable formattable = number;  // Boxing to interface
```

#### **Collection Boxing (Legacy)**
```csharp
ArrayList arrayList = new ArrayList();
arrayList.Add(1);                   // Boxing
arrayList.Add(2);                   // Boxing
int value = (int)arrayList[0];      // Unboxing with cast
```

### Avoiding Boxing:

#### **Use Generic Collections**
```csharp
// Avoid boxing
List<int> list = new List<int>();
list.Add(123);                      // No boxing
int value = list[0];                // No unboxing

// Instead of
ArrayList arrayList = new ArrayList();
arrayList.Add(123);                 // Boxing occurs
```

#### **Use Generic Interfaces**
```csharp
// Prefer generic interfaces
IComparable<int> comparable = 42;   // No boxing

// Instead of
IComparable comparable = 42;        // Boxing occurs
```

#### **Avoid object Parameters**
```csharp
// Use generic methods
public static void Process<T>(T value) { }
Process(42);                        // No boxing

// Instead of
public static void Process(object obj) { }
Process(42);                        // Boxing occurs
```

### Safe Unboxing Techniques:

#### **Traditional Casting (Risky)**
```csharp
object obj = 42;
int value = (int)obj;               // Can throw InvalidCastException
```

#### **Pattern Matching (Recommended)**
```csharp
object obj = 42;
if (obj is int value)
{
    // Safe unboxing, value is available here
    Console.WriteLine(value);
}

// Or with switch expression
string result = obj switch
{
    int i => $"Integer: {i}",
    string s => $"String: {s}",
    _ => "Unknown type"
};
```

#### **as Operator with Nullable**
```csharp
object obj = 42;
int? value = obj as int?;           // Returns null if not int
if (value.HasValue)
{
    Console.WriteLine(value.Value);
}
```

### Performance Considerations:

#### **Boxing Costs**
- **Memory allocation**: Each boxing creates new heap object
- **Garbage collection**: More objects = more GC pressure
- **Copy overhead**: Value must be copied to/from heap
- **Type checking**: Runtime type validation during unboxing

#### **When Boxing is Unavoidable**
```csharp
// Interface method calls on value types
int number = 42;
string result = number.ToString();  // Boxing for interface call

// Heterogeneous collections (use object or variant types)
object[] mixed = { 1, "text", 3.14 }; // Boxing for value types

// Legacy APIs that require object parameters
Console.WriteLine(42);              // Boxing for object parameter
```

#### **Optimization Strategies**
```csharp
// Cache boxed values for repeated use
private static readonly object BoxedTrue = true;
private static readonly object BoxedFalse = false;

public static object GetBoxedBool(bool value)
{
    return value ? BoxedTrue : BoxedFalse;
}

// Use value type containers
public struct Container<T> where T : struct
{
    public T Value { get; set; }
}
```

### Modern C# Features:

#### **Span<T> and Memory<T>**
```csharp
// Avoid boxing when working with value type collections
Span<int> numbers = stackalloc int[] { 1, 2, 3, 4, 5 };
// No boxing, stack-allocated
```

#### **ref readonly Parameters**
```csharp
public void Process(in DateTime date)  // Pass by reference, no boxing
{
    // Use date without copying or boxing
}
```

### Detecting Boxing:

#### **Performance Profilers**
- Use profilers to identify boxing hotspots
- Look for unexpected allocations in value type operations

#### **Code Analysis**
- Some analyzers can detect potential boxing scenarios
- Review collection usage and interface calls

### Key Principle:

Boxing should only occur when necessary (heterogeneous storage, legacy APIs, interface requirements). Prefer generic collections and safe unboxing with pattern matching. Understand the performance cost - boxing allocates heap memory and copying overhead.

### Common Anti-Patterns:

```csharp
// Anti-pattern: Non-generic collections
ArrayList list = new ArrayList();
list.Add(1); list.Add(2); list.Add(3);

// Better: Generic collections
List<int> list = new List<int> { 1, 2, 3 };

// Anti-pattern: Object parameters for value types
public void Log(object message) { }
Log(42); // Boxing

// Better: Generic or overloaded methods
public void Log<T>(T message) { }
public void Log(int message) { }
```

---

*This documentation guide complements the practical examples in [boxing-unboxing-examples.ipynb](../examples/boxing-unboxing-examples.ipynb)*