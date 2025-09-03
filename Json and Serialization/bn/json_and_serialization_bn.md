**JSON (JavaScript Object Notation)** হচ্ছে একটি হালকা-ধরনের (lightweight) ডেটা বিনিময়ের ফরম্যাট। এটি এমনভাবে তৈরি যে, মানুষ সহজেই এটা পড়তে ও বুঝতে পারে এবং কম্পিউটারও খুব সহজে এটি বিশ্লেষণ (parse) ও তৈরি (generate) করতে পারে। **Dart-এ JSON serialization** মানে হলো একটি Dart অবজেক্ট (যেমন কোনো ক্লাসের ইনস্ট্যান্স) কে JSON স্ট্রিং-এ রূপান্তর করা। আর **deserialization** মানে ঠিক উল্টো কাজটি—একটি JSON স্ট্রিং থেকে আবার Dart অবজেক্ট তৈরি করা।

## Why do we need JSON serialization?

Dart একটি strongly typed এবং object-oriented প্রোগ্রামিং ভাষা। অন্যদিকে, **JSON** সাধারণত ডেটা আদান-প্রদানের জন্য (যেমন: API, লোকাল স্টোরেজ বা ফাইল ব্যবস্থাপনা) ব্যবহৃত হয়।
যখন আপনি কোনো **API-র সাথে কাজ করেন** বা ডেটা স্থানীয়ভাবে (local database), যেমন **SharedPreferences বা Hive-এ সংরক্ষণ করেন**, তখন প্রায়ই সেই ডেটা JSON ফরম্যাটে পাঠাতে বা পেতে হয়।
এই JSON ডেটাকে Dart অ্যাপে ব্যবহারের জন্য প্রথমে **Dart অবজেক্টে রূপান্তর (deserialization)** করতে হয়। একইভাবে, যদি Dart অ্যাপ থেকে JSON ফরম্যাটে ডেটা পাঠাতে চান, তাহলে সেই ডেটাকে আগে **JSON স্ট্রিং-এ রূপান্তর (serialization)** করতে হবে।

## চলুন একটি বাস্তব উদাহরণ দেখি

ধরুন, আপনি কোনো API থেকে নিচের JSON ডেটা পাচ্ছেন:

```dart
{
  "name": "Charlie",
  "age": 28
}
```

চলুন দেখি, কীভাবে এই ডেটাকে একটি Dart ক্লাসে রূপান্তর করা যায়:

```dart
class Person{
  final String name;
  final int age;

  Person({required this.name, required this.age});
}
```

## Deserialization: JSON থেকে Dart অবজেক্টে রূপান্তর

```dart
factory Person.fromJson(Map<String, dynamic> json) {
  return Person(
    name: json['name'],
    age: json['age'],
  );
}
```

## Serialization: Dart অবজেক্ট থেকে JSON এ রূপান্তর

```dart
Map<String, dynamic> toJson(Person person) {
  return {
    'name': person.name,
    'age': person.age,
  };
}
```

এখন পুরো ক্লাসটি নিচের মতো দেখাবে:

```dart
class Person{
  final String name;
  final int age;

  Person({required this.name, required this.age});

  factory Person.fromJson(Map<String, dynamic> json) {
    return Person(
      name: json['name'],
      age: json['age'],
    );
  }

  Map<String, dynamic> toJson() {
   return {
    'name': name,
    'age': age,
   };
  }
}

void main() {
  // Step 1: Get JSON data (e.g., from API)
  Map<String, dynamic> jsonData = {
    'name': 'Diana',
    'age': 34
  };

  // Step 2: Convert JSON to Person object
  Person person = Person.fromJson(jsonData);
  print('Deserialized -> Name: ${person.name}, Age: ${person.age}');

  // Step 3: Convert Person object back to JSON
  Map<String, dynamic> jsonOutput = person.toJson();
  print('Serialized -> JSON: $jsonOutput');
}
```

## Dart-এ JSON serialization করার পদ্ধতিসমূহ

## সরাসরি (ম্যানুয়াল) serialization

Dart-এর dart:convert লাইব্রেরি JSON রূপান্তরের জন্য প্রস্তুত (built-in) সাপোর্ট প্রদান করে। এর মাধ্যমে আপনি অতিরিক্ত কোনো প্যাকেজ বা ডিপেন্ডেন্সি ছাড়াই সরাসরি JSON ডেটার সাথে কাজ করতে পারেন।

## JSON এবং Dart অবজেক্টের মধ্যে রূপান্তর

1. **JSON থেকে Dart:**

   - JSON স্ট্রিংকে Dart অবজেক্টে রূপান্তর করতে **jsonDecode()** ব্যবহার করা হয়।
   - ফলাফল হিসেবে আপনি পাবেন List, Map, এবং বেসিক ডেটা টাইপগুলো (যেমন String, Number, Boolean) মিলে গঠিত একটি অবজেক্ট।

2. **Dart থেকে JSON:**
   - Dart অবজেক্টকে JSON স্ট্রিং-এ রূপান্তর করতে **jsonEncode()** ব্যবহার করা হয়।
   - JSON এ সরাসরি রূপান্তরযোগ্য টাইপগুলোর মধ্যে আছে: int, String, bool, List ও Map।

```dart
class Person{
  final String name;
  final int age;

  Person({required this.name, required this.age});

  facotory Person.fromJson(Map<String, dynamic> json) {
    return Person(
      name: json['name'],
      age: json['age'],
    );
  }

  Map<String, dynamic> toJson() {
   return {
    'name': name,
    'age': age,
   };
  }
}

import 'dart:convert';

void main() {
  // Create an instance of the Person class
  Person person = Person('John Doe', 25);

  // Convert the Person object to JSON
  String jsonString = jsonEncode(person.toJson());
  print('Serialized JSON: $jsonString');

  // Convert the JSON back to a Dart object
  Map<String, dynamic> jsonMap = jsonDecode(jsonString);
  Person decodedPerson = Person.fromJson(jsonMap);
  print('Deserialized Person: ${decodedPerson.name}, ${decodedPerson.age}');

}
```

## json_serializable প্যাকেজ ব্যবহার করে কোড জেনারেশন

যখন মডেলগুলো কিছুটা জটিল হয়, তখন json_serializable প্যাকেজ ব্যবহার করাই সবচেয়ে ভালো সমাধান। প্রথমে আপনার pubspec.yaml ফাইলে প্রয়োজনীয় ডিপেন্ডেন্সি গুলো যোগ করুন:

````dart
dependencies:
  json_annotation: ^4.8.1

dev_dependencies:
  build_runner: ^2.4.6
  json_serializable: ^6.7.1
``


এরপর আপনার মডেল ক্লাসটি নিচের মতো লিখুন:

```dart
import 'package:json_annotation/json_annotation.dart';

part 'user.g.dart';  // This will be generated

@JsonSerializable()
class User {
  final String name;
  final int age;
  final String email;

  User({required this.name, required this.age, required this.email});

  // Connect the generated code
  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
  Map<String, dynamic> toJson() => _$UserToJson(this);
}
````

তারপর কমান্ড লাইনে নিচের মতো **কোড জেনারেটর চালান**:

```dart
  dart pub run build_runner build
```

এটি সব প্রয়োজনীয় (boilerplate) serialization কোড স্বয়ংক্রিয়ভাবে তৈরি করে দেয়।

## @JsonSerializable() এর প্যারামিটারসমূহ

1. **createFactory**

**Default: true**

যদি false থাকে, তাহলে fromJson ফ্যাক্টরি মেথডটি তৈরি হবে না।

```dart
@JsonSerializable(createFactory: false)
class User {
  final String name;

  User({required this.name});

  // You must implement fromJson manually if createFactory is false
  factory User.fromJson(Map<String, dynamic> json) {
    return User(name: json['name']);
  }

  Map<String, dynamic> toJson() => _$UserToJson(this);
}
```

2. **createToJson**

**Default: true**

যদি false থাকে, তাহলে toJson ফ্যাক্টরি মেথডটি তৈরি হবে না।

```dart
@JsonSerializable(createToJson: false)
class Product {
  final String id;

  Product({required this.id});

  factory Product.fromJson(Map<String, dynamic> json) => _$ProductFromJson(json);

  // You must implement toJson manually
  Map<String, dynamic> toJson() {
    return {'id': id};
  }
}
```

3. **explicitToJson**

**Default: false**

নেস্টেড অবজেক্ট থাকলে, এটি নিশ্চিত করে প্রতিটি নেস্টেড অবজেক্টের উপর সরাসরি **.toJson()** মেথড কল করা হয়।

```dart
@JsonSerializable(explicitToJson: true)
class Order {
  final Customer customer;

  Order({required this.customer});

  factory Order.fromJson(Map<String, dynamic> json) => _$OrderFromJson(json);
  Map<String, dynamic> toJson() => _$OrderToJson(this);
}

@JsonSerializable()
class Customer {
  final String name;

  Customer({required this.name});

  factory Customer.fromJson(Map<String, dynamic> json) => _$CustomerFromJson(json);
  Map<String, dynamic> toJson() => _$CustomerToJson(this);
}
```

**explicitToJson: true** সেট না করলে, নেস্টেড **Customer.toJson()** সঠিকভাবে কল নাও হতে পারে।

4. **includeIfNull**

**Default: true**

নির্ধারণ করে আউটপুট JSON-এ null মানগুলো অন্তর্ভুক্ত করা হবে কি না।

```dart
@JsonSerializable(includeIfNull: false)
class Article {
  final String title;
  final String? subtitle; // will be omitted from output if null

  Article({required this.title, this.subtitle});

  factory Article.fromJson(Map<String, dynamic> json) => _$ArticleFromJson(json);
  Map<String, dynamic> toJson() => _$ArticleToJson(this);
}
```

5. **fieldRename**
   Dart ফিল্ডগুলোকে JSON-এ স্বয়ংক্রিয়ভাবে ভিন্ন কেসিং ফরম্যাটে নামকরণ করা হয়। কেসিং ফরম্যাটসমূহ:

- none (default)
- snake
- kebab
- pascal

```dart
@JsonSerializable(fieldRename: FieldRename.snake)
class BlogPost {
  final String postTitle; // becomes "post_title"
  final DateTime publishedAt;

  BlogPost({required this.postTitle, required this.publishedAt});

  factory BlogPost.fromJson(Map<String, dynamic> json) => _$BlogPostFromJson(json);
  Map<String, dynamic> toJson() => _$BlogPostToJson(this);
}

```

6. **anyMap**

**Default: false**

Map<String, dynamic> এর পরিবর্তে Map<dynamic, dynamic> ব্যবহার করার অনুমতি দেয়।

```dart
@JsonSerializable(anyMap: true)
class FlexibleModel {
  final String data;

  FlexibleModel({required this.data});

  factory FlexibleModel.fromJson(Map json) => _$FlexibleModelFromJson(json);
  Map<String, dynamic> toJson() => _$FlexibleModelToJson(this);
}
```

7. **checked**

**Default: false**

Deserialize-করার সময় রানটাইমে টাইপ পরীক্ষা করে (টাইপ মিল না হলে বিস্তারিত ত্রুটি দেখায়)।

```dart
@JsonSerializable(checked: true)
class SafeModel {
  final int age;

  SafeModel({required this.age});

  factory SafeModel.fromJson(Map<String, dynamic> json) => _$SafeModelFromJson(json);
  Map<String, dynamic> toJson() => _$SafeModelToJson(this);
}
```

## @JsonSerializable ব্যবহারের জন্য কার্যকর টিপস

1. নেস্টেড মডেলের জন্য **explicitToJson: true** ব্যবহার করুন।
2. API-এর ফিল্ড নামকরণের নিয়ম অনুযায়ী মিলাতে **fieldRename** ব্যবহার করুন।
3. JSON আউটপুট পরিষ্কার রাখার জন্য **includeIfNull: false** ব্যবহার করুন।
4. প্রয়োজন অনুযায়ী স্বয়ংক্রিয়ভাবে তৈরি হওয়া মেথড বন্ধ করুন (**createFactory: false, createToJson: false**)।
5. নমনীয় ডেটা সোর্সের জন্য **anyMap: true** ব্যবহার করুন।
6. কোনো পরিবর্তনের পর অবশ্যই কোড পুনরায় জেনারেট করুন (কমান্ড: dart pub run build_runner build)।
7. ক্লাস ফাইলের শুরুতে **part 'model.g.dart';** যোগ করতে ভুলবেন না।

## Dart-এ (json_serializable প্যাকেজে) @JsonKey অ্যানোটেশন কী?

@JsonKey অ্যানোটেশন হল json_serializable প্যাকেজের একটি শক্তিশালী টুল, যা আপনাকে নির্দিষ্ট ফিল্ডগুলো কীভাবে সিরিয়ালাইজ এবং ডেসিরিয়ালাইজ হবে তা নিয়ন্ত্রণ করার সূক্ষ্ম সুবিধা দেয়। চলুন উদাহরণের মাধ্যমে এর সব ফিচারগুলো দেখি:

1. **name**

JSON-এ ফিল্ডের নাম পরিবর্তন করে।

```dart
@JsonSerializable()
class User {
  @JsonKey(name: 'full_name')
  final String name;

  User({required this.name});

  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
  Map<String, dynamic> toJson() => _$UserToJson(this);
}
```

**JSON input/Output:**

```dart
{ "full_name": "Alice" }
```

2. **ignore**

ফিল্ডটিকে সিরিয়ালাইজেশন এবং ডেসিরিয়ালাইজেশন উভয় থেকে বাদ দেয়।

```dart
@JsonSerializable()
class Session {
  final String token;

  @JsonKey(ignore: true)
  final bool? isLoggedIn; // Ignored

  Session({required this.token, this.isLoggedIn});

  factory Session.fromJson(Map<String, dynamic> json) => _$SessionFromJson(json);
  Map<String, dynamic> toJson() => _$SessionToJson(this);
}
```

এখন isLoggedIn ফিল্ডটি JSON আউটপুটে দেখানো হবে না এবং ইনপুট থেকেও নেওয়া হবে না।

3. **defaultValue**

JSON ইনপুটে ফিল্ডটি অনুপস্থিত থাকলে একটি ডিফল্ট মান প্রদান করে।

```dart
@JsonSerializable()
class Settings {
  @JsonKey(defaultValue: true)
  final bool darkMode;

  Settings({required this.darkMode});

  factory Settings.fromJson(Map<String, dynamic> json) => _$SettingsFromJson(json);
  Map<String, dynamic> toJson() => _$SettingsToJson(this);
}

```

**Input: {} → Result:** darkMode = true

3. **includeIfNull**

নির্ধারণ করে ফিল্ডটির মান null হলে সেটি আউটপুট JSON-এ অন্তর্ভুক্ত করা হবে কিনা।

```dart
@JsonSerializable(includeIfNull: false)
class Article {
  final String title;

  @JsonKey(includeIfNull: false)
  final String? summary;

  Article({required this.title, this.summary});

  factory Article.fromJson(Map<String, dynamic> json) => _$ArticleFromJson(json);
  Map<String, dynamic> toJson() => _$ArticleToJson(this);
}
```

**Input:** summary = null -> **Output JSON won't contain** summary

5. **fromJson and toJson**

নির্দিষ্ট ফিল্ডগুলোর জন্য কাস্টম সিরিয়ালাইজেশন ও ডেসিরিয়ালাইজেশন লজিক প্রয়োগ করতে ব্যবহৃত হয়।

```dart
@JsonSerializable()
class GeoPoint {
  final double latitude;
  final double longitude;

  GeoPoint({required this.latitude, required this.longitude});

  // Custom conversion methods
  static GeoPoint fromJson(Map<String, dynamic> json) {
    return GeoPoint(
      latitude: (json['lat'] as num).toDouble(),
      longitude: (json['lng'] as num).toDouble(),
    );
  }

  static Map<String, dynamic> toJson(GeoPoint point) {
    return {
      'lat': point.latitude,
      'lng': point.longitude,
    };
  }
}

@JsonSerializable()
class Location {
  final String address;

  @JsonKey(
    fromJson: GeoPoint.fromJson,
    toJson: GeoPoint.toJson,
  )
  final GeoPoint coordinates;

  Location({required this.address, required this.coordinates});

  factory Location.fromJson(Map<String, dynamic> json) =>
      _$LocationFromJson(json);

  Map<String, dynamic> toJson() => _$LocationToJson(this);
}

```

6. **required**

ডেসিরিয়ালাইজেশনের সময় একটি মান উপস্থিত আছে কিনা তা রানটাইমে যাচাই করার জন্য চেক যোগ করে।

```dart
@JsonSerializable()
class Config {
  @JsonKey(required: true)
  final String apiKey;

  Config({required this.apiKey});

  factory Config.fromJson(Map<String, dynamic> json) => _$ConfigFromJson(json);
  Map<String, dynamic> toJson() => _$ConfigToJson(this);
}
```

If **apiKey** is missing during deserialization, a runtime error is thrown.

7. **disallowNullValue**

JSON-এ কোনো কী থাকলেও তার মান null হলে একটি ত্রুটি সৃষ্টি করে।

```dart
@JsonSerializable()
class Profile {
  @JsonKey(disallowNullValue: true)
  final String username;

  Profile({required this.username});

  factory Profile.fromJson(Map<String, dynamic> json) => _$ProfileFromJson(json);
  Map<String, dynamic> toJson() => _$ProfileToJson(this);
}

```

যদি { "username": null } থাকে, তাহলে এটি রানটাইমে একটি ত্রুটি তৈরি করবে।

## @JsonKey কার্যকরভাবে ব্যবহার করার টিপস

1. **কনভার্টারগুলো সরল রাখুন:** কনভার্টার ফাংশনগুলো হওয়া উচিত পিউর, অর্থাৎ শুধুমাত্র রূপান্তরের কাজ করবে এবং কোনো পার্শ্বপ্রতিক্রিয়া (side effects) থাকবে না।
2. **কাস্টম কনভার্টার ডকুমেন্ট করুন:** প্রত্যাশিত ইনপুট ফরম্যাট এবং বিশেষ হ্যান্ডলিং সংক্রান্ত বিস্তারিত মন্তব্য যুক্ত করুন।
3. **ব্যতিক্রমের জন্য ফিল্ড-লেভেল অ্যানোটেশন ব্যবহার করুন:** সাধারণ কনফিগারেশনগুলো ক্লাস লেভেলে @JsonSerializable প্যারামিটার দিয়ে সেট করুন, আর @JsonKey ব্যবহার করুন শুধু ব্যতিক্রমী ক্ষেত্রে।
4. **এজ কেসগুলো পরীক্ষা করুন:** বিশেষ করে null, খালি মান বা ভুল ফরম্যাটের ইনপুট ব্যবহার করে আপনার কাস্টম কনভার্টার ফাংশনগুলো ভালোভাবে পরীক্ষা করুন।
5. **নামকরণের ধারাবাহিকতা বজায় রাখুন:** যখন name: ব্যবহার করে কাস্টম ফিল্ড নাম দেন, তখন মডেলগুলোতে একটি সুশৃঙ্খল প্যাটার্ন অনুসরণ করার চেষ্টা করুন।
6. **ভ্যালিডেশন বিবেচনা করুন:** কাস্টম fromJson কনভার্টারগুলোর মধ্যে ডেটার সঠিকতা যাচাই করুন এবং অবৈধ ডেটা পেলে এক্সসেপশন ছুঁড়ে দিন।

## Tips to Keep in Mind

1. **`dart:convert` থেকে `jsonDecode()` এবং `jsonEncode()` ব্যবহার করুন**
   - `jsonDecode()` একটি JSON স্ট্রিংকে Map এ রূপান্তর করে।
   - `jsonEncode()` একটি Map বা অবজেক্টকে JSON স্ট্রিংয়ে রূপান্তর করে।
2. **`fromJson()` এর জন্য সর্বদা factory কনস্ট্রাক্টর ব্যবহার করুন**
   - এটি JSON ম্যাপ থেকে নতুন একটি ইনস্ট্যান্স তৈরি করতে সাহায্য করে।
3. **বড় বা নেস্টেড মডেলের জন্য আলাদা ক্লাস তৈরি করুন**
   - JSON-এর প্রতিটি নেস্টেড স্ট্রাকচারের জন্য আলাদা Dart মডেল ক্লাস থাকা ভালো।
4. **অনুপস্থিত ফিল্ড বা null মান সঠিকভাবে হ্যান্ডেল করুন**
   - প্রয়োজন অনুযায়ী `required` কীওয়ার্ড ব্যবহার করুন বা ডিফল্ট মান দিন।
5. **স্বয়ংক্রিয় কোড জেনারেশনের জন্য উপযুক্ত টুল ব্যবহার করুন**
   - বড় প্রজেক্টের ক্ষেত্রে, `json_serializable` এবং `build_runner` প্যাকেজ ব্যবহার করে `fromJson()` এবং `toJson()` মেথডগুলো স্বয়ংক্রিয়ভাবে জেনারেট করতে পারেন।

## উপসংহার

Dart-এ JSON serialization খুবই গুরুত্বপূর্ণ কারণ এটি JSON ডেটাকে Dart অবজেক্টে এবং Dart অবজেক্টকে আবার JSON ডেটায় রূপান্তর করে। এই প্রক্রিয়া আয়ত্ত করলে API বা অন্যান্য ডেটা সোর্স থেকে ডেটা সহজে নিয়ে ব্যবহার করা যায় এবং ডেটা সুন্দর ও সুষ্ঠুভাবে পরিচালনা করা সহজ হয়।