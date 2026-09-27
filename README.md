# Dart & Flutter Interview Questions — Complete Guide

> **Note:** Dart version references are included where behavior differs across versions. Enhanced enums require Dart 2.17+, `Isolate.run` requires 2.19+, and pattern matching requires 3.0+.

---

## Table of Contents

1. [What is Dart, and why is it used by Flutter?](#1-what-is-dart-and-why-is-it-used-by-flutter)
2. [Difference between var, final, and const](#2-difference-between-var-final-and-const)
3. [Difference between final and const](#3-difference-between-final-and-const)
4. [What is null safety in Dart?](#4-what-is-null-safety-in-dart)
5. [Difference between ?, !, and ??](#5-difference-between---and-)
6. [Difference between late and nullable variables](#6-difference-between-late-and-nullable-variables)
7. [Named and positional parameters](#7-named-and-positional-parameters)
8. [Optional parameters](#8-optional-parameters)
9. [Required parameters](#9-required-parameters)
10. [Difference between List, Set, and Map](#10-difference-between-list-set-and-map)
11. [Difference between where(), map(), and forEach()](#11-difference-between-where-map-and-foreach)
12. [Higher-order functions](#12-higher-order-functions)
13. [Anonymous functions](#13-anonymous-functions)
14. [Closures](#14-closures)
15. [Extensions](#15-extensions)
16. [Mixins](#16-mixins)
17. [Difference between extends, implements, and with](#17-difference-between-extends-implements-and-with)
18. [Abstract classes](#18-abstract-classes)
19. [Factory constructors](#19-factory-constructors)
20. [Enums](#20-enums)
21. [Generics](#21-generics)
22. [Exception handling](#22-exception-handling)
23. [Difference between throw and rethrow](#23-difference-between-throw-and-rethrow)
24. [Futures](#24-futures)
25. [async/await](#25-asyncawait)
26. [Difference between Future, Stream, and Isolate](#26-difference-between-future-stream-and-isolate)
27. [StreamController](#27-streamcontroller)
28. [Event loop](#28-event-loop)
29. [Isolates](#29-isolates)
30. [When to use an isolate](#30-when-to-use-an-isolate)

---

<div style="page-break-after: always;"></div>

# 1. What is Dart, and why is it used by Flutter?

Dart is an open-source, general-purpose programming language developed by Google. It is a modern, object-oriented language designed to be approachable, portable, and productive for building high-quality applications across multiple platforms.

### What is Dart?

Dart is a client-optimized language specifically designed for building user interfaces. It is free and open-source, with its repository publicly available. Key characteristics include:

*   **Multi-Platform Support**: Dart can compile to native machine code for mobile (ARM, x64) and desktop, as well as to JavaScript and WebAssembly for the web.
*   **Modern Features**: It offers modern language features like sound null safety, async-await for asynchronous programming, and pattern matching to help write safe and concise code.
*   **Productive Development**: The language supports "hot reload," which allows developers to see changes instantly in a running app without losing state, significantly speeding up the development cycle.

### Why Does Flutter Use Dart?

Flutter, Google's UI toolkit for building natively compiled applications, chose Dart because it perfectly matched the requirements for building a modern UI framework. The main reasons include:

*   **Optimized for UI Development**: Dart is designed for the technical requirements of client development. It handles asynchronous operations efficiently and uses an isolate-based concurrency model instead of shared-memory threads, which avoids common issues like data races and UI jank.
*   **High Performance Compilation**: Dart's compiler technology is crucial for Flutter. It uses **Just-In-Time (JIT) compilation** during development for fast iteration (hot reload) and **Ahead-Of-Time (AOT) compilation** for release builds, which creates fast, predictable native machine code with quick startup times.
*   **Unified Codebase**: Since everything in Flutter—including layout, UI, and logic—is written in Dart, developers don't need to switch between multiple languages or use separate declarative layout languages like XML. This simplifies tooling and development.
*   **Productivity and Safety**: Dart's strong typing and rich static analysis help catch errors before running the app. Its mature async-await syntax makes handling event-driven UI code clean and manageable.

In short, Dart provides the ideal combination of performance, productivity, and portability that Flutter needs to deliver a consistent, high-quality development experience across all platforms.

**Key point:** Dart was purpose-built for client-side UI work, and its dual JIT/AOT compilation is what makes Flutter's hot reload and native performance possible.

---

<div style="page-break-after: always;"></div>

# 2. Difference between var, final, and const

In Dart, `var`, `final`, and `const` are all used to declare variables, but they differ in **mutability**, **when the value is determined**, and **compile-time guarantees**.

### `var` — Mutable, Type Inferred

- **Mutability**: The variable **can be reassigned** after initialization.
- **Type**: The type is **inferred** from the assigned value (but fixed once inferred).
- **When resolved**: At **runtime**.

```dart
var name = 'Alice';
name = 'Bob'; // allowed, value can change
// name = 42; // error: type is already inferred as String
```

### `final` — Immutable Reference, Runtime Value

- **Mutability**: The variable **cannot be reassigned** after initialization.
- **Type**: Type is inferred (or can be explicitly declared).
- **When resolved**: At **runtime** — the value is determined when the code executes.
- **Use case**: When you need a value that won't change but isn't known until runtime (e.g., from user input, API, or `DateTime.now()`).

```dart
final time = DateTime.now(); // value set at runtime, then locked
// time = DateTime.now();    // error: can't reassign

final list = [1, 2, 3];
list.add(4); // allowed — the list object itself is mutable
```

### `const` — Immutable, Compile-Time Constant

- **Mutability**: The variable **cannot be reassigned**.
- **Type**: Type is inferred (or can be explicitly declared).
- **When resolved**: At **compile time** — the value must be known before the program runs.
- **Use case**: For true constants like `const pi = 3.14;` or fixed widget configurations.
- Also creates **deeply immutable** objects (the object and its contents can't change).

```dart
const pi = 3.14;
// const now = DateTime.now(); // error: not a compile-time constant

const list = [1, 2, 3];
// list.add(4); // error: const collections are deeply immutable
```

### Quick Comparison

| Feature | `var` | `final` | `const` |
|---|---|---|---|
| Reassignable? | Yes | No | No |
| Value known at | Runtime | Runtime | Compile time |
| Deeply immutable? | No | No | Yes |
| Type inferred? | Yes | Yes | Yes |

### Rule of Thumb

- Use **`const`** whenever the value is known at compile time — it's the most efficient.
- Use **`final`** when the value is set once but only known at runtime.
- Use **`var`** for values that need to change over time.

> In Flutter, `const` is especially powerful: `const` widgets are **canonicalized and reused**, which can significantly improve rebuild performance.

**Key point:** `var` = mutable and inferred, `final` = set once at runtime, `const` = set once at compile time and deeply immutable.

**See also:** [#3 — Difference between final and const](#3-difference-between-final-and-const)

---

<div style="page-break-after: always;"></div>

# 3. Difference between final and const

Here's a focused breakdown of the differences between `final` and `const` in Dart.

### The Core Difference

| | `final` | `const` |
|---|---|---|
| **When value is set** | Runtime | Compile time |
| **Reassignable?** | No | No |
| **Deeply immutable?** | No (object can still mutate) | Yes (fully immutable) |
| **Can use runtime values?** | Yes | No |
| **Can be instance variables?** | Yes | No (must be `static const`) |

### 1. Runtime vs. Compile Time

This is the most important distinction.

```dart
final time = DateTime.now(); // OK — value resolved at runtime
// const time = DateTime.now(); // Error — not a compile-time constant
```

`const` requires the value to be **fully known before the program runs**. `final` just means "assigned once," and that assignment can happen while the app is running.

```dart
final a = 5;         // fine
const b = 5;         // fine
const c = 5 * 2;     // fine — evaluated at compile time
final d = 5 * 2;     // fine — but evaluated at runtime
```

### 2. Shallow vs. Deep Immutability

This trips up a lot of developers.

```dart
final list = [1, 2, 3];
list.add(4); // Allowed! The list object itself is mutable.

const list2 = [1, 2, 3];
list2.add(4); // Error! const collections are deeply immutable.
```

- `final` locks the **variable** (you can't point it to a new object), but the **object it points to** can still change.
- `const` locks the **variable and the entire object graph** — nothing inside can ever change.

### 3. Instance Variables

```dart
class Person {
  final String name;      // allowed
  // const String name;   // not allowed as an instance field

  Person(this.name);
}
```

`const` variables must be **top-level** or **static** — because their value must exist at compile time, independent of any instance.

```dart
class Circle {
  static const pi = 3.14; // must be static
}
```

### 4. Canonicalization (Flutter-Specific)

`const` values are **canonicalized** — Dart creates only **one instance** of an identical `const` object, no matter how many times you write it.

```dart
const p1 = Point(1, 2);
const p2 = Point(1, 2);
print(identical(p1, p2)); // true — same object in memory!

final f1 = Point(1, 2);
final f2 = Point(1, 2);
print(identical(f1, f2)); // false — two separate objects
```

This is why `const` widgets in Flutter are so valuable — Flutter can skip rebuilding them entirely.

### Summary

- Use **`const`** when the value is known at **compile time** and should be **fully immutable** — best performance, canonicalized.
- Use **`final`** when the value is set **once at runtime** but you don't need deep immutability.
- Both prevent **reassignment**; only `const` guarantees **deep immutability** and **compile-time evaluation**.

> A good mental model: **`const` is a stricter, compile-time version of `final`.** If something can be `const`, prefer it — but `final` is the right choice when the value depends on runtime data.

**Key point:** `final` = runtime-set, shallow-immutable. `const` = compile-time-set, deeply immutable, and canonicalized.

**See also:** [#2 — Difference between var, final, and const](#2-difference-between-var-final-and-const)

---

<div style="page-break-after: always;"></div>

# 4. What is null safety in Dart?

**Null safety** in Dart means a variable **cannot contain `null` unless you explicitly allow it**. It shifts null errors from **runtime** to **compile time**.

### Core Idea

By default, all types are **non-nullable**:

```dart
int age = 25;   // OK
age = null;     // Compile error
```

To allow null, add `?`:

```dart
int? age = null; // OK — nullable type
```

### Key Features

- **`?` (nullable type)** — declares a variable may be null.

  ```dart
  String? name;
  ```

- **`!` (null assertion)** — tells the compiler "this is not null."

  ```dart
  print(name!.length); // crashes if name is null
  ```

- **`?.` (null-aware access)** — safely accesses members.

  ```dart
  print(name?.length); // returns null instead of crashing
  ```

- **`??` (null-coalescing)** — provides a fallback value.

  ```dart
  print(name ?? 'Guest');
  ```

- **`late`** — promises the variable will be initialized before use.

  ```dart
  late String greeting;
  ```

### Why It Matters

- Catches null errors **at compile time**, not runtime.
- Makes code **safer and more predictable**.
- Improves **performance** (no null checks needed at runtime for non-nullable types).

> In short: **non-nullable by default, nullable only when you say so.**

**Key point:** Null safety moves null errors from runtime to compile time, making Dart apps safer by default.

**See also:** [#5 — Difference between ?, !, and ??](#5-difference-between---and-), [#6 — late vs nullable](#6-difference-between-late-and-nullable-variables)

---

<div style="page-break-after: always;"></div>

# 5. Difference between ?, !, and ??

In **Dart**, these three operators all deal with nullability but serve very different purposes.

| Operator | Name | Purpose | Example |
|----------|------|---------|---------|
| `?` (on a type) | Nullable type modifier | Declares that a variable may hold `null` | `String? name;` |
| `?.` | Null-aware access | Safely accesses a member; short-circuits to `null` if the target is null | `user?.name` → null if user is null, else `user.name` |
| `!` | Null assertion | Forces non-null; throws at runtime if the value is null | `user!.name` → throws if user is null |
| `??` | Null coalescing | Provides a default value when the left side is null | `user?.name ?? "Guest"` → "Guest" if name is null |

> **Common confusion:** The `?` in `String?` (type annotation) is **not** the same as the `?.` operator. They look similar but do completely different things.

### Quick Examples

```dart
String? name; // nullable type

name?.length;        // ?. — returns null, doesn't crash
name!.length;        // !  — crashes at runtime if name is null
name ?? "Unknown";   // ?? — uses "Unknown" if name is null

// Chaining safe access with a fallback
user?.profile?.avatar ?? "default.png"
```

### When to Use

- `?` → When a value *might* legitimately be null (declaring the type).
- `?.` → When accessing members on something that might be null.
- `!` → When you're **certain** it's non-null (risky, use sparingly).
- `??` → When you want a fallback for a null case.

**Key point:** `?` declares nullability, `?.` safely navigates it, `!` asserts non-nullness, and `??` provides a fallback.

**See also:** [#4 — Null safety](#4-what-is-null-safety-in-dart), [#6 — late vs nullable](#6-difference-between-late-and-nullable-variables)

---

<div style="page-break-after: always;"></div>

# 6. Difference between late and nullable variables

In **Dart**, `late` and nullable (`?`) are two different tools for two different problems. The real distinction is about **initialization timing**, not just nullability.

| Aspect | `late` | Nullable `?` |
|--------|--------|--------------|
| **Can hold null?** | Not by default (but `late String? x` is possible) | Yes |
| **Initialized?** | Later — but promised before first use | Can remain null indefinitely |
| **Type** | Non-nullable (unless explicitly nullable) | Nullable |
| **Access** | Direct — no `?` needed | Requires `?.` or `??` operators |
| **Error if uninitialized** | `LateInitializationError` at runtime | Compile-time error if accessed without null check |

### Quick Examples

```dart
// late - must initialize before using
late String userName;  // No value yet, but WILL have one
userName = "Vinayak";  // Must set before use
print(userName);       // Safe, no null check needed

// nullable - can be null forever
String? userName;      // Might be null, might not be
print(userName);       // ERROR: must check/handle null
print(userName?.length); // OK: using null-aware operator
```

### Key Differences

```dart
// late - commit to a value (just delayed)
late final userId = fetchUserIdFromDB(); // Set once, used multiple times
print(userId.toUpperCase()); // Direct access, no operators needed

// nullable - genuinely unsure if there's a value
String? userId = fetchUserIdFromDatabase(); // Might be null
print(userId?.toUpperCase()); // Must handle null possibility
```

### When to Use

- `late` → Value exists but isn't ready yet (async init, dependency injection, lazy initialization).
- `?` → Value might legitimately be absent (optional fields, search results).

### Runtime Error Difference

```dart
late String x; // Throws LateInitializationError if accessed before assignment
String? y;     // y is null until assigned, no crash
```

**Key point:** `late` promises "it will be set before use"; `?` says "it might be null forever."

**See also:** [#4 — Null safety](#4-what-is-null-safety-in-dart)

---

<div style="page-break-after: always;"></div>

# 7. Named and positional parameters

In **Dart**:

| Type | Syntax | Required? | Called as |
|------|--------|-----------|-----------|
| **Positional** | `(type param)` | Yes (unless default) | `func(value)` |
| **Named** | `({type param})` | Optional if it has a default or is nullable; otherwise `required` | `func(name: value)` |

### Quick Examples

```dart
// Positional - order matters
void greet(String name, int age) {
  print("$name is $age");
}
greet("Vinayak", 25);  // Must pass in order
greet(25, "Vinayak");  // Wrong order, breaks

// Named - order doesn't matter
void greet({String name = "Guest", int age = 0}) {
  print("$name is $age");
}
greet(name: "Vinayak", age: 25);
greet(age: 25, name: "Vinayak");  // Same result
greet();                           // Uses defaults
```

### Mix Both

```dart
void createUser(String id, {String? email, required String role}) {
  // id is positional (required)
  // email is named (optional, nullable)
  // role is named (required)
}

createUser("123", role: "admin");
createUser("123", email: "test@example.com", role: "user");
```

### Defaults and Required

```dart
void build({
  String title = "Default",  // Optional, has default
  required String content,   // Named but required
}) {}

build(content: "Hello");  // Must pass content
build(title: "App", content: "Hello");
```

> **Note:** A named parameter without a default and without `?` must be marked `required`, or the code won't compile.

### Best Practice

- **Positional** → Few, required params (1–2).
- **Named** → Optional params or when there are many params (readability).

**Key point:** Positional params are ordered and required; named params are unordered and can be optional or required.

---

<div style="page-break-after: always;"></div>

# 8. Optional parameters

In **Dart**, optional parameters are parameters you **don't have to provide** when calling a function.

### Two Types

```dart
// 1. Positional optional - with defaults, in brackets []
void greet(String name, [String greeting = "Hi"]) {
  print("$greeting, $name");
}
greet("Vinayak");              // Uses default "Hi"
greet("Vinayak", "Hello");     // Overrides default

// 2. Named optional - in braces {}, with defaults
void greet({String name = "Guest", String greeting = "Hi"}) {
  print("$greeting, $name");
}
greet();                        // Uses both defaults
greet(name: "Vinayak");         // Uses default greeting
greet(greeting: "Hey", name: "Vinayak");  // Overrides both
```

### Quick Reference

```dart
// Positional optional with default
void func(String required, [String optional = "default"]) {}
func("a");           // optional = "default"
func("a", "b");      // optional = "b"

// Named optional with default
void func({String required = "default", int age = 0}) {}
func();              // Both use defaults
func(required: "x"); // age = 0

// Truly optional (nullable) - no default
void func({String? email}) {}
func();              // email = null
func(email: "test@example.com");
```

### Key Difference

- **Optional with default** → Always has a value.
- **Nullable optional** → Can be null.

**Key point:** Optional parameters can be positional (`[]`) or named (`{}`), and they can have defaults or be nullable.

**See also:** [#7 — Named and positional parameters](#7-named-and-positional-parameters), [#9 — Required parameters](#9-required-parameters)

---

<div style="page-break-after: always;"></div>

# 9. Required parameters

In **Dart**, required parameters are parameters **you must provide** when calling a function.

### Positional Required

```dart
void greet(String name, int age) {
  // Both name and age are required
}
greet("Vinayak", 25);  // Must pass both
greet("Vinayak");      // Error: age is missing
```

### Named Required

```dart
void greet({required String name, required int age}) {
  // Both are required, but order doesn't matter
}
greet(name: "Vinayak", age: 25);
greet(name: "Vinayak");            // Error: age is required
```

### Mix

```dart
void createUser(String id, {required String role, String? email}) {
  // id is positional required
  // role is named required
  // email is named optional
}
createUser("123", role: "admin");
createUser("123");  // Error: role is required
```

**Key point:** The compiler enforces required parameters — skipping one is a compile-time error.

**See also:** [#7 — Named and positional parameters](#7-named-and-positional-parameters), [#8 — Optional parameters](#8-optional-parameters)

---

<div style="page-break-after: always;"></div>

# 10. Difference between List, Set, and Map

In **Dart**:

| Type | Order | Duplicates | Access | Use case |
|------|-------|-----------|--------|----------|
| `List<T>` | Ordered | Allowed | By index | Ordered collection |
| `Set<T>` | Insertion-ordered | Not allowed | No index | Unique values only |
| `Map<K,V>` | Insertion-ordered | No duplicate keys | By key | Key-value pairs |

> **Note:** Dart's default `Set` is a `LinkedHashSet`, which preserves insertion order. It's not truly "unordered."

### Examples

```dart
// LIST - ordered, indexed, duplicates
List<int> numbers = [1, 2, 3, 2, 1];
print(numbers[0]);      // 1 (access by index)
print(numbers.length);  // 5 (duplicates kept)

// SET - unique, insertion-ordered, no index
Set<int> unique = {1, 2, 3, 2, 1};
print(unique);          // {1, 2, 3} (duplicates removed)
print(unique.length);   // 3
// print(unique[0]);    // Error - no index access

// MAP - key-value pairs
Map<String, int> ages = {"Alice": 30, "Bob": 25};
print(ages["Alice"]);   // 30 (access by key)
print(ages.length);     // 2
```

### Operations

```dart
// List
List<String> names = ["Alice", "Bob"];
names.add("Charlie");
names.remove("Alice");
names.contains("Bob");    // true

// Set
Set<int> nums = {1, 2, 3};
nums.add(4);
nums.remove(2);
nums.contains(3);         // true
nums.union({3, 4, 5});    // {1, 2, 3, 4, 5}

// Map
Map<String, String> user = {"name": "Vinayak", "city": "Mumbai"};
user["age"] = "25";       // Add
user.remove("city");
user.keys;                // ["name", "age"]
user.values;              // ["Vinayak", "25"]
```

### When to Use

- `List` → Ordered collection, need indexing.
- `Set` → Only unique values, fast lookup.
- `Map` → Key-value associations.

**Key point:** `List` allows duplicates and index access; `Set` enforces uniqueness; `Map` stores key-value pairs.

---

<div style="page-break-after: always;"></div>

# 11. Difference between where(), map(), and forEach()

In **Dart** (on Lists, Iterables):

| Method | Purpose | Returns | Use case |
|--------|---------|---------|----------|
| `where()` | Filter items based on condition | Filtered iterable | Keep only items that match |
| `map()` | Transform each item | New iterable with transformed items | Convert each item to something else |
| `forEach()` | Do something with each item | Nothing (void) | Side effects only (print, update UI) |

### Quick Examples

```dart
List<int> numbers = [1, 2, 3, 4, 5];

// where() - filter
List<int> evens = numbers.where((n) => n % 2 == 0).toList();
// evens = [2, 4]

// map() - transform
List<String> strings = numbers.map((n) => "num: $n").toList();
// strings = ["num: 1", "num: 2", "num: 3", ...]

// forEach() - do action on each (no return)
numbers.forEach((n) => print(n));
// Prints: 1 2 3 4 5
// Returns: null
```

> **Note:** `where()` and `map()` return lazy iterables. Call `.toList()` if you need an eager list.

### Key Difference

- `where()` → Selects items (returns filtered iterable).
- `map()` → Changes items (returns new iterable).
- `forEach()` → Acts on items (returns nothing).

**Key point:** `where` filters, `map` transforms, `forEach` performs side effects — and only the first two return new iterables.

---

<div style="page-break-after: always;"></div>

# 12. Higher-order functions

In **Dart**, higher-order functions are functions that **take functions as parameters** or **return functions**.

### Two Types

```dart
// 1. Function as parameter
void executeAction(Function action) {
  action(); // Call the function passed in
}
executeAction(() => print("Hello"));  // Pass a function

// 2. Return a function
Function greetGenerator(String greeting) {
  return (String name) => "$greeting, $name";
}
final greet = greetGenerator("Hi");
print(greet("Vinayak"));  // "Hi, Vinayak"
```

### Real Examples

```dart
// Higher-order: takes a function
List<int> filterNumbers(List<int> nums, Function filter) {
  return nums.where((n) => filter(n) as bool).toList();
}
final evens = filterNumbers([1, 2, 3, 4], (n) => n % 2 == 0);

// Higher-order: returns a function
Function multiplier(int factor) {
  return (int num) => num * factor;
}
final double = multiplier(2);
print(double(5));  // 10

// Built-in higher-order: map, where, forEach
numbers.map((n) => n * 2);  // Takes a function parameter
```

**Key point:** Functions are first-class citizens in Dart — they can be passed around like any other value.

**See also:** [#13 — Anonymous functions](#13-anonymous-functions), [#14 — Closures](#14-closures)

---

<div style="page-break-after: always;"></div>

# 13. Anonymous functions

In **Dart**, anonymous functions are functions **without a name**.

### Syntax

```dart
// Full anonymous function
(parameters) {
  // body
};

// Arrow syntax (one-liner)
(parameters) => expression;
```

### Examples

```dart
// Anonymous function assigned to variable
final add = (int a, int b) => a + b;
print(add(2, 3));  // 5

// Passed to higher-order function
numbers.map((n) => n * 2);
numbers.where((n) => n > 5);
numbers.forEach((n) => print(n));

// Multi-line
final greet = (String name) {
  final message = "Hello, $name";
  return message;
};
```

**Key point:** Anonymous functions have no name and are usually passed directly to other functions or stored in variables.

**See also:** [#12 — Higher-order functions](#12-higher-order-functions), [#14 — Closures](#14-closures)

---

<div style="page-break-after: always;"></div>

# 14. Closures

In **Dart**, closures are functions that **capture and remember variables from their enclosing scope**, even after that scope is gone.

### Simple Example

```dart
Function makeCounter() {
  int count = 0;  // Local variable
  return () => ++count;  // Function captures count
}

final counter = makeCounter();
print(counter());  // 1
print(counter());  // 2
print(counter());  // 3
// count still exists inside the closure!
```

### How It Works

```dart
void outer() {
  String message = "Hello";  // Captured by closure

  void inner() {
    print(message);  // Accesses outer's message
  }

  inner();  // "Hello"
  return inner;  // Closure remembers message
}

final func = outer();
func();  // Still prints "Hello" even though outer() finished
```

**Key point:** A closure "closes over" (remembers) variables from where it was defined, not where it's called.

**See also:** [#12 — Higher-order functions](#12-higher-order-functions), [#13 — Anonymous functions](#13-anonymous-functions)

---

<div style="page-break-after: always;"></div>

# 15. Extensions

In **Dart**, extensions let you **add methods to existing classes** without modifying the original class.

### Syntax

```dart
extension NameOnType on ClassName {
  // Add methods here
}
```

### Examples

```dart
// Add method to String
extension StringExtension on String {
  bool get isEmail => contains("@");
  String capitalize() => this[0].toUpperCase() + substring(1);
}

"test@example.com".isEmail;  // true
"hello".capitalize();         // "Hello"

// Add method to int
extension IntExtension on int {
  bool get isEven => this % 2 == 0;
  Duration toDays() => Duration(days: this);
}

4.isEven;      // true
5.toDays();    // Duration
```

**Key point:** Extensions add functionality to any class (built-in or custom) without inheritance or modification.

---

<div style="page-break-after: always;"></div>

# 16. Mixins

In **Dart**, mixins let you **reuse code across multiple classes** without a strict inheritance hierarchy.

### Syntax

```dart
mixin MixinName {
  // Methods to mix in
}

class ClassName with MixinName {
  // Gets all methods from mixin
}
```

### Examples

```dart
// Define mixin
mixin Loggable {
  void log(String message) => print("[LOG] $message");
}

// Mix into classes
class User with Loggable {
  String name = "Vinayak";

  void display() {
    log("Displaying user: $name");  // From mixin
  }
}

class Product with Loggable {
  String title = "Laptop";

  void display() {
    log("Showing product: $title");  // Same mixin
  }
}

User().display();      // "[LOG] Displaying user: Vinayak"
Product().display();   // "[LOG] Showing product: Laptop"
```

### Multiple Mixins

```dart
mixin Loggable {
  void log(String msg) => print("[LOG] $msg");
}

mixin Debuggable {
  void debug(String msg) => print("[DEBUG] $msg");
}

class Service with Loggable, Debuggable {
  void start() {
    log("Service started");
    debug("Debugging enabled");
  }
}
```

### Key Difference from Inheritance

- **Inheritance** → "is-a" relationship (class `extends`).
- **Mixin** → Adds behavior to a class without forcing an inheritance hierarchy. The class still "is-a" thing with that behavior, but the code comes from multiple sources.

**Key point:** Mixins let you share behavior across unrelated classes without forcing an inheritance chain.

**See also:** [#17 — extends, implements, with](#17-difference-between-extends-implements-and-with)

---

<div style="page-break-after: always;"></div>

# 17. Difference between extends, implements, and with

In **Dart**:

| Keyword | Purpose | Inherits code? | Use case |
|---------|---------|----------------|----------|
| `extends` | Subclass (inheritance) | Yes — inherits implementation | Is-a relationship |
| `implements` | Contract (interface) | No — must override all members | Guarantees interface |
| `with` | Mix in code (mixin) | Yes — adds methods | Reuse code across classes |

### Examples

```dart
// Base class
class Animal {
  void eat() => print("Eating");
  void sleep() => print("Sleeping");
}

// EXTENDS - inherits & can override
class Dog extends Animal {
  void bark() => print("Woof");

  @override
  void eat() => print("Dog eating");
}
Dog dog = Dog();
dog.eat();   // "Dog eating" (overridden)
dog.sleep(); // "Sleeping" (inherited)
dog.bark();  // "Woof" (own method)

// IMPLEMENTS - must override ALL members (no inheritance)
class Cat implements Animal {
  @override
  void eat() => print("Cat eating");    // MUST implement
  @override
  void sleep() => print("Cat sleeping"); // MUST implement too
}
Cat cat = Cat();
cat.eat();  // "Cat eating"

// WITH - mixin (reuse across classes)
mixin Playable {
  void play() => print("Playing");
}

class Dog with Playable {
  // Gets play() method automatically
}
Dog dog = Dog();
dog.play();  // "Playing"
```

> **Note:** If `Animal` had more members, `Cat implements Animal` would need to override every one — otherwise it wouldn't compile.

### Multi-Level

```dart
class Dog extends Animal implements Comparable with Playable {
  // Extends: inherits from Animal
  // Implements: must override Comparable methods
  // With: gets methods from Playable mixin
}
```

### Key Differences

```dart
// extends - shares code
class Parent {
  void method() => print("Parent");
}
class Child extends Parent {} // Inherits method()

// implements - defines contract
abstract class Interface {
  void method();
}
class Implementation implements Interface {
  @override
  void method() => print("Must implement");
}

// with - adds methods
mixin Behavior {
  void method() => print("From mixin");
}
class User with Behavior {} // Gets method()
```

### When to Use

- `extends` → Parent-child relationship, reuse logic.
- `implements` → Force a class to follow an interface.
- `with` → Add behavior to multiple unrelated classes.

**Key point:** `extends` inherits code, `implements` inherits only the contract, `with` mixes in behavior.

---

<div style="page-break-after: always;"></div>

# 18. Abstract classes

In **Dart**, abstract classes are classes that **cannot be instantiated** and **define methods that subclasses must implement**.

### Syntax

```dart
abstract class ClassName {
  void abstractMethod();  // No body - must override

  void concreteMethod() {  // Can have implementation
    print("Concrete");
  }
}
```

### Examples

```dart
// Abstract class
abstract class Animal {
  void eat();        // Abstract - subclass must implement
  void sleep();      // Abstract - subclass must implement

  void rest() {      // Concrete - has implementation
    print("Resting");
  }
}

// Cannot do this:
// Animal animal = Animal();  // Error - can't instantiate

// Must extend and override abstract methods
class Dog extends Animal {
  @override
  void eat() => print("Dog eating");

  @override
  void sleep() => print("Dog sleeping");

  // Inherits rest() from Animal
}

final dog = Dog();  // OK - concrete class
dog.eat();          // "Dog eating"
dog.rest();         // "Resting" (from abstract class)
```

### Abstract Properties

```dart
abstract class Vehicle {
  String get name;           // Abstract getter
  double get speed;          // Abstract getter

  void start() => print("Starting");  // Concrete
}

class Car extends Vehicle {
  @override
  String get name => "Car";

  @override
  double get speed => 100.0;
}
```

**Key point:** Abstract classes define the blueprint; concrete subclasses provide the implementation.

---

<div style="page-break-after: always;"></div>

# 19. Factory constructors

In **Dart**, factory constructors are constructors that **don't necessarily create a new instance**. They can return existing instances, subclasses, or control object creation.

### Syntax

```dart
factory ClassName(...) {
  // Custom logic
  return instance;
}
```

### Examples

```dart
// Singleton - returns same instance
class Logger {
  static Logger? _instance;

  factory Logger() {
    _instance ??= Logger._internal();  // Create once, reuse
    return _instance!;
  }

  Logger._internal();  // Private constructor
}

final log1 = Logger();
final log2 = Logger();
print(log1 == log2);  // true - same instance!

// Conditional return - return subclass
abstract class Animal {
  factory Animal(String type) {
    if (type == "dog") return Dog();
    if (type == "cat") return Cat();
    throw "Unknown animal";
  }
}

class Dog implements Animal {}
class Cat implements Animal {}

final animal = Animal("dog");  // Returns Dog instance
```

### Caching Example

```dart
class User {
  static final Map<String, User> _cache = {};

  final String id;

  factory User(String id) {
    return _cache.putIfAbsent(id, () => User._internal(id));
  }

  User._internal(this.id);
}

final user1 = User("123");
final user2 = User("123");
print(user1 == user2);  // true - same cached instance
```

### Parse from JSON

```dart
class Config {
  final String apiUrl;

  factory Config.fromJson(Map<String, dynamic> json) {
    return Config(
      apiUrl: json['url'] ?? 'https://api.example.com',
    );
  }

  Config({required this.apiUrl});
}

final config = Config.fromJson({'url': 'https://custom.com'});
```

**Key point:** Factory constructors control instance creation — they can return cached instances, subclasses, or apply custom logic before creating.

---

<div style="page-break-after: always;"></div>

# 20. Enums

In **Dart**, an enum (enumeration) is a special class used to represent a **fixed set of constant values**.

### Syntax

```dart
enum Status {
  active,
  inactive,
  pending,
}
```

### Basic Usage

```dart
enum Status {
  active,
  inactive,
  pending,
}

void main() {
  Status current = Status.active;

  print(current);           // Status.active
  print(current.index);     // 0
  print(current.name);      // "active"

  // Switch over enum
  switch (current) {
    case Status.active:
      print("User is active");
    case Status.inactive:
      print("User is inactive");
    case Status.pending:
      print("User is pending");
  }

  // Iterate all values
  for (final status in Status.values) {
    print(status);
  }
}
```

### Enhanced Enums (Dart 2.17+)

```dart
enum Planet {
  mercury(mass: 3.3e23, radius: 2439.7),
  earth(mass: 5.97e24, radius: 6371.0),
  mars(mass: 6.42e23, radius: 3389.5);

  const Planet({required this.mass, required this.radius});

  final double mass;
  final double radius;

  double get surfaceGravity => 6.674e-11 * mass / (radius * radius);
}

void main() {
  print(Planet.earth.surfaceGravity);
  print(Planet.mars.radius);
}
```

**Key point:** Enums represent a fixed, type-safe set of named constants — much safer than raw strings or integers.

---

<div style="page-break-after: always;"></div>

# 21. Generics

In **Dart**, generics let you **write code that works with different types** while maintaining type safety.

### Syntax

```dart
class ClassName<T> {
  // T is a placeholder for any type
}

List<String> names = ["a", "b"];  // T = String
List<int> numbers = [1, 2];       // T = int
```

### Examples

```dart
// Generic class
class Box<T> {
  T item;

  Box(this.item);

  T getItem() => item;
  void setItem(T newItem) => item = newItem;
}

final stringBox = Box<String>("Hello");
print(stringBox.getItem());  // "Hello" - type safe

final intBox = Box<int>(42);
print(intBox.getItem());     // 42

// Generic function
T printAndReturn<T>(T value) {
  print(value);
  return value;
}

printAndReturn<String>("test");  // T = String
printAndReturn<int>(123);        // T = int

// Generic with constraints
class NumberBox<T extends num> {
  T value;
  NumberBox(this.value);

  T add(T other) => value + other as T;  // Only numbers
}

NumberBox<int>(5).add(3);     // Works
// NumberBox<String>("a");    // Error - String isn't num
```

### Built-in Generics

```dart
List<String> names = ["Alice", "Bob"];
Map<String, int> ages = {"Alice": 30, "Bob": 25};
Set<int> numbers = {1, 2, 3};

// Without generics (less safe)
List items = ["a", 1, true];  // Mixed types, no type safety
```

### Multiple Type Parameters

```dart
class Pair<K, V> {
  K key;
  V value;

  Pair(this.key, this.value);
}

final pair = Pair<String, int>("age", 30);
print(pair.key);    // "age"
print(pair.value);  // 30
```

**Key point:** Write once, use with multiple types safely — no casting needed.

---

<div style="page-break-after: always;"></div>

# 22. Exception handling

In **Dart**, exception handling lets you **catch and handle errors** gracefully.

### Syntax

```dart
try {
  // Code that might throw error
} catch (e) {
  // Handle error
} finally {
  // Always runs (optional)
}
```

### Examples

```dart
// Basic try-catch
try {
  int result = 10 ~/ 0;  // Division by zero
} catch (e) {
  print("Error: $e");    // Catches error
}

// Catch specific exceptions
try {
  int.parse("abc");
} on FormatException catch (e) {
  print("Format error: $e");
} on Exception catch (e) {
  print("Generic error: $e");
}

// With stack trace
try {
  throw Exception("Something went wrong");
} catch (e, stackTrace) {
  print("Error: $e");
  print("Stack: $stackTrace");
}

// Finally - always runs
try {
  final file = openFile("data.txt");
  processFile(file);
} catch (e) {
  print("Error: $e");
} finally {
  file.close();  // Runs even if error
}
```

### Throw Custom Exceptions

```dart
// Custom exception
class UserNotFoundException implements Exception {
  String message;
  UserNotFoundException(this.message);

  @override
  String toString() => "User Error: $message";
}

// Throw it
void getUser(String id) {
  if (id.isEmpty) {
    throw UserNotFoundException("ID cannot be empty");
  }
}

// Catch it
try {
  getUser("");
} catch (e) {
  print(e);  // "User Error: ID cannot be empty"
}
```

### Async Exception Handling

```dart
// Future
Future<String> fetchData() async {
  try {
    final response = await api.get("/data");
    return response;
  } catch (e) {
    print("API error: $e");
    return "error";
  }
}

// Stream
stream.listen(
  (data) => print(data),
  onError: (e) => print("Stream error: $e"),
);
```

**Key point:** Wrap risky code in try-catch, handle errors, use finally for cleanup.

**See also:** [#23 — throw vs rethrow](#23-difference-between-throw-and-rethrow)

---

<div style="page-break-after: always;"></div>

# 23. Difference between throw and rethrow

In **Dart**:

| Keyword | Purpose | Stack trace |
|---------|---------|-------------|
| `throw` | Throw new exception or modify caught exception | New stack trace |
| `rethrow` | Re-throw caught exception unchanged | Original stack trace preserved |

### Examples

```dart
// throw - loses original stack trace
try {
  riskyOperation();
} catch (e) {
  throw Exception("New error: $e");  // Creates new stack trace
}

// rethrow - preserves original stack trace
try {
  riskyOperation();
} catch (e) {
  print("Logged: $e");
  rethrow;  // Re-throws same exception with original stack
}
```

### Difference in Action

```dart
void level3() {
  throw Exception("Original error");
}

void level2() {
  try {
    level3();
  } catch (e) {
    rethrow;  // Stack trace shows: level3 → level2 → level1
  }
}

void level1() {
  try {
    level2();
  } catch (e, stackTrace) {
    print(stackTrace);  // Shows full chain with level3
  }
}

// vs with throw:
void level2b() {
  try {
    level3();
  } catch (e) {
    throw e;  // Stack trace starts here, loses level3 context
  }
}
```

### Common Pattern

```dart
try {
  await fetchUser();
} catch (e) {
  log("Error occurred: $e");  // Log it
  rethrow;  // Re-throw to let caller handle, keep original stack
}
```

**Key point:** Use `rethrow` to preserve debugging info; use `throw` to create or transform errors.

**See also:** [#22 — Exception handling](#22-exception-handling)

---

<div style="page-break-after: always;"></div>

# 24. Futures

In **Dart**, a Future represents **an asynchronous operation that completes with a value in the future** (not immediately).

### Syntax

```dart
Future<T> - represents value of type T that will arrive later
```

### Examples

```dart
// Create Future
Future<String> fetchName() {
  return Future.delayed(Duration(seconds: 2), () => "Vinayak");
}

// Consume with .then()
fetchName().then((name) {
  print(name);  // "Vinayak" (after 2 seconds)
});

// Chain multiple operations
fetchName()
    .then((name) => print("Hello, $name"))
    .then((_) => print("Done"));

// Handle error with .catchError()
fetchName()
    .then((name) => print(name))
    .catchError((error) => print("Error: $error"));
```

### Async/await (Cleaner Syntax)

```dart
// Instead of .then() chain
Future<void> main() async {
  try {
    final name = await fetchName();  // Wait for result
    print(name);                      // "Vinayak"
  } catch (e) {
    print("Error: $e");
  }
}
```

### Common Methods

```dart
// Future.value() - resolves immediately
Future<int> quick() => Future.value(42);

// Future.delayed() - wait then resolve
Future<String> delayed() =>
  Future.delayed(Duration(seconds: 1), () => "Done");

// Future.error() - rejects
Future error() => Future.error(Exception("Failed"));

// Wait for multiple Futures
Future.wait([future1, future2, future3])
    .then((results) => print(results));
```

> **Note:** `Future.wait` fails fast on the first error by default. Pass `eagerError: false` to wait for all futures and collect errors together.

**Key point:** Operations that take time (API calls, file reads) return Futures instead of blocking.

**See also:** [#25 — async/await](#25-asyncawait)

---

<div style="page-break-after: always;"></div>

# 25. async/await

In **Dart**, `async`/`await` is **syntactic sugar for working with Futures** — makes async code look synchronous and easier to read.

### Syntax

```dart
// async keyword makes function return Future
Future<T> functionName() async {
  // await pauses until Future completes
  final result = await futureOperation();
  return result;
}
```

### Examples

```dart
// Without async/await (hard to read)
Future<String> getName() {
  return fetchUser().then((user) => user.name);
}

// With async/await (clean)
Future<String> getName() async {
  final user = await fetchUser();
  return user.name;
}

// Error handling
Future<void> loadData() async {
  try {
    final data = await fetchFromApi();  // Waits for Future
    print(data);                         // After it completes
  } catch (e) {
    print("Error: $e");
  }
}

// Sequential operations
Future<void> process() async {
  final step1 = await operation1();
  final step2 = await operation2(step1);  // Waits for step1
  print(step2);
}

// Concurrent operations (start both, then await)
Future<void> concurrent() async {
  final future1 = operation1();  // Starts immediately
  final future2 = operation2();  // Starts immediately

  final result1 = await future1;
  final result2 = await future2;  // Both already running
}
```

> **Note:** The "concurrent" example starts both operations before awaiting. This is different from sequential awaiting, where the second only starts after the first completes.

### Key Differences

```dart
// .then() chain (old way)
fetchUser()
    .then((user) => print(user.name))
    .catchError((e) => print(e));

// async/await (new way - same result, cleaner)
try {
  final user = await fetchUser();
  print(user.name);
} catch (e) {
  print(e);
}
```

**Key point:** `async` makes a function return a Future; `await` pauses until the Future completes. Code reads top-to-bottom like synchronous code.

**See also:** [#24 — Futures](#24-futures), [#26 — Future vs Stream vs Isolate](#26-difference-between-future-stream-and-isolate)

---

<div style="page-break-after: always;"></div>

# 26. Difference between Future, Stream, and Isolate

In **Dart**:

| Type | Produces | When | Use case |
|------|----------|------|----------|
| `Future<T>` | Single value | Once (in future) | API call, file read, async task |
| `Stream<T>` | Multiple values | Over time (ongoing) | Real-time data, events, continuous stream |
| `Isolate` | Communicates via ports | Parallel execution | Heavy computation, blocking work |

> **Note:** Isolates don't "return" values directly — they communicate via `SendPort`/`ReceivePort`. `Isolate.run` wraps this for convenience.

### Examples

```dart
// FUTURE - single value once
Future<String> fetchUser() async {
  await Future.delayed(Duration(seconds: 1));
  return "Vinayak";  // One value, done
}

final name = await fetchUser();
print(name);  // "Vinayak"

// STREAM - multiple values over time
Stream<int> countUp() async* {
  for (int i = 1; i <= 5; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;  // Emit value
  }
}

countUp().listen((num) {
  print(num);  // 1, 2, 3, 4, 5 (over 5 seconds)
});

// ISOLATE - run heavy work on separate thread
int fibonacci(int n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

// Don't block UI
final result = await Isolate.run(() => fibonacci(40));
print(result);  // Computes without freezing UI
```

### Visual Difference

```text
Future:
|-----Time------|
Operation
               Result

Stream:
|-----Time------|
Value1 Value2 Value3 Value4

Isolate:
Main thread:  |----|---|----| (continues)
Isolate:         |-----Heavy work-----| (separate)
```

### When to Use

```dart
// Future - one async operation
final user = await api.getUser("123");

// Stream - listen to changes
messages.stream.listen((msg) => print(msg));

// Isolate - CPU-intensive work
final sorted = await Isolate.run(() => largeList.sort());
```

**Key point:** Future = one result; Stream = multiple results over time; Isolate = parallel execution on a separate thread.

**See also:** [#29 — Isolates](#29-isolates), [#30 — When to use an isolate](#30-when-to-use-an-isolate)

---

<div style="page-break-after: always;"></div>

# 27. StreamController

In **Dart**, a `StreamController` lets you **create a Stream manually and control it** — add values, close it, handle listeners.

### Syntax

```dart
final controller = StreamController<T>();
// controller.sink - add values
// controller.stream - listen to values
```

### Examples

```dart
// Create controller
final controller = StreamController<int>();

// Add values via sink
controller.sink.add(1);
controller.sink.add(2);
controller.sink.add(3);

// Listen to stream
controller.stream.listen((value) {
  print(value);  // 1, 2, 3
});

// Close when done
controller.close();
```

> **Note:** You cannot call `sink.add()` after `close()` — it throws a `StateError`.

### Practical Example

```dart
class DataService {
  final controller = StreamController<String>();

  // Expose as stream (not sink)
  Stream<String> get dataStream => controller.stream;

  // Add data internally
  void fetchData() async {
    for (int i = 0; i < 5; i++) {
      await Future.delayed(Duration(seconds: 1));
      controller.sink.add("Data $i");
    }
    controller.close();
  }
}

// Usage
final service = DataService();
service.dataStream.listen((data) => print(data));
service.fetchData();
```

### Broadcast Controller — Multiple Listeners

```dart
// Regular controller - only one listener
final single = StreamController<int>();

// Broadcast - many listeners
final broadcast = StreamController<int>.broadcast();

broadcast.stream.listen((v) => print("Listener 1: $v"));
broadcast.stream.listen((v) => print("Listener 2: $v"));

broadcast.sink.add(42);  // Both listeners receive 42
```

### Error Handling

```dart
final controller = StreamController<int>();

controller.stream.listen(
  (value) => print(value),
  onError: (e) => print("Error: $e"),
  onDone: () => print("Stream closed"),
);

controller.sink.add(1);
controller.sink.addError(Exception("Oops"));
controller.close();
```

**Key point:** Manually create and control streams — useful for wrapping callbacks or managing data flow.

---

<div style="page-break-after: always;"></div>

# 28. Event loop

In **Dart**, the event loop **manages the order of execution** for synchronous code, microtasks, and asynchronous operations.

### How It Works

```text
1. Execute all synchronous code
2. Drain the microtask queue (highest priority async)
3. Process one event from the event queue
4. Go back to step 2 (drain microtasks after every event)
5. Repeat
```

> **Important:** The event loop drains the **entire** microtask queue after each event — not just once at startup.

### Two Async Queues

```dart
// Microtask queue (runs first)
scheduleMicrotask(() => print("Microtask"));

// Event queue (runs after microtasks)
Future(() => print("Future"));
```

### Execution Order Example

```dart
void main() {
  print("1. Start");

  Future(() => print("4. Future"));

  scheduleMicrotask(() => print("3. Microtask"));

  print("2. End");
}

// Output:
// 1. Start
// 2. End
// 3. Microtask
// 4. Future
```

### Why It Matters

```dart
void main() async {
  print("1. Sync");

  Future.delayed(Duration.zero, () => print("3. Future"));

  await Future.microtask(() => print("2. Microtask"));

  print("4. After await");
}

// Output:
// 1. Sync
// 2. Microtask
// 4. After await
// 3. Future
```

**Key point:** Synchronous code runs first, then all microtasks drain, then events are processed one at a time — with microtasks draining again after each event.

---

<div style="page-break-after: always;"></div>

# 29. Isolates

In **Dart**, an Isolate is a **separate thread with its own memory and event loop** — runs code in parallel without blocking the main thread.

### Syntax

```dart
final result = await Isolate.run(() => heavyComputation());
```

> **Note:** `Isolate.run` requires Dart 2.19+. For older versions, use `Isolate.spawn` with `SendPort`/`ReceivePort`.

### Examples

```dart
// Synchronous blocking (bad)
int fibonacci(int n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

void main() {
  print("Start");
  final result = fibonacci(40);  // Freezes UI for seconds
  print("Result: $result");
}

// With Isolate (good - non-blocking)
void main() async {
  print("Start");
  final result = await Isolate.run(() => fibonacci(40));  // Parallel
  print("Result: $result");
  // UI stays responsive
}
```

### Spawn Isolate with Communication

```dart
// Worker function
void isolateWork(SendPort sendPort) {
  final value = fibonacci(40);
  sendPort.send(value);  // Send result back
}

void main() async {
  final receivePort = ReceivePort();

  // Start isolate
  await Isolate.spawn(isolateWork, receivePort.sendPort);

  // Wait for result
  final result = await receivePort.first;
  print("Result: $result");
}
```

### Key Differences from Threads

- No shared memory (safe, no race conditions).
- Communicate via message passing.
- Heavy computation doesn't freeze the UI.

### When to Use

- CPU-intensive work (parsing, encryption, heavy math).
- File operations.
- Image processing.
- Data parsing.

**Key point:** Run heavy work on a separate isolate so the main thread stays responsive for UI and user input.

**See also:** [#30 — When to use an isolate](#30-when-to-use-an-isolate)

---

<div style="page-break-after: always;"></div>

# 30. When to use an isolate

Use isolates when the **main thread would block** and freeze the UI.

### CPU-Intensive Operations

```dart
// Heavy computation
final sorted = await Isolate.run(() => largeList.sort());
final fib = await Isolate.run(() => fibonacci(50));
final hash = await Isolate.run(() => computeHash(largeData));
```

### Large Data Processing

```dart
// Parse huge JSON
final data = await Isolate.run(() => jsonDecode(largeJsonString));

// Filter/transform large lists
final filtered = await Isolate.run(() =>
  items.where((i) => expensiveCheck(i)).toList()
);
```

### Image/Video Operations

```dart
final compressed = await Isolate.run(() =>
  compressImage(imageBytes)
);
```

### Encryption/Hashing

```dart
final encrypted = await Isolate.run(() =>
  encryptData(sensitiveData, key)
);
```

### When NOT to Use

```dart
// Too lightweight - overhead exceeds benefit
await Isolate.run(() => print("hello"));
await Isolate.run(() => x + y);

// Network I/O - use Future instead
final user = await api.getUser();  // Already async, no isolate needed

// UI updates - must happen on main thread
// Can't update setState from isolate
```

**Rule of thumb:** If an operation takes **>100ms**, consider an isolate to keep the UI responsive.

**Key point:** Use isolates for CPU-bound work that would otherwise freeze the UI; use Futures for I/O-bound work.

**See also:** [#26 — Future vs Stream vs Isolate](#26-difference-between-future-stream-and-isolate), [#29 — Isolates](#29-isolates)
