# Collection Iteration - Documentation Guide

This document provides comprehensive documentation links and learning resources for understanding and implementing proper collection iteration techniques in C#.

## Conceptual Understanding

Start here to understand the fundamental concepts of collection iteration:

- **[Collections and Data Structures](https://learn.microsoft.com/en-us/dotnet/standard/collections/)** - Overview of .NET collections
  
  Comprehensive guide to .NET collection types, including performance characteristics and when to use each type.

- **[Iterators](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/iterators)** - Core iteration concepts
  
  Understanding how iterators work in C#, including yield statements and deferred execution.

## Implementation Guides

Practical documentation for different iteration techniques:

- **[foreach Statement](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/iteration-statements#the-foreach-statement)** - Primary iteration method
  
  Complete guide to foreach loops, including how they work with IEnumerable and when to use them.

- **[for and while Statements](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/iteration-statements)** - Index-based iteration
  
  Understanding traditional loop constructs and when they're preferable to foreach.

- **[LINQ Query Operations](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)** - Functional iteration
  
  Using LINQ for data processing, filtering, and transformation during iteration.

## Best Practices & Advanced Patterns

Design guidelines and sophisticated iteration techniques:

- **[Performance Considerations](https://learn.microsoft.com/en-us/dotnet/standard/collections/selecting-a-collection-class)** - Choosing the right approach
  
  Performance implications of different iteration methods and collection types.

- **[Async Enumeration](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-8#asynchronous-streams)** - Modern async patterns
  
  Using IAsyncEnumerable for asynchronous iteration patterns in modern C#.

- **[Thread-Safe Collections](https://learn.microsoft.com/en-us/dotnet/standard/collections/thread-safe/)** - Concurrent iteration
  
  Safe iteration patterns when working with concurrent collections and multithreading.

## Quick Reference

### Choosing the Right Iteration Method:

#### **Use `foreach` when:**
- Simple iteration through all elements
- Working with IEnumerable/ICollection
- Don't need element index
- Want clean, readable code
- Working with LINQ results

```csharp
foreach (var item in collection)
{
    ProcessItem(item);
}
```

#### **Use `for` loop when:**
- Need element index/position
- Iterating arrays or indexed collections
- Need to iterate backwards or skip elements
- Performance is critical (arrays)
- Need to modify the collection during iteration

```csharp
for (int i = 0; i < array.Length; i++)
{
    ProcessItem(array[i], i);
}
```

#### **Use `while` loop when:**
- Complex iteration conditions
- Unknown number of iterations
- Conditional advancement through collection
- Custom iterator logic

```csharp
while (enumerator.MoveNext())
{
    if (SomeCondition(enumerator.Current))
        ProcessItem(enumerator.Current);
}
```

#### **Use LINQ when:**
- Data processing/transformation
- Filtering during iteration
- Complex queries
- Functional programming style
- Chaining operations

```csharp
var results = collection
    .Where(x => x.IsActive)
    .Select(x => x.Name)
    .OrderBy(name => name);
```

### Safety Considerations:

- **Null checks**: Always validate collections before iteration
- **Collection modification**: Avoid modifying collection during foreach
- **Exception handling**: Handle potential exceptions in iteration logic
- **Resource disposal**: Use `using` with disposable enumerators

### Performance Tips:

- **Cache properties**: `for (int i = 0, count = list.Count; i < count; i++)`
- **Use spans for arrays**: `foreach (var item in array.AsSpan())`
- **Consider ReadOnlySpan**: For performance-critical scenarios
- **Avoid nested LINQ**: Can cause performance issues

### Common Patterns:

```csharp
// Safe null iteration
foreach (var item in collection ?? Enumerable.Empty<T>())

// Index with foreach using LINQ
foreach (var (item, index) in collection.Select((item, index) => (item, index)))

// Reverse iteration
for (int i = list.Count - 1; i >= 0; i--)

// Concurrent-safe iteration
foreach (var item in collection.ToList()) // snapshot
```

### Key Principle:

Choose the right iteration method for the task - foreach for simple iteration, for loops when you need index control, LINQ for data processing. Always consider safety (null checks), performance (cache properties), and readability.

### Modern C# Features:

- **Range and Index**: `foreach (var item in collection[1..^1])`
- **Pattern matching**: `foreach (var item in collection) { if (item is SpecificType specific) ... }`
- **Tuple deconstruction**: `foreach (var (key, value) in dictionary)`

---

*This documentation guide complements the practical examples in [collection-iteration-examples.ipynb](../examples/collection-iteration-examples.ipynb)*