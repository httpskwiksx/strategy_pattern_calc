# Assignment 5 - Strategy Pattern

**Topic:** Option B - Calculator  
**Student:** Ulzhan Musakhan  
**Group:** SE-2524  
**Environment:** Java JDK 17  
**Repository:** https://github.com/httpskwiksx/strategy_pattern_calc

## 1. Introduction

This project demonstrates the **Strategy behavioral design pattern** through a simple calculator. Its family of interchangeable algorithms consists of **addition, subtraction, and multiplication**. Each operation is implemented in a separate class that follows the shared `CalculationStrategy` interface.

`Calculator` is the **Context**. It stores a reference to the current strategy and delegates calculations to it. `Main` is the **Client**: it creates the calculator, selects an initial strategy, and replaces that strategy at runtime using `setStrategy()`. The same `Calculator` object is reused for all three operations, without an `if`/`switch` statement in the Context.

**Pattern roles:**

| Role | Class | Responsibility |
| --- | --- | --- |
| Client | `Main` | Creates the Context, selects strategies, and prints results. |
| Context | `Calculator` | Stores a strategy reference and delegates calculation. |
| Strategy interface | `CalculationStrategy` | Declares `calculate(int a, int b)`. |
| Concrete strategies | `AddStrategy`, `SubtractStrategy`, `MultiplyStrategy` | Implement three arithmetic algorithms. |

**Runtime switching in `Main.java`:**

```java
Calculator calculator = new Calculator(new AddStrategy());
System.out.println("Addition: " + calculator.calculate(10, 5));

calculator.setStrategy(new SubtractStrategy());
System.out.println("Subtraction: " + calculator.calculate(10, 5));

calculator.setStrategy(new MultiplyStrategy());
System.out.println("Multiplication: " + calculator.calculate(10, 5));
```

Expected output:

```text
Addition: 15
Subtraction: 5
Multiplication: 50
```

Only the strategy reference changes; the Context object is not recreated.

## 2. UML Class Diagram

![UML class diagram for Calculator Strategy Pattern](https://raw.githubusercontent.com/httpskwiksx/strategy_pattern_calc/main/docs/strategy-uml.png)

> The UML image is hosted in the GitHub repository (`docs/strategy-uml.png`). It will be accessible after the report and image have been pushed to `main`.

*Figure 1. Class structure of the Strategy calculator.*

The arrows labeled `creates` are **dependencies** from `Main` to `Calculator` and the concrete strategies. The `holds` arrow is an **association** from `Calculator` to `CalculationStrategy`. The three `implements` arrows are **realization** relationships from the concrete strategy classes to their shared interface.

## 3. Clean Code Principles

The following five principles are supported by annotated excerpts from the Java implementation.

### 3.1 Elimination of Conditional Logic

**Code excerpt - `Calculator.java`:**

```java
public int calculate(int a, int b) {
    return strategy.calculate(a, b);
}
```

**Explanation:** The Context calls a method defined by the strategy interface. Java polymorphism dispatches the call to the selected implementation. There is no `if/else` or `switch` inside `Calculator` to decide which arithmetic operation to perform.

**Benefit:** The algorithm-selection logic is removed from the Context, keeping the code small and easier to maintain.

### 3.2 Meaningful Names

**Code excerpt - `AddStrategy.java`:**

```java
public class AddStrategy implements CalculationStrategy {
    @Override
    public int calculate(int a, int b) {
        return a + b;
    }
}
```

**Explanation:** `CalculationStrategy` describes the common contract, `AddStrategy` identifies the concrete algorithm, and `calculate()` communicates what the method does.

**Benefit:** The purpose of each class and method is apparent without unnecessary comments.

### 3.3 Single Responsibility Principle

**Code excerpt - `SubtractStrategy.java`:**

```java
public class SubtractStrategy implements CalculationStrategy {
    @Override
    public int calculate(int a, int b) {
        return a - b;
    }
}
```

**Explanation:** `SubtractStrategy` is responsible only for the subtraction algorithm. Addition and multiplication are implemented independently in their own classes.

**Benefit:** A change to one operation does not require changing the implementations of other operations.

### 3.4 Algorithm-Independent Context

**Code excerpt - `Calculator.java`:**

```java
private CalculationStrategy strategy;

public Calculator(CalculationStrategy strategy) {
    this.strategy = strategy;
}

public void setStrategy(CalculationStrategy strategy) {
    this.strategy = strategy;
}
```

**Explanation:** The Context stores the **interface type** `CalculationStrategy`, not a particular concrete strategy. Its constructor accepts any implementation, and `setStrategy()` replaces the active implementation at runtime.

**Benefit:** `Calculator` remains independent of how addition, subtraction, or multiplication is internally implemented.

### 3.5 Easy Extensibility

**Code excerpts - `MultiplyStrategy.java` and `Main.java`:**

```java
public class MultiplyStrategy implements CalculationStrategy {
    @Override
    public int calculate(int a, int b) {
        return a * b;
    }
}
```

```java
calculator.setStrategy(new MultiplyStrategy());
System.out.println("Multiplication: " + calculator.calculate(10, 5));
```

**Explanation:** `MultiplyStrategy` shows how a separate algorithm can implement the same interface and be selected by the Client without modifying `Calculator`. Likewise, a future `DivideStrategy` could implement `CalculationStrategy` and then be selected by the Client. `DivideStrategy` is an **example of a possible extension**, not a class in the submitted project.

**Benefit:** New algorithms can be added as separate classes while the existing Context remains unchanged. This supports the Open/Closed Principle for `Calculator`.

## 4. Strategy vs. State

**Strategy** and **State** both use composition and interchangeable implementations, but they serve different purposes. Strategy lets a client choose between algorithms that perform the same kind of task. State changes an object's behavior according to its internal state and state transitions. This calculator uses Strategy because `Main` explicitly chooses the arithmetic operation with `setStrategy()`. `Calculator` does not manage state transitions.

## 5. Conclusion

The Strategy pattern makes the calculator flexible and easy to extend. It replaces conditional selection inside the Context with a shared interface and independent algorithm implementations. It also allows the same `Calculator` instance to switch algorithms at runtime. The main trade-off is having several small classes, but each class has a clear responsibility.

## 6. GitHub Repository

https://github.com/httpskwiksx/strategy_pattern_calc

## References

1. *Assignment #5 - Strategy Pattern*, Astana IT University, 2026–2027.
2. Martin, R. C. (2008). *Clean Code: A Handbook of Agile Software Craftsmanship*. Chapter 10.
3. Freeman, E., & Robson, E. (2021). *Head First Design Patterns* (2nd ed.). Chapter 1.
