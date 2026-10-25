# Strategy Pattern - Calculator

## Description

This project demonstrates the Strategy design pattern in Java 17.

The calculator supports three interchangeable algorithms:
- Addition
- Subtraction
- Multiplication

The Strategy pattern allows changing the algorithm at runtime without modifying the Calculator class.

## Project Structure

- CalculationStrategy.java - Strategy interface
- AddStrategy.java - Addition algorithm
- SubtractStrategy.java - Subtraction algorithm
- MultiplyStrategy.java - Multiplication algorithm
- Calculator.java - Context
- Main.java - Client

## How to Run

Requirements: Java JDK 17 or later.

Compile:

```bash
javac -d out src/*.java
```

Run:

```bash
java -cp out Main
```

## Expected Output

```text
Addition: 15
Subtraction: 5
Multiplication: 50
```

## Runtime Strategy Switching

The Calculator stores a reference to CalculationStrategy.

The setStrategy() method changes the current algorithm at runtime.

## How to Add a New Strategy

1. Create a new class implementing CalculationStrategy.
2. Override the calculate(int a, int b) method.
3. Implement the new algorithm.
4. Select the new strategy in Main using setStrategy().

The Calculator class does not need to be modified.

## GitHub Repository

https://github.com/httpskwiksx/strategy_pattern_calc
