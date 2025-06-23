# Unified Swift Style Guide

## Introduction

This style guide synthesizes best practices from Apple's API Design Guidelines, the Google Swift Style Guide, the Airbnb Swift Style Guide, and the Kodeco (Ray Wenderlich) Style Guide. Its goal is to ensure the generation of clear, consistent, idiomatic, and maintainable Swift code.

The primary principles are clarity, consistency, and brevity, in that order. The most important goal is clarity at the point of use.

## Table of Contents

* [Source File Organization](#source-file-organization)
    * [File Naming](#file-naming)
    * [Import Statements](#import-statements)
* [Code Organization](#code-organization)
    * [Protocol Conformance](#protocol-conformance)
    * [MARK Comments](#mark-comments)
    * [Declaration Order](#declaration-order)
* [Naming](#naming)
    * [General Conventions](#general-conventions)
    * [Booleans](#booleans)
    * [Delegates](#delegates)
    * [Acronyms and Initialisms](#acronyms-and-initialisms)
* [Formatting](#formatting)
    * [Line Length](#line-length)
    * [Indentation](#indentation)
    * [Braces](#braces)
    * [Line Wrapping](#line-wrapping)
    * [Vertical Whitespace](#vertical-whitespace)
    * [Horizontal Whitespace](#horizontal-whitespace)
    * [Semicolons](#semicolons)
    * [Parentheses](#parentheses)
* [Types & Declarations](#types--declarations)
    * [Immutability](#immutability)
    * [Type Inference](#type-inference)
    * [Syntactic Sugar](#syntactic-sugar)
    * [Access Control](#access-control)
    * [Class Definitions](#class-definitions)
    * [Structs and Value Types](#structs-and-value-types)
    * [Computed Properties](#computed-properties)
    * [`self` Usage](#self-usage)
* [Functions & Closures](#functions--closures)
    * [Function Declarations](#function-declarations)
    * [Function Calls](#function-calls)
    * [Return Values](#return-values)
    * [Closures](#closures)
* [Control Flow](#control-flow)
    * [Golden Path](#golden-path)
    * [Ternary Operator](#ternary-operator)
    * [switch Statements](#switch-statements)
* [Programming Practices](#programming-practices)
    * [Optionals](#optionals)
    * [Error Handling](#error-handling)
    * [Memory Management](#memory-management)
    * [Global Scope](#global-scope)
* [Documentation Comments](#documentation-comments)
    * [Format](#format)
    * [Content](#content)

---

## Source File Organization

### File Naming

* Source files must use the `.swift` extension.
* A file containing a single type should be named after that type (e.g., `MyType.swift`).
* An extension to a type `MyType` that adds conformance to `MyProtocol` should be named `MyType+MyProtocol.swift`.

### Import Statements

* Import only the modules that are strictly required. For example, do not import `UIKit` if `Foundation` is sufficient.
* Imports must be at the top of the file, after any file-level comments.
* Group imports in the following order, with each group sorted alphabetically and separated by a blank line:
    1.  Standard module imports (e.g., `UIKit`, `Foundation`).
    2.  `@testable` imports (in test targets only).

**Example:**
```swift
//  Copyright © 2025 Kodeco Inc.
//

import Foundation
import UIKit

@testable import MyModule
```

## Code Organization

### Protocol Conformance

* Organize protocol conformance in a separate `extension` for that protocol. This groups related methods together and keeps the original class definition clean.

**Example:**
```swift
class MyViewController: UIViewController {
  // Main class implementation
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
  // UITableViewDataSource methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
  // UIScrollViewDelegate methods
}
```

### MARK Comments

* Use `// MARK: -` to create logical sections of code within a file.
* Precede each type and protocol-conformance extension with a `MARK` comment to improve navigability.
* Within a type, use `MARK:` to group methods and properties by access level and functionality.

**Example:**
```swift
// MARK: - Spaceship

final class Spaceship {
  // MARK: Lifecycle

  init() { ... }

  // MARK: Public

  public var speed: Double

  // MARK: Private

  private func igniteEngines() { ... }
}

// MARK: - Spaceship + Maneuverable
extension Spaceship: Maneuverable {
  // Protocol methods
}
```

### Declaration Order

Within each access-level section, arrange declarations in the following order:
1.  Nested types (`enum`, `struct`, `class`) and type aliases
2.  Static properties
3.  Instance properties (including SwiftUI `@State`, `@Environment`, etc.)
4.  `init` and `deinit` methods
5.  Static methods
6.  Instance methods

## Naming

### General Conventions

* Follow the official [Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/).
* Names for types and protocols must be `UpperCamelCase`. Everything else (functions, methods, variables, constants, enum cases) must be `lowerCamelCase`.
* Prioritize clarity at the point of use over brevity.
* Omit needless words. Words that repeat type information should be removed unless they are necessary to avoid ambiguity.
* Do not use Objective-C-style prefixes like `AB` or `RW`. Swift provides module namespacing to prevent collisions.
* Use US English spelling to align with Apple's APIs.

### Booleans

* Boolean property names should read as assertions about the receiver (e.g., `x.isEmpty`, `line.intersects(otherLine)`).
* Name booleans with prefixes like `is`, `has`, or `allows` (e.g., `isSpaceship`, `hasSpacesuit`).

### Delegates

* Delegate methods must take the source of the delegate call (e.g., the table view) as their first, unnamed parameter.

**Example:**
```swift
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
```

### Acronyms and Initialisms

* Acronyms should be uniformly uppercased or lowercased based on the surrounding case convention.

**Example:**
```swift
var utf8Bytes: [UTF8.CodeUnit]
var isRepresentableAsASCII = true
var userSMTPServer: SecureSMTPServer
```

## Formatting

### Line Length

* Limit lines to a maximum of 100 characters.

### Indentation

* Indent using 2 spaces, not tabs.

### Braces

* Braces must follow the K&R (Kernighan & Ritchie) style.
    * The opening brace (`{`) must appear on the same line as the declaration or statement.
    * The closing brace (`}`) must appear on a new line.

**Example:**
```swift
if user.isHappy {
  // Do something
} else {
  // Do something else
}
```

### Line Wrapping

* For long function declarations or calls, place the opening parenthesis on the same line as the name, then place each argument on its own new, indented line.

**Example:**
```swift
func generateStars(
  at location: Point,
  count: Int,
  color: StarColor
) -> [Star] {
  // Implementation
}

let stars = generateStars(
  at: aLongerLocationPointName,
  count: aLongerCountVariableName,
  color: .blue
)
```

* For multi-line conditional statements, break after the leading keyword (`if`, `guard`, `while`) and indent each condition by 2 spaces.

**Example:**
```swift
guard
  let galaxy,
  galaxy.name == "Milky Way"
else { ... }
```

### Vertical Whitespace

* There should be exactly one blank line between methods and type declarations.
* Do not add blank lines at the beginning or end of a scope (inside braces).
* Files must end with a single newline character.

### Horizontal Whitespace

* Colons must have no space on the left and one space on the right (e.g., `let name: String`).
* Infix operators (`+`, `==`, `??`) must have a single space on each side.
* There should be no space inside the brackets of collection literals (e.g., `[1, 2, 3]`).

### Semicolons

* Do not use semicolons to terminate statements. They are only required to separate multiple statements on a single line, which should be avoided.

### Parentheses

* Omit parentheses around conditional expressions.

**Example:**
```swift
// Preferred
if name == "Hello" { ... }

// Not Preferred
if (name == "Hello") { ... }
```

## Types & Declarations

### Immutability

* Prefer immutable values. Use `let` instead of `var` whenever a value will not be changed.
* Prefer immutable static properties (`static let`) or computed static properties (`static var`) over mutable ones (`static var`) to avoid global mutable state.

### Type Inference

* Let the compiler infer types whenever possible. Avoid writing redundant type information.

**Example:**
```swift
// Preferred
let sun = Star(mass: 1.989e30)
let enableGravity = true

// Not Preferred
let sun: Star = Star(mass: 1.989e30)
let enableGravity: Bool = true
```

* For empty collections, provide an explicit type annotation.

**Example:**
```swift
var names: [String] = []
var lookup: [String: Int] = [:]
```

### Syntactic Sugar

* Always use the shorthand form for standard library types.

**Example:**
* `[Element]` instead of `Array<Element>`.
* `[Key: Value]` instead of `Dictionary<Key, Value>`.
* `Wrapped?` instead of `Optional<Wrapped>`.

### Access Control

* Use the strictest access level possible, preferring `private` over `fileprivate`.
* Omit the `internal` keyword, as it is the default access level.
* Specify access control on each declaration individually within an extension, not on the extension itself.

### Class Definitions

* Mark all classes as `final` by default. Only omit `final` when a class is explicitly designed to be subclassed. This improves performance and promotes safer, more deliberate design.

### Structs and Value Types

* Use structs for types that do not have a distinct identity, such as coordinates or a collection of values. Use classes for types that have an identity or a specific lifecycle, such as a person or a network controller.

### Computed Properties

* For read-only computed properties, omit the `get` clause.

**Example:**
```swift
var diameter: Double {
  return radius * 2
}
```

### `self` Usage

* Omit `self` unless it is required by the compiler. `self` is only required in these cases:
    * To disambiguate a property name from a parameter in an initializer (e.g., `self.name = name`).
    * Inside `@escaping` closures to explicitly capture `self`.

## Functions & Closures

### Function Declarations

* Short function declarations can be on a single line.
* Long function declarations must be wrapped, with each parameter on a new, indented line.

### Function Calls

* Mirror the formatting of declarations. If a call is too long for one line, wrap it with each parameter on a new, indented line.

### Return Values

* Omit the `return` keyword in single-expression functions, closures, and computed properties.
* For functions and closures returning `Void`, use `Void` in type definitions (`() -> Void`), but omit it entirely from `func` declarations (`func doSomething()`).

### Closures

* Use trailing closure syntax when a closure is the final argument of a function.
* If a function call has multiple closure arguments, do not use trailing closure syntax for any of them.
* Use descriptive names for closure parameters instead of shorthand names like `$0` unless the closure is very short and the context is clear.
* Name unused closure parameters with an underscore (`_`).

## Control Flow

### Golden Path

* Use `guard` statements for early exits. The main logic of a function should not be nested inside an `if` statement. This is the "golden path" principle.

**Example:**
```swift
func compute() throws -> Frequencies {
  guard let context = context else {
    throw FFTError.noContext
  }
  guard let inputData = inputData else {
    throw FFTError.noInputData
  }

  // "Golden path" logic starts here
  return frequencies
}
```

### Ternary Operator

* Use the ternary operator (`? :`) only for simple conditions where it enhances clarity, typically during variable assignment. Avoid nesting ternary operators.

### `switch` Statements

* When switching over an enum, enumerate all cases instead of using a `default` case. This ensures that when new cases are added, the compiler will raise an error, forcing all switch statements to be updated.
* Insert a blank line after any switch case that has a multi-line body.

## Programming Practices

### Optionals

* Declare types that can legitimately be `nil` as optionals (`?`).
* Avoid implicitly unwrapped optionals (`!`) except for `IBOutlet`s and other properties that are guaranteed to be non-nil before first use.
* Use the `if let` shorthand for optional binding (`if let myVar { ... }`) when shadowing the original variable name.
* Never use force unwrapping (`!`) or force casting (`as!`) unless it is a logical impossibility for the operation to fail, and include a comment explaining the invariant that guarantees safety. An exception is in test code, where a failed cast should fail the test.

### Error Handling

* Use `do-catch` statements to handle errors that a caller might reasonably want to recover from.
* Do not use `try?` unless the specific error is unimportant and can be safely ignored.
* Never use `try!` to call a throwing function whose error is unhandled.

### Memory Management

* Code must not create strong reference cycles. Use `weak` and `unowned` references to break cycles.
* When capturing `self` in a closure, use `[weak self]` and the `guard let self = self else { return }` idiom to safely extend its lifetime. `[unowned self]` is unsafe and should be avoided unless it is certain that `self` will outlive the closure.

### Global Scope

* Avoid global functions and variables whenever possible. Prefer methods and properties scoped within a type.
* For namespacing constants or static functions, use a caseless `enum` to prevent it from being instantiated.

**Example:**
```swift
enum MathConstants {
  static let pi = 3.14159
}
```

## Documentation Comments

### Format

* Use triple-slash (`///`) for documentation comments.
* Use Apple's documentation markup format for rich text, including parameter names in backticks (`` ` ``).

### Content

* Write a documentation comment for every public declaration.
* Begin with a single-sentence summary of the declaration's purpose.
* Use the `Parameter(s)`, `Returns`, and `Throws` tags to document function details. These may be omitted only if the summary sentence already provides complete clarity.

**Example:**
```swift
/// Returns the sum of the numbers in the given array.
///
/// - Parameter numbers: The numbers to sum.
/// - Returns: The sum of the numbers.
func sum(_ numbers: [Int]) -> Int {
  // ...
}
```