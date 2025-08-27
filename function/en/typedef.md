

## What is Typedef in Dart?

In Dart, `typedef` is used to create a **custom name** for a function type. It helps make your code more **readable**, **reusable**, and **type-safe**—especially when working with higher-order functions or callbacks.

---

## Why Use Typedef?

- Improve code clarity with meaningful names
- Avoid repetitive long function type declarations
- Enable flexible APIs and cleaner documentation

---

## Basic Syntax

```dart
typedef Operation = int Function(int a, int b);
```

This defines a function signature where:
- The function returns an `int`
- It takes two `int` parameters

---

## Example: Using Typedef for Callbacks

```dart
typedef GreetFunction = void Function(String name);

void greetUser(String name, GreetFunction greeter) {
  greeter(name);
}

void main() {
  greetUser("Omar", (name) {
    print("Hello, $name!");
  });
}
```

Here, `GreetFunction` improves readability by naming the function type.

---

## Example: Passing Logic as Typedef

```dart
typedef IntMath = int Function(int, int);

int calculate(int a, int b, IntMath operation) {
  return operation(a, b);
}

void main() {
  int result = calculate(4, 2, (x, y) => x * y);
  print(result); // Output: 8
}
```

You can define multiple typedefs for different function shapes, enabling flexible logic reuse.

---

## Typedef vs Function Types

Without typedef:

```dart
int compute(int a, int b, int Function(int, int) op) {
  return op(a, b);
}
```

With typedef:

```dart
typedef BinaryOp = int Function(int, int);

int compute(int a, int b, BinaryOp op) {
  return op(a, b);
}
```

 Typedef makes it much easier to read and maintain.

---

## Using Typedefs with Classes

```dart
typedef Validator = bool Function(String);

class FormField {
  final Validator validator;

  FormField(this.validator);

  void validate(String input) {
    if (validator(input)) {
      print("Valid input.");
    } else {
      print("Invalid input.");
    }
  }
}

void main() {
  var field = FormField((text) => text.isNotEmpty);
  field.validate("Hello"); // Output: Valid input.
}
```

---

## Summary

- `typedef` creates aliases for function types.
- Makes complex function signatures easier to manage.
- Commonly used in Dart for callbacks, APIs, and Flutter widgets.
- Boosts code readability and maintainability.

