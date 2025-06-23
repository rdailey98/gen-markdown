## Swift Style Guide for Language Models

This unified Swift style guide synthesizes conventions from Airbnb, Google, Kodeco, and the Swift API Design Guidelines. It is designed for large language models (LLMs) to produce idiomatic, maintainable, and consistent Swift code.

---

## Table of Contents

1. [Code Layout](#code-layout)
2. [Naming Conventions](#naming-conventions)
3. [Types & Type Inference](#types--type-inference)
4. [Access Control](#access-control)
5. [Optionals & Unwrapping](#optionals--unwrapping)
6. [Functions & Closures](#functions--closures)
7. [Control Flow](#control-flow)
8. [Enum Usage](#enum-usage)
9. [Protocols & Delegation](#protocols--delegation)
10. [Error Handling](#error-handling)
11. [Asynchronous Code](#asynchronous-code)
12. [Documentation & Comments](#documentation--comments)
13. [File Structure](#file-structure)

---

## Code Layout

- Indent using **2 spaces**. Never use tabs.
- Line length limit: **100 characters**.
- Trim trailing whitespace.
- Always end files with a **newline**.
- Insert a blank line:
  - Between top-level declarations
  - Between functions within a type
  - After multiline `switch` cases

### Braces

```swift
// Correct
if isReady {
  execute()
}

// Incorrect
if isReady
{
  execute()
}
```

### Colon Spacing

```swift
// Correct
let name: String
let dict: [String: Int]

// Incorrect
let name :String
let dict :[String:Int]
```

---

## Naming Conventions

- Use **UpperCamelCase** for types and protocols.
- Use **lowerCamelCase** for functions, variables, constants.
- Boolean names should use `is`, `has`, `can`, `should`.
- Acronyms: capitalize uniformly. Example: `userID`, `URLRequest`, `htmlBody`.
- No Objective-C-style prefixes (e.g. `RW` or `AIR`).

### Backing Properties

Use `_property` only when backing a public/computed property.

```swift
private var _value: Int
var value: Int {
  _value * 2
}
```

---

## Types & Type Inference

- Prefer **type inference** when the right-hand side is clear.

```swift
let name = "World"
let count = items.count
```

- Explicitly specify types for:
  - Empty arrays/dictionaries
  - Public API signatures

```swift
var names: [String] = []
let payload: [String: Any] = [:]
```

- Use shorthand:

```swift
[String]   // ✅
Array<String> // ❌
```

- Use `Void` as closure return type:

```swift
() -> Void // ✅
() -> ()   // ❌
```

---

## Access Control

- Omit `internal` (it is the default).
- Always use the **strictest level** needed.

```swift
private let count = 0
```

- Specify access **on each declaration**, not at the extension level.

```swift
extension Foo {
  public func doSomething() {}
}
```

---

## Optionals & Unwrapping

- Prefer **optional binding** over forced unwrapping.
- Use Swift 5.7 shorthand when appropriate:

```swift
if let user {
  print(user.name)
}
```

- Use optional chaining for short accesses:

```swift
let name = user?.profile?.name
```

- Use `!` only when absolutely safe and justified (e.g. `@IBOutlet`).

---

## Functions & Closures

### Declarations

- Use meaningful names. Follow grammar and fluency.

```swift
func insert(_ item: T, at index: Int) {}
```

### Calls

- Align multiline calls like this:

```swift
performAction(
  with: item,
  options: [.animated, .delayed]
)
```

### Closures

- Use trailing closure **only if it’s the last parameter**.
- Use `{ $0 }` only when context is obvious.
- Omit `return` in single-expression closures:

```swift
let squares = numbers.map { $0 * $0 }
```

- Use `Void`, not `()`:

```swift
let handler: () -> Void
```

---

## Control Flow

- Avoid nesting: use `guard` early exits.

```swift
guard let user else {
  return
}
```

- Use commas `,` for multiple `guard`/`if` conditions, not `&&`.

```swift
guard let a = a, let b = b else { return }
```

- Do not wrap conditionals in parentheses.

```swift
if x > 0 { } // ✅
if (x > 0) { } // ❌
```

---

## Enum Usage

- One case per line unless trivial:

```swift
enum Planet {
  case mercury
  case venus
}
```

- Avoid `default` if all cases are handled.
- Use raw values only when mapping to external data.

---

## Protocols & Delegation

- Group conformance in dedicated extensions:

```swift
// MARK: - UITableViewDelegate
extension MyViewController: UITableViewDelegate {
  ...
}
```

- Protocol names:

  - Noun for identity: `Collection`, `Listable`
  - `-able`, `-ible` for capability: `Encodable`, `Equatable`

- Delegate method naming:

```swift
func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath)
```

---

## Error Handling

- Use `try?` only if failure is expected.
- Prefer `guard let` with `try?` over `try!`.

```swift
guard let data = try? fetchData() else {
  return
}
```

- Throwing functions must be clearly documented.

---

## Asynchronous Code

- Use `[weak self]` in closures. Capture `self` safely:

```swift
someAsyncOp { [weak self] result in
  guard let self else { return }
  self.handle(result)
}
```

- Prefer `Task {}` over deprecated `DispatchQueue` APIs.

---

## Documentation & Comments

- Use `///` for doc comments. Avoid `/** */`.
- Begin with a **summary line**.
- Use Markdown:

```swift
/// Updates the title label.
///
/// - Parameter title: The title to display.
/// - Returns: True if update succeeded.
```

- Comment **why**, not what.

---

## File Structure

- File names should match primary type (`MyClass.swift`).
- Use `// MARK:` to organize:

```swift
// MARK: Lifecycle
// MARK: Public
// MARK: Private
```

- Group protocol conformances with `MARK` headers.

- Sort import statements alphabetically. Group `@testable` separately.

---

## End

