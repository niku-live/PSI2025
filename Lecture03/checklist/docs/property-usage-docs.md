# Property Usage in Struct and Class - Documentation Guide

This document provides comprehensive documentation links and learning resources for understanding and implementing properties in C# classes and structs.

## Conceptual Understanding

Start here to understand the fundamental concepts of properties:

- **[Properties Overview](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/properties)** - Core property concepts
  
  Comprehensive introduction to properties, covering syntax, access levels, and the difference between properties and fields.

- **[Auto-Implemented Properties](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/auto-implemented-properties)** - When and how to use auto-properties
  
  Guide to auto-implemented properties, including when to use them vs full properties, and initialization patterns.

## Implementation Guides

Practical documentation for implementing properties:

- **[Property Implementation Patterns](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/how-to-implement-a-lightweight-class-with-auto-implemented-properties)** - Practical implementation
  
  Real-world examples of property implementation in lightweight classes, showing best practices and common patterns.

- **[Indexers](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/indexers/)** - Custom indexer implementation
  
  Complete guide to indexers (indexed properties), including syntax, overloading, and use cases for collection-like classes.

## Best Practices & Advanced Patterns

Design guidelines and modern property techniques:

- **[Property Design Guidelines](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/property)** - Microsoft's property design recommendations
  
  Official Microsoft guidelines for property design, covering naming, behavior, and when to use properties vs methods.

- **[Expression-bodied Properties](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/expression-bodied-members)** - Modern property syntax patterns
  
  Modern C# syntax for expression-bodied properties, getters, and setters, making code more concise and readable.

## Quick Reference

### Property Types:

- **Auto-implemented properties**: Simple properties without custom logic
- **Full properties**: Properties with custom getter/setter logic
- **Read-only properties**: Properties with only getters
- **Computed properties**: Properties that calculate values on access
- **Indexers**: Array-like access to class/struct data

### When to Use Properties:

- **Data encapsulation**: Control access to private fields
- **Validation**: Validate input before setting values
- **Computed values**: Calculate values based on other data
- **Change notification**: Trigger events when values change
- **Future extensibility**: Start with auto-properties, extend later

### Key Principle:

Properties should control access to data with validation, computed values, or encapsulation. Avoid simple auto-properties that could be public fields - use properties when you need logic, validation, or future extensibility.

---

*This documentation guide complements the practical examples in [property-usage-examples.ipynb](../examples/property-usage-examples.ipynb)*