# Class, Struct, Record, and Enum - Documentation Guide

This document provides comprehensive documentation links and learning resources for understanding and implementing classes, structs, records, and enums in C#.

## Conceptual Understanding

Start here to understand the fundamental concepts and when to use each type:

- **[Choosing Between Class and Struct](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/choosing-between-class-and-struct)** - Decision-making guide
  
  Essential resource for understanding when to choose one type over another. Covers memory allocation, performance implications, and design considerations.

- **[Class types](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/class)** - Core concepts
  
  Comprehensive overview of C# classes, including inheritance, access modifiers, and object-oriented principles.

## Implementation Guides

Practical documentation for implementing each type:

- **[Structure types](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/struct)** - How to implement structs
  
  Detailed guide on struct syntax, constructors, methods, and value type behavior.

- **[Record types](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/tutorials/records)** - Record implementation and usage
  
  Modern C# feature guide covering record syntax, immutability patterns, and with-expressions.

- **[Enumeration types](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/enum)** - Enum implementation
  
  Complete guide to enums, including underlying types, flags, and best practices.

## Best Practices & Advanced Patterns

Design guidelines and advanced usage patterns:

- **[Type Design Guidelines](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/type)** - When to use each type
  
  Microsoft's official guidelines for type design, covering naming conventions, member design, and inheritance patterns.

- **[Immutable Types Best Practices](https://learn.microsoft.com/en-us/dotnet/csharp/write-safe-efficient-code)** - Record and struct patterns
  
  Modern approaches to writing safe, efficient code with immutable types and defensive programming techniques.

## Quick Reference

### When to Use Each Type:

- **Classes**: Complex entities with identity, behavior, and potentially mutable state (e.g., User, ShoppingCart, DatabaseConnection)
- **Structs**: Small, immutable value objects without identity (e.g., Point, Money, Coordinate, Color)  
- **Records**: Immutable data transfer objects with value-based equality (e.g., DTOs, API responses, configuration objects)
- **Enums**: Named constants representing discrete choices (e.g., OrderStatus, LogLevel, FileType)

### Performance Considerations:

- Structs should be small (≤16 bytes) and immutable to avoid copying overhead
- Classes are reference types allocated on the heap
- Records provide automatic equality and immutability features
- Enums are efficient for representing fixed sets of values

---

*This documentation guide complements the practical examples in [class-struct-record-enum-examples.ipynb](../examples/class-struct-record-enum-examples.ipynb)*