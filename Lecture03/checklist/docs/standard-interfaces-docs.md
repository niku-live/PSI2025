# Standard Interfaces - Documentation Guide

This document provides comprehensive documentation links and learning resources for understanding and implementing standard .NET interfaces in C#.

## Conceptual Understanding

Start here to understand the fundamental concepts of standard interfaces:

- **[Interfaces](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/interfaces/)** - Interface fundamentals
  
  Complete introduction to interfaces in C#, including definition, implementation, and when to use interfaces vs abstract classes.

- **[Common Interface Patterns](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/interface-design)** - Design principles
  
  Official Microsoft guidelines for interface design, including naming conventions and best practices for creating meaningful interfaces.

## Implementation Guides

Practical documentation for implementing specific standard interfaces:

### Equality and Comparison Interfaces

- **[IEquatable<T> Implementation](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type)** - Value equality
  
  Step-by-step guide to implementing IEquatable<T> for value equality, including GetHashCode() and best practices.

- **[IComparable<T> Implementation](https://learn.microsoft.com/en-us/dotnet/api/system.icomparable-1#examples)** - Natural ordering
  
  How to implement IComparable<T> for natural ordering of custom types, including handling null values and edge cases.

- **[IComparer<T> Implementation](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.icomparer-1#examples)** - Custom sorting
  
  Creating custom comparison logic with IComparer<T> for flexible sorting strategies and multi-criteria comparisons.

### Collection Interfaces

- **[IEnumerable<T> Implementation](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/iterators)** - Custom iteration
  
  Implementing IEnumerable<T> with yield statements and custom iterators for domain-specific collections.

- **[IEnumerator<T> Implementation](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerator-1#examples)** - Iterator patterns
  
  Manual IEnumerator<T> implementation for advanced scenarios and custom iteration logic.

## Best Practices & Advanced Patterns

Design guidelines and sophisticated interface techniques:

- **[Equality Operators and GetHashCode](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/equality-comparisons)** - Complete equality
  
  Implementing equality operators, Equals(), and GetHashCode() correctly for robust equality comparisons.

- **[Generic Interface Constraints](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/generics/constraints-on-type-parameters)** - Type constraints
  
  Using interface constraints in generic methods and classes to ensure type capabilities.

- **[Interface Segregation](https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles#interface-segregation-principle)** - SOLID principles
  
  Applying interface segregation principle to create focused, cohesive interfaces.

## Quick Reference

### Core Standard Interfaces Overview:

#### **IEquatable<T>** - Value Equality
```csharp
public class Person : IEquatable<Person>
{
    public string Name { get; set; }
    public int Age { get; set; }

    public bool Equals(Person other)
    {
        if (other is null) return false;
        if (ReferenceEquals(this, other)) return true;
        return Name == other.Name && Age == other.Age;
    }

    public override bool Equals(object obj) => Equals(obj as Person);
    
    public override int GetHashCode() => HashCode.Combine(Name, Age);
    
    public static bool operator ==(Person left, Person right) => 
        EqualityComparer<Person>.Default.Equals(left, right);
    
    public static bool operator !=(Person left, Person right) => !(left == right);
}
```

**When to implement:**
- Custom value equality logic needed
- Using objects as dictionary keys
- Collection contains/remove operations
- Business identity comparison

#### **IComparable<T>** - Natural Ordering
```csharp
public class Employee : IComparable<Employee>
{
    public string Name { get; set; }
    public decimal Salary { get; set; }
    public DateTime HireDate { get; set; }

    public int CompareTo(Employee other)
    {
        if (other is null) return 1;
        
        // Primary sort: Salary (descending)
        int salaryCompare = other.Salary.CompareTo(Salary);
        if (salaryCompare != 0) return salaryCompare;
        
        // Secondary sort: Hire date (ascending)
        int dateCompare = HireDate.CompareTo(other.HireDate);
        if (dateCompare != 0) return dateCompare;
        
        // Tertiary sort: Name (ascending)
        return string.Compare(Name, other.Name, StringComparison.OrdinalIgnoreCase);
    }
}
```

**When to implement:**
- Objects have natural ordering
- Default sorting behavior needed
- Using Array.Sort() or List<T>.Sort()
- SortedSet<T> or SortedDictionary<T,V> usage

#### **IComparer<T>** - Custom Sorting
```csharp
public class EmployeeNameComparer : IComparer<Employee>
{
    public int Compare(Employee x, Employee y)
    {
        if (x is null && y is null) return 0;
        if (x is null) return -1;
        if (y is null) return 1;
        
        return string.Compare(x.Name, y.Name, StringComparison.OrdinalIgnoreCase);
    }
}

public class EmployeeSalaryComparer : IComparer<Employee>
{
    private readonly bool _descending;
    
    public EmployeeSalaryComparer(bool descending = false)
    {
        _descending = descending;
    }
    
    public int Compare(Employee x, Employee y)
    {
        if (x is null && y is null) return 0;
        if (x is null) return -1;
        if (y is null) return 1;
        
        int result = x.Salary.CompareTo(y.Salary);
        return _descending ? -result : result;
    }
}

// Usage
employees.Sort(new EmployeeNameComparer());
employees.Sort(new EmployeeSalaryComparer(descending: true));
```

**When to implement:**
- Multiple sorting strategies needed
- Sorting logic separate from object
- Dynamic or configurable sorting
- Third-party object sorting

#### **IEnumerable<T>** - Custom Collections
```csharp
public class NumberRange : IEnumerable<int>
{
    private readonly int _start;
    private readonly int _end;
    private readonly int _step;

    public NumberRange(int start, int end, int step = 1)
    {
        _start = start;
        _end = end;
        _step = step;
    }

    public IEnumerator<int> GetEnumerator()
    {
        for (int i = _start; i <= _end; i += _step)
        {
            yield return i;
        }
    }

    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}

// Usage
var range = new NumberRange(1, 10, 2);
foreach (int number in range) // 1, 3, 5, 7, 9
{
    Console.WriteLine(number);
}
```

**When to implement:**
- Custom data structures
- Domain-specific collections
- Lazy evaluation scenarios
- LINQ integration needed

#### **IEnumerator<T>** - Manual Iteration
```csharp
public class TreeNode<T>
{
    public T Value { get; set; }
    public List<TreeNode<T>> Children { get; set; } = new();
}

public class TreeEnumerator<T> : IEnumerator<T>
{
    private readonly Stack<TreeNode<T>> _stack = new();
    private TreeNode<T> _current;

    public TreeEnumerator(TreeNode<T> root)
    {
        if (root != null)
            _stack.Push(root);
    }

    public T Current { get; private set; }
    object IEnumerator.Current => Current;

    public bool MoveNext()
    {
        if (_stack.Count == 0) return false;

        _current = _stack.Pop();
        Current = _current.Value;

        // Add children in reverse order for correct traversal
        for (int i = _current.Children.Count - 1; i >= 0; i--)
        {
            _stack.Push(_current.Children[i]);
        }

        return true;
    }

    public void Reset() => throw new NotSupportedException();
    public void Dispose() { }
}
```

**When to implement:**
- Complex iteration logic
- Memory-efficient traversal
- State management during iteration
- Custom data structure traversal

### Implementation Best Practices:

#### **IEquatable<T> Best Practices**
- Always implement with Equals(object) override
- Implement GetHashCode() consistently
- Consider operator overloads (==, !=)
- Handle null values appropriately
- Ensure reflexive, symmetric, transitive properties

#### **IComparable<T> Best Practices**
- Return consistent results for same inputs
- Handle null values (typically consider null less than any value)
- Ensure transitivity: if A < B and B < C, then A < C
- Consider culture-sensitive comparisons for strings
- Document comparison criteria clearly

#### **IComparer<T> Best Practices**
- Make stateless when possible
- Handle null values consistently
- Consider creating generic comparers for reusability
- Use Comparer<T>.Default for standard comparisons
- Document comparison logic

#### **IEnumerable<T> Best Practices**
- Use yield return for lazy evaluation
- Ensure thread-safety if needed
- Handle disposed state appropriately
- Consider deferred execution implications
- Implement IEnumerable (non-generic) for compatibility

### Common Patterns:

#### **Fluent Comparison Builder**
```csharp
public static class ComparisonBuilder<T>
{
    public static IComparer<T> OrderBy<TKey>(Func<T, TKey> keySelector)
        where TKey : IComparable<TKey>
    {
        return Comparer<T>.Create((x, y) => 
            Comparer<TKey>.Default.Compare(keySelector(x), keySelector(y)));
    }
    
    public static IComparer<T> OrderByDescending<TKey>(Func<T, TKey> keySelector)
        where TKey : IComparable<TKey>
    {
        return Comparer<T>.Create((x, y) => 
            Comparer<TKey>.Default.Compare(keySelector(y), keySelector(x)));
    }
}

// Usage
var comparer = ComparisonBuilder<Employee>
    .OrderByDescending(e => e.Salary)
    .ThenBy(e => e.Name);
```

#### **Composite Equality**
```csharp
public class Address : IEquatable<Address>
{
    public string Street { get; set; }
    public string City { get; set; }
    public string ZipCode { get; set; }

    public bool Equals(Address other) =>
        other is not null &&
        Street == other.Street &&
        City == other.City &&
        ZipCode == other.ZipCode;

    public override bool Equals(object obj) => Equals(obj as Address);
    
    public override int GetHashCode() => 
        HashCode.Combine(Street, City, ZipCode);
}
```

### Key Principle:

Implement interfaces to solve real business problems - IEquatable for business identity comparison, IComparable for domain-specific ordering, IEnumerable for custom collections with business logic, IComparer for multiple sorting strategies. Focus on adding business value rather than just satisfying technical requirements.

### API References:

For detailed API information, refer to these official references:
- [IEquatable<T> API](https://learn.microsoft.com/en-us/dotnet/api/system.iequatable-1)
- [IComparable<T> API](https://learn.microsoft.com/en-us/dotnet/api/system.icomparable-1)
- [IComparer<T> API](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.icomparer-1)
- [IEnumerator<T> API](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerator-1)
- [IEnumerable<T> API](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1)

---

*This documentation guide complements the practical examples in [standard-interfaces-examples.ipynb](../examples/standard-interfaces-examples.ipynb)*