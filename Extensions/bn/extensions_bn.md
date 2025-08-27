extensions ডার্ট-এর একটি শক্তিশালী ফিচার, যা বিদ্যমান ক্লাসগুলোর মূল কোড না বদলিয়ে তাতে অতিরিক্ত ফিচার বা কার্যকারিতা যোগ করার সুযোগ দেয়। এই ডকুমেন্টটিতে extensions সম্পর্কে বিস্তারিত আলোচনা করা হয়েছে — মৌলিক ধারণা থেকে শুরু করে বাস্তব জীবনে কীভাবে ব্যবহার করা যায়, তা পর্যন্ত।

## এক্সটেনশন (extensions) কী?

ডার্টে এক্সটেনশন হলো এমন একটি ল্যাঙ্গুয়েজ ফিচার, যার মাধ্যমে ডেভেলপাররা কোনো বিদ্যমান ক্লাসে নতুন মেথড, গেটার, সেটার এবং অপারেটর যোগ করতে পারেন, মূল ক্লাসটি পরিবর্তন না করেই বা কোনো সাবক্লাস তৈরি না করেই। এর মাধ্যমে আপনি এমন ক্লাসগুলোকেও বর্ধিত করতে পারেন, যেগুলোর ওপর আপনার নিয়ন্ত্রণ নেই বা আপনি নিজে তৈরি করেননি।

## এক্সটেনশন কেন দরকার?

এক্সটেনশন ডার্ট প্রোগ্রামিংয়ের বেশ কিছু সাধারণ সমস্যা সমাধান করে:

1. **ক্লাস পরিবর্তন না করেই নতুন ফিচার যোগ করা:** আপনি ডার্টের core লাইব্রেরি বা তৃতীয় পক্ষের প্যাকেজ থেকে নেওয়া ক্লাসগুলোতেও অতিরিক্ত ফিচার যোগ করতে পারেন।
2. **কোডকে সংগঠিত রাখা:** এক্সটেনশন ব্যবহার করে নির্দিষ্ট কোনো টাইপের ওপর কাজ করে এমন ফিচারগুলো এক জায়গায় রাখা যায়।
3. **ইউটিলিটি ক্লাস এড়িয়ে চলা:** আলাদা স্ট্যাটিক হেলপার মেথড তৈরি না করে, সরাসরি টাইপের সঙ্গে জুড়ে এক্সটেনশন মেথড তৈরি করা যায়।
4. **পাঠযোগ্যতা বৃদ্ধি:** কোড আরও পরিষ্কার এবং অবজেক্ট-ওরিয়েন্টেড স্টাইলে লেখা সম্ভব হয়, যেমন মেথড চেইনিং ব্যবহার করে।
5. **প্রসঙ্গভিত্তিক ফিচার যোগ করা:** সাধারণ ক্লাসে আপনি নির্দিষ্ট ডোমেইনভিত্তিক কার্যকারিতা যোগ করতে পারেন।

## এক্সটেনশন আসলে কী?

এক্সটেনশন আপনাকে বিদ্যমান যেকোনো টাইপে (যেমন int, String, List, enum ইত্যাদি) নতুন ফিচার বা কার্যকারিতা যোগ করার সুযোগ দেয়—সোর্স কোডে কোনো পরিবর্তন না করে এবং ইনহেরিটেন্স ছাড়াই।

আপনি ইতিমধ্যেই জানেন, ডার্টে String টাইপে .toUpperCase() এর মতো বিল্ট-ইন মেথড থাকে। এক্সটেনশনের সাহায্যে আপনি একইভাবে নিজের কাস্টম মেথড যেমন .capitalize() পুরো অ্যাপ জুড়ে সব String-এর জন্য যুক্ত করতে পারেন।

এটি কোডকে আরও ফ্লুয়েন্ট, রিডেবল এবং পুনঃব্যবহারযোগ্য করে তোলে।

## কীভাবে এক্সটেনশন লিখবেন

নিচে এক্সটেনশন লেখার সিনট্যাক্স দেখানো হলো:

```dart
extension YourExtensionName on TypeToExtend {
   // Method
  ReturnType methodName(Parameters) {
    // Implementation
  }

  // Getter
  ReturnType get propertyName {
    // Implementation
  }

  // Setter (if applicable)
  set propertyName(ReturnType value) {
    // Implementation
  }

  // Operator Overload (if applicable)
  ReturnType operator +(TypeToExtend other) {
    // Implementation
  }
}
```

**এক্সটেনশনের প্রধান উপাদানগুলো:**

1. **extension:** এক্সটেনশন শুরু করার জন্য ব্যবহৃত কীওয়ার্ড
2. **YourExtensionName:** এক্সটেনশনের নাম
3. **on TypeToExtend:** যেই টাইপ (যেমন class, int, double, String, enum ইত্যাদি)-এ এক্সটেনশন প্রয়োগ করা হচ্ছে
4. **body:** এখানে থাকবে এক্সটেনশনের মেথড, গেটার, সেটার এবং অপারেটরগুলো

## চলুন এক্সটেনশনের কিছু ব্যবহারিক উদাহরণ দেখি

### 1.String-এর প্রথম অক্ষর বড় করা (Capitalize করা)

নাম বা শিরোনামের প্রথম অক্ষর বড় করতে চান? এটা খুব সহজেই নিচের মতো করা যায়:

```dart
extension StringHelpers on String {
  String capitalize() {
    if (isEmpty) return '';
    return this[0].toUpperCase() + substring(1);
  }
}
```

এখন আপনি এটি আপনার কোডের যেকোনো জায়গায় ব্যবহার করতে পারবেন

```dart
void main() {
  String name = 'dart';
  print(name.capitalize()); // Output: Dart
}
```

### 2.তারিখকে সুন্দরভাবে ফরম্যাট করুন

তারিখগুলো সুন্দরভাবে দেখানো অনেক সময় কঠিন হয়, এই এক্সটেনশন তা সহজ করে তোলে:

```dart
import 'package:intl/intl.dart';

extension DateHelpers on DateTime {
  String toReadableFormat() {
    return DateFormat('dd MMM yyyy, hh:mm a').format(this);
  }
}

void main() {
  DateTime now = DateTime.now();
  print(now.toReadableFormat()); // Output: 11 May 2025, 02:30 PM
}
```

### 3.নিজের ক্লাসে প্রয়োজনীয় মেথড যোগ করুন

এক্সটেনশন আপনার নিজের তৈরি ক্লাসের উপরেও কাজ করে:

```dart
class User {
  final String name;
  final int age;
  User(this.name, this.age);
}

extension UserHelpers on User {
  String describe() => '$name is $age years old';
}
```

এখন যেকোনো User অবজেক্টে এই নতুন মেথডটি ব্যবহার করা যাবে

```dart
void main() {
  final user = User('Siam', 21);
  print(user.describe()); // Output: Siam is 21 years old
}
```

## চলুন একটি উদাহরণ দেখি — এক্সটেনশন ছাড়া এবং এক্সটেনশনসহ

নিচের কোডটি যাচাই করে যে কোনো List null বা খালি (empty) কি না, এবং সেই অনুযায়ী মেসেজ প্রিন্ট করে:

```dart
  void main() {
  List<String>? tags1 = ['flutter', 'dart'];
  List<String>? tags2 = [];
  List<String>? tags3 = null;

  if (tags1 != null && tags1.isNotEmpty) {
    print("tags1 is not null or empty"); // This will print
  } else {
    print("tags1 is null or empty");
  }

  if (tags2 != null && tags2.isNotEmpty) {
    print("tags2 is not null or empty");
  } else {
    print("tags2 is null or empty"); // This will print
  }

  if (tags3 != null && tags3.isNotEmpty) {
    print("tags3 is not null or empty");
  } else {
    print("tags3 is null or empty"); // This will print
  }
}
```

এক্সটেনশন ব্যবহারের পর:

```dart
// list_extensions.dart
extension NullableListUtils<T> on List<T>? {
  void isNotNullOrEmpty(){
    if(this != null && this!.isNotEmpty){
      print("${this} is not null or empty");
    }
    else{
       print("${this} is  null or empty");
    }
  }
}

import 'list_extensions.dart';

void main() {
  List<String>? tags1 = ['flutter', 'dart'];
  List<String>? tags2 = [];
  List<String>? tags3 = null;

  tags1.isNotNullOrEmpty();
  tags2.isNotNullOrEmpty();
  tags3.isNotNullOrEmpty();
}
```

এক্সটেনশন ব্যবহারের ফলে আপনার কোড হয় আরও সংক্ষিপ্ত, পরিষ্কার এবং বুঝতে সহজ — বিশেষ করে যখন একই ধরনের চেক বা অপারেশন বারবার করতে হয়।

## এক্সটেনশন ভিতরে ভিতরে (under the hood) কীভাবে কাজ করে

এই প্রক্রিয়াটা ভালোভাবে বোঝার জন্য প্রথমে একটি উদাহরণ দেখা যাক:

```dart
extension StringExtension on String {
  String capitalize() {
    if (isEmpty) return this;
    return this[0].toUpperCase() + substring(1).toLowerCase();
  }
}

void main() {
  String name = "john";
  print(name.capitalize()); // Output: John
}
```

ডার্টে এক্সটেনশন মূলত একটি সিনট্যাকটিক সুগার — অর্থাৎ, এটি কোড লেখার একটি সহজ, সংক্ষিপ্ত ও পাঠযোগ্য উপায়, কিন্তু ব্যাকএন্ডে এটি এক ধরণের স্ট্যাটিক মেথড কল হিসেবেই কাজ করে।

যখন আপনি কোনো এক্সটেনশন মেথড ব্যবহার করেন, ডার্ট কম্পাইলার এটি মূলত একটি স্ট্যাটিক মেথড কল-এ রূপান্তর করে। এ কারণে এক্সটেনশন ব্যবহারের কিছু সীমাবদ্ধতা রয়েছে। নিচে ধাপে ধাপে বিষয়টি ব্যাখ্যা করা হলো:

## 1. কম্পাইল টাইমেই এক্সটেনশন নির্ধারণ হয়:

ডার্ট কম্পাইলার অবজেক্টের স্ট্যাটিক টাইপ দেখে কম্পাইল করার সময়ই নির্ধারণ করে কোন এক্সটেনশনটি ব্যবহার হবে।অর্থাৎ, রানটাইমে নয়, এক্সটেনশন নির্বাচন হয় কোড কম্পাইল হওয়ার সময়েই।

## 2. মেথড কলকে স্ট্যাটিক মেথডে রূপান্তর:

ধরুন আপনি লিখেছেন:

```dart
name.capitalize()
```

কম্পাইলার এটিকে (ধারণাগতভাবে) এমনভাবে রিরাইট করে:

```dart
StringExtension.capitalize(name)
```

এখানে StringExtension হলো সেই এক্সটেনশন ক্লাসের নাম, যেটি capitalize() মেথডটি সংজ্ঞায়িত করেছে।

## 3. রানটাইম পলিমরফিজমে অংশ নেয় না:

যেহেতু এক্সটেনশন মেথড কম্পাইলটাইমে স্ট্যাটিকভাবে রেজলভ হয়, তাই এটি ইনহেরিটেন্স বা ওভাররাইডিংয়ের মতো রানটাইম মেথড ডিসপ্যাচের অংশ হয় না। অর্থাৎ, আপনি যদি একই নামের একাধিক এক্সটেনশন তৈরি করেন, সেক্ষেত্রে কম্পাইলার শুধুমাত্র একটিকে বেছে নেবে এবং বাকিগুলো উপেক্ষা করবে — এটি রানটাইমে বদলাবে না।

## 4. this কীওয়ার্ড ব্যবহার করে অবজেক্টে অ্যাক্সেস:

এক্সটেনশন মেথডের ভেতরে this কীওয়ার্ড ব্যবহার করে সেই অবজেক্টকে নির্দেশ করা হয়, যার ওপর এক্সটেনশনটি প্রয়োগ করা হয়েছে। এটি একদম ঠিক সাধারণ ক্লাস মেথডের মতোই কাজ করে, যেখানে this দিয়ে বর্তমান অবজেক্ট বোঝানো হয়।

## এক্সটেনশন দক্ষভাবে ব্যবহারের কিছু কার্যকর পরামর্শ

1. **এক্সটেনশনকে নির্দিষ্ট লক্ষ্যেই সীমাবদ্ধ রাখুন** - প্রতিটি এক্সটেনশন যেন একটি নির্দিষ্ট কাজের জন্যই তৈরি হয়।
2. **বুঝতে সহজ এমন নাম ব্যবহার করুন** -এক্সটেনশনের নাম এমনভাবে দিন, যাতে দেখে সহজেই বোঝা যায় এটি কী কাজ করে।
3. **অতিরিক্ত ব্যবহার থেকে বিরত থাকুন** -খুব বেশি এক্সটেনশন ব্যবহার করলে কোডের গঠন জটিল হয়ে যেতে পারে এবং বুঝতে অসুবিধা হতে পারে।
4. **পুনরাবৃত্ত কোড কমাতে ব্যবহার করুন** - যদি একই ধরনের কোড বারবার লিখতে হয়, তাহলে সেটিকে এক্সটেনশনে রূপান্তর করুন — এতে কোড সংক্ষিপ্ত এবং পরিষ্কার হয়।

এই টিপসগুলো অনুসরণ করলে আপনি এক্সটেনশন ফিচারটি আরও কার্যকরভাবে কাজে লাগাতে পারবেন।

## এক্সটেনশনের সীমাবদ্ধতা

1. **প্রাইভেট মেম্বারে অ্যাক্সেস নেই:** এক্সটেনশন যে ক্লাসে প্রয়োগ করা হয়, তার প্রাইভেট ভ্যারিয়েবল বা মেথডে এক্সটেনশন থেকে সরাসরি অ্যাক্সেস করা যায় না।

```dart
class User {
  String _secret = "hidden";

  String _privateMethod() => "Top Secret";
}

// extension.dart
extension UserExtension on User {
  String exposeSecret() {
    // return _secret;          // Error
    // return _privateMethod(); // Error
    return "Can't access private members";
  }
}
```

2. **স্ট্যাটিক রেজল্যুশন:** এক্সটেনশনগুলি কম্পাইল-টাইমে স্ট্যাটিকভাবে রেজলভ হয়, অর্থাৎ ভেরিয়েবলের ঘোষণা করা (declared) টাইপের উপর ভিত্তি করে, রানটাইম টাইপের উপর নয়।

```dart
class Animal {
  void speak() => print("Animal speaking");
}

class Dog extends Animal {
  void bark() => print("Woof!");
}

extension AnimalExtension on Animal {
  void extendedMethod() => print("Animal extension");
}

extension DogExtension on Dog {
  void extendedMethod() => print("Dog extension");
}

void main() {
  Animal a = Dog();
  a.extendedMethod(); // Output: Animal extension
}
```

3. **ওভাররাইডিং করা যায় না:** এক্সটেনশন মেথড দিয়ে ক্লাসের ইতিমধ্যেই থাকা মেথডগুলো ওভাররাইড করা সম্ভব নয়। অর্থাৎ, একই নামের মেথড থাকলে এক্সটেনশন মেথড সেটিকে প্রতিস্থাপন করতে পারে না।

```dart
class Car {
  void drive() => print("Driving a car");
}

extension CarExtension on Car {
  void drive() => print("Driving in extension"); // Ignored
}

void main() {
  Car car = Car();
  car.drive(); // Output: Driving a car
}
```

4. **super কল করা যায় না:** এক্সটেনশন থেকে আপনি super কীওয়ার্ড ব্যবহার করে প্যারেন্ট ক্লাসের মেথড কল করতে পারবেন না। অর্থাৎ, ইনহেরিটেন্সের মতো super কলের সুবিধা এক্সটেনশনে নেই।

```dart
extension MySuperExtension on Object {
  void callSuperMethod() {
    // super.toString(); // Error: 'super' can't be used in an extension
  }
}
```

5. **এক্সটেনশনের উপর এক্সটেনশন সম্ভব নয়:** এক্সটেনশনকে আরেকটি এক্সটেনশন দিয়ে এক্সটেন্ড করা যায় না। অর্থাৎ, এক্সটেনশন একইভাবে ইনহেরিট বা সম্প্রসারিত করা যায় না।

```dart
extension FirstExtension on String {
  String shout() => toUpperCase();
}

// Not allowed
// extension SecondExtension on FirstExtension {
//   String whisper() => toLowerCase();
// }
```

6. **নামকরণ সমস্যা** যদি একাধিক এক্সটেনশন একই নামের মেথড ডিফাইন করে, তাহলে আপনি স্পষ্টভাবে এক্সটেনশন ব্যবহার করতে হবে বা ইমপোর্ট প্রিফিক্স ব্যবহার করে এই দ্বন্দ্ব সমাধান করতে হবে।

```dart
// file_a.dart
extension ExtensionA on String {
  String describe() => "From A";
}

// file_b.dart
extension ExtensionB on String {
  String describe() => "From B";
}

// main.dart
import 'file_a.dart';
import 'file_b.dart';

void main() {
  "hello".describe(); // Error: Conflicting extensions
}

//solution
import 'file_a.dart' as a;
import 'file_b.dart' as b;

void main() {
  print(a.ExtensionA("hello").describe());
  print(b.ExtensionB("hello").describe());
}
```

7. **সীমিত কনস্ট্রাক্টর নিয়ন্ত্রণ:** এক্সটেনশন দিয়ে নতুন কনস্ট্রাক্টর বা ফ্যাক্টরি মেথড যোগ করা সম্ভব নয়। অর্থাৎ, এক্সটেনশন থেকে কোনো ক্লাসের ইনস্ট্যান্স তৈরি বা কাস্টমাইজ করা যায় না।

```dart
extension ConstructorExtension on String {
  // Can't do this:
  // ConstructorExtension(); // Error: Constructors not allowed in extensions
}
```

8. **ইনস্ট্যান্স ভেরিয়েবল নেই:** এক্সটেনশন মেথড এবং কম্পিউটেড প্রপার্টি ডিফাইন করতে পারে, কিন্তু ইনস্ট্যান্স ভেরিয়েবল (স্টেট) যোগ করতে পারে না। অর্থাৎ, এক্সটেনশন নিজস্ব কোনো ডাটা মেম্বার রাখতে পারে না।

```dart
extension MyExtension on String {
  // Error: Instance variables not allowed
  // int lengthCache = 0;

  int get doubleLength => this.length * 2; // Allowed
}
```

## উপসংহার

ডার্টে এক্সটেনশন হলো একটি চমৎকার ফিচার, যা বিদ্যমান ক্লাসে নতুন ফিচার যোগ করতে দেয়—সোর্স কোড পরিবর্তন ছাড়াই। এটি কোডকে করে তোলে আরও পরিষ্কার, সংগঠিত এবং রক্ষণাবেক্ষণযোগ্য।

বিশেষ করে যখন আপনি কোর লাইব্রেরি বা থার্ড-পার্টি ক্লাসের সঙ্গে কাজ করেন, এক্সটেনশন আপনাকে সহজেই সেই ক্লাসগুলিকে আপনার প্রয়োজন অনুযায়ী মানিয়ে নিতে সাহায্য করে—ইনহেরিটেন্স বা অতিরিক্ত ক্লাস ছাড়াই।

যদিও এতে কিছু সীমাবদ্ধতা আছে, তবে এর কাজের পদ্ধতি বুঝে ব্যবহার করলে এক্সটেনশন আপনাকে আরও কার্যকর, সংক্ষিপ্ত এবং প্রকাশপূর্ণ ডার্ট কোড লেখায় সহায়তা করবে।