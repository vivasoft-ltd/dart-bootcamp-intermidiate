**JSON (JavaScript Object Notation)** is a lightweight data-interchange format that's easy for humans to read and write, and easy for machines to parse and generate.**JSON serialization in Dart** refers to the process of converting a Dart object (like a class instance) into a JSON string, and **deserialization** is the reverse—converting a JSON string back into a Dart object.

## Why do we need JSON serialization?

Dart is a strongly typed, object-oriented language, while **JSON** is a lightweight, text-based format used for data exchange (e.g., from an API, local storage, or a file).

When working with APIs or storing data in local databases like SharedPreferences or Hive, we often **receive or send data in JSON format**. To use that data in Dart, we need to convert it into Dart objects (deserialization), and to send Dart data as JSON, we need serialization.

## Real-World Example

Let's say you receive below JSON form an API:

```dart
{
  "name": "Charlie",
  "age": 28
}
```

You want to map this to a Dart class:

```dart
class Person{
  final String name;
  final int age;

  Person({required this.name, required this.age});
}
```

## Deserialization: JSON -> Dart Object

```dart
factory Person.fromJson(Map<String, dynamic> json) {
  return Person(
    name: json['name'],
    age: json['age'],
  );
}
```

## Serialization: Dart Object -> JSON

```dart
Map<String, dynamic> toJson(Person person) {
  return {
    'name': person.name,
    'age': person.age,
  };
}
```

Now the whole class would look like below:

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

## Approaches to JSON serialization in Dart

## Manual Serialization

Dart provides built-in support for JSON conversion through the **dart:convert** library. This allows you to work with JSON data without additional dependencies.

## Converting between JSON and Dart objects

1. **JSON to Dart:**

    - Use **jsonDecode()** to convert a JSON string to Dart objects
    - The result will be a combination of Lists, Maps, and primitive types

2. **Dart to JSON:**
    - Use **jsonEncode()** to convert Dart objects to a JSON string
    - Only certain types are directly encodable (numbers, strings, booleans, null, lists, and maps)

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

## Code Generation With json_serializable

For more complex models the **json_serializable** package is the preferred solution
At first set up your **pubspec.yaml** with the necessary dependencies:

````dart
dependencies:
  json_annotation: ^4.8.1

dev_dependencies:
  build_runner: ^2.4.6
  json_serializable: ^6.7.1
``


Then write your model class like below :

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

**Run the code generator**

```dart
  dart pub run build_runner build
```

This generates all the boilerplate serialization code automatically.

## Available Parameters in @JsonSerializabel()

1. **createFactory**

**Default: true**
If false, the factory fromJson method will not be generated.

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
If false, the toJson() method will not be generated.

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
For nested objects, this ensures **.toJson()** is called on each nested object explicitly.

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

Without **explicitToJson: true**, the nested **Customer.toJson()** might not be called properly.

4. **includeIfNull**

**Default: true**
Whether null values should be included in the output JSON.

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
   Automatically renames Dart fields to a different casing in JSON. Options:

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

Allows Map<dynamic, dynamic> instead of Map<String, dynamic>.

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

Adds runtime type checking during deserialization (throws detailed errors on type mismatch).

```dart
@JsonSerializable(checked: true)
class SafeModel {
  final int age;

  SafeModel({required this.age});

  factory SafeModel.fromJson(Map<String, dynamic> json) => _$SafeModelFromJson(json);
  Map<String, dynamic> toJson() => _$SafeModelToJson(this);
}
```
## Tips for Using @JsonSerializable  Effectively

1. Use **explicitToJson: true** for Nested Models
2. Use **fieldRename** to Match API Field Naming Conventions
3. Use **includeIfNull: false** to Clean Up JSON Output
5. Disable Auto-Generated Methods (**createFactory: false**,**createToJson: false**) When Needed
6. Use **anyMap: true** for Flexible Data Sources
7. Always regenerate code after making changes (dart pub run build_runner build).
8. Don't forget **part 'model.g.dart';** at the top of your class file.

## What is @JsonKey Annotation in Dart (json_serializable)

The @JsonKey annotation is a powerful tool in the json_serializable package that gives you fine-grained control over how individual fields are serialized and deserialized. Let's explore all its capabilities with examples:

1. **name**

Renames the field in the JSON representation.

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

Excludes the field from both serialization and deserialization.

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

Now isLoggedIn field will not appear in JSON output and won't be read from input.

3. **defaultValue**

Provides a default value if the field is missing in JSON input.

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

Controls whether the field is included in the output JSON if it is null.

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

Used for custom serialization/deserialization logic for specific fields.

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

Adds a runtime check to ensure a value is present during deserialization

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

Throws an error if the key is present in the JSON with a null value.

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

if { "username": null } will throw a runtime error.

## Tips for Using @JsonKey Effectively

1. **Keep converters simple:** Converter functions should be pure, handling only the conversion logic without side effects.
2. **Document custom converters:** Add comments explaining the expected format and any special handling.
3. **Use field-level annotations for exceptions:** Set most common configurations at the class level using @JsonSerializable parameters, and use @JsonKey only for exceptions.
4. **Test edge cases:** Especially test your custom converter functions with edge cases like null, empty values, or malformed input.
5. **Consistent naming:** When using custom field names with name:, try to follow a consistent pattern across your models.
6. **Consider validation:** You can perform validation in custom fromJson converters, throwing exceptions for invalid data.




## Tips to Keep in Mind

1. **Use `jsonDecode()` and `jsonEncode()` from `dart:convert`**
    - `jsonDecode()` converts a JSON string into a Map.
    - `jsonEncode()` converts a Map or object into a JSON string.

2. **Always use a factory constructor for `fromJson()`**
    - It allows returning a new instance based on the JSON map.

3. **For large or nested models, create separate classes**
    - Each nested structure in JSON should ideally have its own Dart model class.

4. **Handle missing fields or nulls properly**
    - Use the `required` keyword or provide default values where needed.

5. **Consider using tools for automatic code generation**
    - For bigger projects, you can automate `fromJson()` and `toJson()` using packages like `json_serializable` and `build_runner`.

---

## Coclusion

JSON serialization is a crucial concept in Dart when working with structured data. It bridges the gap between raw JSON and usable Dart objects. Mastering this will make it much easier to work with external data sources and organize your application’s data effectively.