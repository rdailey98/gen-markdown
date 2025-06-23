# Unified Swift Style Guide

A comprehensive style guide for writing clear, consistent, and idiomatic Swift code, synthesized from industry best practices.

## Table of Contents

1. [Source File Basics](#source-file-basics)
2. [Naming](#naming)
3. [Code Organization](#code-organization)
4. [Spacing and Formatting](#spacing-and-formatting)
5. [Language Features](#language-features)
6. [Functions and Methods](#functions-and-methods)
7. [Closures](#closures)
8. [Types and Properties](#types-and-properties)
9. [Control Flow](#control-flow)
10. [Error Handling](#error-handling)
11. [Access Control](#access-control)
12. [Memory Management](#memory-management)
13. [Documentation](#documentation)
14. [Testing](#testing)
15. [SwiftUI and Modern Swift](#swiftui-and-modern-swift)

## Source File Basics

### File Names

- **Name files after the primary type they contain**: `MyViewController.swift`
- **Use `TypeName+ProtocolName.swift` for protocol conformance extensions**: `Array+Equatable.swift`
- **Use descriptive names for files with multiple types**: `NetworkingModels.swift`

### File Encoding

- **Always use UTF-8 encoding**
- **End files with a single newline character**

### Import Statements

- **Alphabetize all imports**
- **Group imports by type with blank lines between groups**:
  1. System frameworks
  2. Third-party frameworks
  3. Local/project imports
  4. `@testable` imports (in test files only)

```swift
import CoreLocation
import Foundation
import UIKit

import Alamofire
import RxSwift

import MyProjectCore
import MyProjectUI

@testable import MyProject
```

- **Import only required modules** (prefer `Foundation` over `UIKit` when UI isn't needed)

## Naming

### General Principles

- **Use descriptive, unambiguous names**
- **Prioritize clarity over brevity**
- **Use American English spelling** (`color` not `colour`)

### Naming Conventions

- **Types and protocols**: `UpperCamelCase`
- **Everything else**: `lowerCamelCase`
- **Acronyms**: Treat as words (`UrlValidator` → `URLValidator`, `userId` → `userID`)

```swift
// Correct
class URLValidator {
    func isValidURL(_ url: URL) -> Bool { }
}

let urlValidator = URLValidator()
let userID = "12345"

// Incorrect
class UrlValidator {
    func isValidUrl(_ URL: URL) -> Bool { }
}
```

### Booleans

- **Use `is`, `has`, `should`, `will` prefixes for clarity**

```swift
var isLoading: Bool
var hasPermission: Bool
var shouldAutorotate: Bool
```

### Protocols

- **Protocols describing what something is**: Use nouns (`Collection`)
- **Protocols describing capabilities**: Use `-able`, `-ible`, or `-ing` suffixes

```swift
protocol Equatable { }
protocol ProgressReporting { }
protocol Collection { }
```

### Delegates

- **First parameter should be the delegate source**
- **Method names should read like sentences**

```swift
func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath)
func scrollViewDidScroll(_ scrollView: UIScrollView)
```

## Code Organization

### Type Organization

- **Use `// MARK: -` comments to organize code**
- **Standard MARK organization**:
  1. `// MARK: - Lifecycle`
  2. `// MARK: - Public`
  3. `// MARK: - Internal`
  4. `// MARK: - Private`

```swift
class MyViewController: UIViewController {
    // MARK: - Lifecycle
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    // MARK: - Public
    
    func configure(with model: Model) { }
    
    // MARK: - Private
    
    private func setupUI() { }
}
```

### Extensions

- **Use extensions to organize protocol conformances**
- **One protocol per extension when practical**

```swift
// MARK: - UITableViewDataSource

extension MyViewController: UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return items.count
    }
}

// MARK: - UITableViewDelegate

extension MyViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        // Handle selection
    }
}
```

### Property Organization

Within each access level section, order properties as:
1. Static properties
2. Instance properties
3. Computed properties

## Spacing and Formatting

### Indentation

- **Use 2 spaces for indentation** (never tabs)
- **Configure Xcode to use spaces**

### Line Length

- **Maximum line length: 100 characters**
- **Wrap long lines at appropriate breaking points**

### Braces

- **Opening braces on same line with one space before**
- **Closing braces on new line**

```swift
if condition {
    // code
} else {
    // code
}

func doSomething() {
    // code
}
```

### Spacing

- **One space after commas**: `[1, 2, 3]`
- **One space around operators**: `let sum = 1 + 2`
- **No space before colons, one space after**: `[key: value]`
- **No spaces inside brackets/parentheses**: `array[0]`, `function(param)`

### Blank Lines

- **One blank line between type members**
- **One blank line between logical sections**
- **No blank lines at start/end of scopes**

## Language Features

### Type Inference

- **Prefer type inference for single instances**
- **Explicit types for empty collections and specific numeric types**

```swift
// Preferred
let name = "Swift"
let count = 42
let names: [String] = []
let maxWidth: CGFloat = 100.0

// Not Preferred
let name: String = "Swift"
let count: Int = 42
let names = [String]()
```

### Optionals

- **Use optional binding over force unwrapping**
- **Prefer `guard` for early exits**
- **Use nil-coalescing operator for defaults**

```swift
// Preferred
guard let name = optionalName else { return }

if let value = optional {
    use(value)
}

let name = optionalName ?? "Default"

// Not Preferred
let name = optionalName!
```

### Lazy Initialization

- **Use `lazy` for expensive operations**
- **Prefer closures for complex initialization**

```swift
lazy var dateFormatter: DateFormatter = {
    let formatter = DateFormatter()
    formatter.dateStyle = .long
    formatter.timeStyle = .none
    return formatter
}()
```

## Functions and Methods

### Declaration Style

- **Short declarations on one line**
- **Multi-line for long parameter lists**

```swift
func add(_ a: Int, to b: Int) -> Int {
    return a + b
}

func animate(
    withDuration duration: TimeInterval,
    delay: TimeInterval = 0,
    options: UIView.AnimationOptions = [],
    animations: @escaping () -> Void,
    completion: ((Bool) -> Void)? = nil
) {
    // Implementation
}
```

### Parameter Naming

- **Omit labels when purpose is clear from type**
- **Use labels to clarify at call site**

```swift
func move(from start: Point, to end: Point)
func fade(toAlpha alpha: CGFloat)
func insert(_ element: Element, at index: Int)
```

### Return Statements

- **Omit `return` in single-expression functions**

```swift
// Preferred
func doubled(_ x: Int) -> Int {
    x * 2
}

// Also fine for clarity
func doubled(_ x: Int) -> Int {
    return x * 2
}
```

## Closures

### Trailing Closure Syntax

- **Use trailing closure for single closure at end**
- **Named parameters for multiple closures**

```swift
// Preferred
UIView.animate(withDuration: 0.3) {
    view.alpha = 0
}

UIView.animate(
    withDuration: 0.3,
    animations: {
        view.alpha = 0
    },
    completion: { finished in
        view.removeFromSuperview()
    }
)

// Not Preferred for multiple closures
UIView.animate(withDuration: 0.3, animations: {
    view.alpha = 0
}) { finished in
    view.removeFromSuperview()
}
```

### Closure Parameters

- **Use meaningful names over shorthand when it improves clarity**
- **Omit types when they can be inferred**

```swift
// Clear context - shorthand is fine
let doubled = numbers.map { $0 * 2 }

// Complex logic - named parameters preferred
let strings = numbers.map { number in
    formatter.string(from: number)
}
```

## Types and Properties

### Structs vs Classes

- **Prefer structs for value types**
- **Use classes for reference semantics or Objective-C interop**
- **Default classes to `final` unless subclassing intended**

```swift
struct Point {
    var x: Double
    var y: Double
}

final class NetworkManager {
    static let shared = NetworkManager()
    private init() {}
}
```

### Computed Properties

- **Omit `get` for read-only properties**
- **Keep simple computed properties on one line when readable**

```swift
var diameter: Double {
    radius * 2
}

var area: Double {
    get {
        return Double.pi * radius * radius
    }
    set {
        radius = sqrt(newValue / Double.pi)
    }
}
```

### Static/Class Members

- **Use `static` by default**
- **Use `class` only when overriding is needed**

```swift
class Vehicle {
    static let maxSpeed = 200
    class var description: String {
        "A vehicle"
    }
}
```

## Control Flow

### Early Exit

- **Use `guard` for early exits**
- **Keep the happy path unindented**

```swift
guard let user = currentUser else { return }
guard user.isActive else { return }

// Happy path continues here
processUser(user)
```

### Switch Statements

- **Prefer exhaustive switches over `default`**
- **Align `case` with `switch`**

```swift
switch direction {
case .north:
    moveUp()
case .south:
    moveDown()
case .east:
    moveRight()
case .west:
    moveLeft()
}
```

### Ternary Operator

- **Use only for simple, clear cases**
- **Avoid nested ternaries**

```swift
// Good
let title = isEditing ? "Save" : "Edit"

// Avoid
let title = isEditing ? (hasChanges ? "Save" : "Cancel") : "Edit"
```

## Error Handling

### Error Types

- **Define specific error types**
- **Make errors descriptive**

```swift
enum NetworkError: Error {
    case invalidURL
    case noData
    case decodingFailed(Error)
}
```

### Try/Catch

- **Avoid `try!` except in tests**
- **Use `try?` only when failure is acceptable**
- **Prefer `do-catch` for explicit handling**

```swift
do {
    let data = try fetchData()
    process(data)
} catch NetworkError.noData {
    showEmptyState()
} catch {
    showError(error)
}
```

## Access Control

### Guidelines

- **Omit `internal` (it's the default)**
- **Be explicit with `private`, `fileprivate`, `public`, `open`**
- **Prefer `private` over `fileprivate`**
- **Order: access control, `static`, other modifiers**

```swift
public final class APIClient {
    private static let baseURL = "https://api.example.com"
    
    private let session: URLSession
    
    public init(session: URLSession = .shared) {
        self.session = session
    }
}
```

## Memory Management

### Reference Cycles

- **Use `weak` for delegates and callbacks**
- **Prefer `weak self` over `unowned self`**

```swift
class ViewController {
    func loadData() {
        networkManager.fetch { [weak self] result in
            guard let self = self else { return }
            self.process(result)
        }
    }
}
```

### Capture Lists

- **Always use capture lists when capturing `self` in closures**
- **Explicitly capture other values when it improves clarity**

```swift
let closure = { [weak self, currentValue = self.value] in
    guard let self = self else { return }
    self.update(with: currentValue)
}
```

## Documentation

### Comments

- **Use `///` for documentation**
- **Use `//` for explanatory comments**
- **Document why, not what**

```swift
/// Fetches user data from the server.
/// - Parameter userID: The unique identifier of the user
/// - Returns: The user object if found
/// - Throws: `NetworkError` if the request fails
func fetchUser(id userID: String) async throws -> User {
    // Implementation
}
```

### MARK Comments

- **Use `// MARK: -` with descriptive titles**
- **Group related functionality**

```swift
// MARK: - Properties

// MARK: - Lifecycle

// MARK: - Public Methods

// MARK: - Private Methods
```

## Testing

### Test Naming

- **Use descriptive test names**
- **Follow `test_condition_expectedResult` pattern**

```swift
func test_userIsLoggedOut_showsLoginScreen() {
    // Test implementation
}
```

### Test Organization

- **Arrange, Act, Assert pattern**
- **Use `XCTUnwrap` over force unwrapping**

```swift
func test_fetchUser_success() throws {
    // Arrange
    let expectedUser = User(id: "123", name: "Test")
    mockService.stubbedUser = expectedUser
    
    // Act
    let user = try XCTUnwrap(sut.fetchUser(id: "123"))
    
    // Assert
    XCTAssertEqual(user.id, expectedUser.id)
}
```

## SwiftUI and Modern Swift

### SwiftUI View Organization

```swift
struct ContentView: View {
    // MARK: - Properties
    
    @State private var isLoading = false
    @Binding var user: User
    
    // MARK: - Body
    
    var body: some View {
        content
            .onAppear(perform: loadData)
    }
    
    // MARK: - Views
    
    private var content: some View {
        // View implementation
    }
    
    // MARK: - Methods
    
    private func loadData() {
        // Implementation
    }
}
```

### Async/Await

- **Prefer async/await over completion handlers**
- **Use structured concurrency with `Task` groups**

```swift
func fetchData() async throws -> [Item] {
    async let users = fetchUsers()
    async let posts = fetchPosts()
    
    return try await process(users: users, posts: posts)
}
```

### Property Wrappers

- **Use appropriate property wrappers in SwiftUI**
- **Understand ownership: `@State`, `@Binding`, `@ObservedObject`, `@StateObject`**

```swift
struct MyView: View {
    @State private var counter = 0
    @Binding var isPresented: Bool
    @StateObject private var viewModel = ViewModel()
    @EnvironmentObject var settings: Settings
}
```

---

This unified style guide represents the synthesis of best practices from Airbnb, Google, Ray Wenderlich, and Apple's Swift guidelines. Follow these conventions to write clear, consistent, and maintainable Swift code. 