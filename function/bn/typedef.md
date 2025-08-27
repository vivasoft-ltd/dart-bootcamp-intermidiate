

## Dart-এ Typedef কী?

Dart-এ typedef ব্যবহার করা হয় ফাংশনের জন্য একটি **কাস্টম নাম** বানাতে। এটা কোডকে আরও **পরিষ্কার**, **পুনর্ব্যবহারযোগ্য** এবং **টাইপ-সেফ** করে তোলে। বিশেষ করে হায়ার-অর্ডার ফাংশন বা কলব্যাকের সঙ্গে কাজ করার সময়।

---

## কেন typedef ব্যবহার করবেন?

- কোড আরও স্পষ্ট হয়—ফাংশনের উদ্দেশ্য বোঝা সহজ হয়
- বারবার বড় ফাংশন টাইপ লেখার ঝামেলা কমে
- API ও ডকুমেন্টেশন পরিষ্কার থাকে
- জটিল লজিককে নাম দিয়ে আলাদা করা যায়

---

## সাধারণ সিনট্যাক্স

```dart
typedef Operation = int Function(int a, int b);
```

এখানে আমরা `Operation` নামে একটি ফাংশনের ধরন বানিয়েছি যেটি:

- দুইটি `int` ইনপুট নেয়
- একটি `int` আউটপুট দেয়

---

## উদাহরণ: কলব্যাক হিসেবে typedef ব্যবহার

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

এখানে `GreetFunction` ব্যবহার করায় ফাংশনের টাইপ একেবারে পরিষ্কার বোঝা যাচ্ছে।

---

## উদাহরণ: লজিক পাস করার জন্য typedef

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

এভাবে আপনি একাধিক typedef বানিয়ে বিভিন্ন ধরনের ফাংশনের লজিক সহজে পাস করতে পারেন।

---

## typedef বনাম Function টাইপ

typedef ছাড়া:

```dart
int compute(int a, int b, int Function(int, int) op) {
  return op(a, b);
}
```

typedef দিয়ে:

```dart
typedef BinaryOp = int Function(int, int);

int compute(int a, int b, BinaryOp op) {
  return op(a, b);
}
```

 দ্বিতীয় ভার্সনটা দেখতে অনেক পরিষ্কার, বুঝতেও সহজ।

---

## ক্লাসের সঙ্গে typedef ব্যবহার

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
এখানে `Validator` নামে typedef ব্যবহার করে বুঝতে সহজ হয়েছে যে ফাংশনটি কী কাজ করছে।

---

## সারাংশ

- `typedef` দিয়ে আপনি ফাংশনের জন্য কাস্টম টাইপ তৈরি করতে পারেন
- বড় বা জটিল ফাংশন সিগনেচারকে সহজ ও পড়তে আরামদায়ক করে তোলে
- Dart ও Flutter-এ কলব্যাক, API ডিজাইন, এবং উইজেট কনফিগারেশনে typedef খুব সাধারণভাবে ব্যবহার হয়
- কোড মেইনটেইন করা সহজ হয়, রিডেবিলিটিও বাড়ে

