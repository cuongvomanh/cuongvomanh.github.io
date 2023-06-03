---
layout: post
title: Same Level of Abstraction - Why Loose Coupling and High Cohesion Aren't Enough
categories: [Clean Code, Software Design, Loose Coupling, High Cohesion, Best Practices, Maintainable Code, Code Quality, Software Engineering, Modular Code, Granularity]
---

When we talk about writing clean and maintainable code, two concepts that frequently come up are loose coupling and high cohesion. These concepts define how modules and functions within our code should be organized for a more modular, flexible, and easier-to-maintain codebase. However, one crucial aspect of producing clean code that we often overlook is same level of abstraction.

What is Same Level of Abstraction?

Same level of abstraction means that modules and functions within a system should operate at the same level of abstraction. This approach is designed to keep different levels of detail separate in different modules or functions, helping to create cleaner, more focused, and more maintainable code.

Let's consider an example of some ecommerce software. Our system has a module including a function to calculate the total sales for a given period:

```python
def calculateTotalSales(start_date: datetime, end_date: datetime) -> float:
    total_sales = 0.0
    sales = getSalesBetweenDates(start_date, end_date)
    for sale in sales:
        total_sales += sale['price']
    return total_sales
```

This function operates at a high level of abstraction, which is how much total sales for a given period are. Meanwhile, another module containing a function to retrieve the details of a specific sale, would operate at a lower level of abstraction:

```python
def getSaleDetailsById(sale_id: int) -> dict:
    sale = getSaleById(sale_id)
    items = getSaleItems(sale_id)
    return {
        'sale': sale,
        'items': items,
    }
```

This function operates at a more granular level, providing the details of a specific sale. The granularity of the code in each module should be consistent across the system. This principle of same level of abstraction helps to ensure that each module remains focused and clear, making it easier to read, understand, and modify.

Why Same Level of Abstraction Matters

Ensuring same level of abstraction is essential for writing clean and maintainable code. If the code has inconsistencies in terms of granularity or level of detail, other developers will find it difficult to read and understand the system.

It's easy to fall into the trap of writing code that tries to do too much. For example, you might write a function that handles file IO, parsing, and networking in one. This approach breaks the same level of abstraction principles and makes it harder to maintain, extend, and reuse code.

To ensure same level of abstraction, it's critical to also ensure loose coupling and high cohesion of your modules. Loose coupling makes it easier to ensure each module stays at a consistent level of abstraction, and high cohesion helps to keep each module focused on a particular purpose.

In summary, same level of abstraction is critical for writing clean and maintainable code. The same level of abstraction principles help to keep different aspects of a system organized while maintaining a consistent level of detail. PureComponent can be used as an example of a "same level of abstraction" Component in React. By following best practices such as loose coupling and high cohesion, alongside the principles of same level of abstraction, we can create software that is cleaner, more organized, and easier to maintain.
