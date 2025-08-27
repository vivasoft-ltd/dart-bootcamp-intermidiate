
## What is a Higher-Order Function?

A **higher-order function** is a function that can do at least one of the following:

1. **Take another function as a parameter**
2. **Return a function as a result**

This concept is powerful in Dart because **functions are first-class citizens**. That means you can assign functions to variables, pass them around, and return them from other functions—just like any other object.

---

## Why Use Higher-Order Functions?

- **Cleaner Code**: Avoids repetition and enhances readability.
- **Flexible Design**: Enables functional programming techniques.
- **Reusability**: Write generic methods that work with different behaviors.
- **Simplify Your Code**: Reduce duplication by abstracting behavior.
- **Enable Functional Patterns**: Chain transformations and build pipelines.

---

## Example 1: Passing a Function as a Parameter

```dart
void greet(String name, String Function(String) formatter) {
  print(formatter(name));
}

String uppercase(String input) => input.toUpperCase();

void main() {
  greet('Alice', uppercase); // Output: ALICE
}
```

---

## Example 2: Returning a Function

```dart
Function multiplier(int factor) {
  return (int value) => value * factor;
}

void main() {
  var triple = multiplier(3);
  print(triple(5)); // Output: 15
}
```

---

## Real-World Use Case: List Transformation

```dart
void main() {
  List<int> numbers = [1, 2, 3, 4];
  List<int> doubled = numbers.map((n) => n * 2).toList();
  print(doubled); // Output: [2, 4, 6, 8]
}
```

---

## Common Dart Higher-Order Functions

| Function      | Purpose                            |
|---------------|-------------------------------------|
| `.map()`       | Transforms each element             |
| `.where()`     | Filters a list                      |
| `.forEach()`   | Iterates through each element       |
| `.reduce()`    | Combines all elements into one      |
| `.sort()`      | Sorts elements (takes compare func) |

---

## Practice Challenge

Write a function called `applyTwice` that takes a function and a value, and applies the function twice.

```dart
int applyTwice(int value, int Function(int) func) {
  return func(func(value));
}

void main() {
  int square(int x) => x * x;
  print(applyTwice(2, square)); // Output: 16
}
```

---

## Summary

- Higher-order functions make Dart **more expressive** and **modular**.
- They let you write functions that are **generic and reusable**.
- This is a foundational concept for writing **clean Flutter code**.
