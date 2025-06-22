# Swift Style Guide

This style guide is based on Apple's excellent Swift standard library style and also incorporates feedback from usage across multiple Swift projects within Google. It is a living document and the basis upon which the formatter is implemented.

## Table of Contents

* [Source File Basics](#source-file-basics)
  * [File Names](#file-names)
  * [File Encoding](#file-encoding)
  * [Whitespace Characters](#whitespace-characters)
  * [Special Escape Sequences](#special-escape-sequences)
  * [Invisible Characters and Modifiers](#invisible-characters-and-modifiers)
  * [String Literals](#string-literals)
* [Source File Structure](#source-file-structure)
  * [File Comments](#file-comments)
  * [Import Statements](#import-statements)
  * [Type, Variable, and Function Declarations](#type-variable-and-function-declarations)
  * [Overloaded Declarations](#overloaded-declarations)
  * [Extensions](#extensions)
* [General Formatting](#general-formatting)
  * [Column Limit](#column-limit)
  * [Braces](#braces)
  * [Semicolons](#semicolons)
  * [One Statement Per Line](#one-statement-per-line)
  * [Line-Wrapping](#line-wrapping)
    * [Function Declarations](#function-declarations)
    * [Type and Extension Declarations](#type-and-extension-declarations)
    * [Function Calls](#function-calls)
    * [Control Flow Statements](#control-flow-statements)
    * [Other Expressions](#other-expressions)
  * [Horizontal Whitespace](#horizontal-whitespace)
  * [Horizontal Alignment](#horizontal-alignment)
  * [Vertical Whitespace](#vertical-whitespace)
  * [Parentheses](#parentheses)
* [Formatting Specific Constructs](#formatting-specific-constructs)
  * [Non-Documentation Comments](#non-documentation-comments)
  * [Properties](#properties)
  * [Switch Statements](#switch-statements)
  * [Enum Cases](#enum-cases)
  * [Trailing Closures](#trailing-closures)
  * [Trailing Commas](#trailing-commas)
  * [Numeric Literals](#numeric-literals)
  * [Attributes](#attributes)
* [Naming](#naming)
  * [Apple's API Style Guidelines](#apples-api-style-guidelines)
  * [Naming Conventions Are Not Access Control](#naming-conventions-are-not-access-control)
  * [Identifiers](#identifiers)
  * [Initializers](#initializers)
  * [Static and Class Properties](#static-and-class-properties)
  * [Global Constants](#global-constants)
  * [Delegate Methods](#delegate-methods)
* [Programming Practices](#programming-practices)
  * [Compiler Warnings](#compiler-warnings)
  * [Initializers](#initializers-1)
  * [Properties](#properties-1)
  * [Types with Shorthand Names](#types-with-shorthand-names)
  * [Optional Types](#optional-types)
  * [Error Types](#error-types)
  * [Force Unwrapping and Force Casts](#force-unwrapping-and-force-casts)
  * [Implicitly Unwrapped Optionals](#implicitly-unwrapped-optionals)
  * [Access Levels](#access-levels)
  * [Nesting and Namespacing](#nesting-and-namespacing)
  * [guards for Early Exits](#guards-for-early-exits)
  * [for-where Loops](#for-where-loops)
  * [fallthrough in switch Statements](#fallthrough-in-switch-statements)
  * [Pattern Matching](#pattern-matching)
  * [Tuple Patterns](#tuple-patterns)
  * [Numeric and String Literals](#numeric-and-string-literals)
  * [Playground Literals](#playground-literals)
  * [Trapping vs. Overflowing Arithmetic](#trapping-vs-overflowing-arithmetic)
  * [Defining New Operators](#defining-new-operators)
  * [Overloading Existing Operators](#overloading-existing-operators)
* [Documentation Comments](#documentation-comments)
  * [General Format](#general-format)
  * [Single-Sentence Summary](#single-sentence-summary)
  * [Parameter, Returns, and Throws Tags](#parameter-returns-and-throws-tags)
  * [Apple's Markup Format](#apples-markup-format)
  * [Where to Document](#where-to-document)

## Source File Basics

### File Names

All Swift source files end with the extension `.swift`.

In general, the name of a source file best describes the primary entity that it contains. A file that primarily contains a single type has the name of that type. A file that extends an existing type with protocol conformance is named with a combination of the type name and the protocol name, joined with a plus (`+`) sign. For more complex situations, exercise your best judgment.

For example:

* A file containing a single type `MyType` is named `MyType.swift`.
* A file containing a type `MyType` and some top-level helper functions is also named `MyType.swift`. (The top-level helpers are not the primary entity.)
* A file containing a single extension to a type `MyType` that adds conformance to a protocol `MyProtocol` is named `MyType+MyProtocol.swift`.
* A file containing multiple extensions to a type `MyType` that add conformances, nested types, or other functionality to a type can be named more generally, as long as it is prefixed with `MyType+`; for example, `MyType+Additions.swift`.
* A file containing related declarations that are not otherwise scoped under a common type or namespace (such as a collection of global mathematical functions) can be named descriptively; for example, `Math.swift`.

### File Encoding

Source files are encoded in UTF-8.

### Whitespace Characters

Aside from the line terminator, the Unicode horizontal space character (`U+0020`) is the only whitespace character that appears anywhere in a source file. The implications are:

* All other whitespace characters in string and character literals are represented by their corresponding escape sequence.
* Tab characters are not used for indentation.

### Special Escape Sequences

For any character that has a special escape sequence (`\t`, `\n`, `\r`, `\"`, `\'`, `\\`, and `\0`), that sequence is used rather than the equivalent Unicode (e.g., `\u{000a}`) escape sequence.

### Invisible Characters and Modifiers

Invisible characters, such as the zero width space and other control characters that do not affect the graphical representation of a string, are always written as Unicode escape sequences.

Control characters, combining characters, and variation selectors that _do_ affect the graphical representation of a string are not escaped when they are attached to a character or characters that they modify. If such a Unicode scalar is present in isolation or is otherwise not modifying another character in the same string, it is written as a Unicode escape sequence.

The strings below are well-formed because the umlauts and variation selectors associate with neighboring characters in the string. The second example is in fact composed of _five_ Unicode scalars, but they are unescaped because the specific combination is rendered as a single character.

✅ **Good:**
```swift
let size = "Übergröße"
let shrug = "🤷🏿‍♀️"
```

In the example below, the umlaut and variation selector are in strings by themselves, so they are escaped.

✅ **Good:**
```swift
let diaeresis = "\u{0308}"
let skinToneType6 = "\u{1F3FF}"
```

If the umlaut were included in the string literally, it would combine with the preceding quotation mark, impairing readability. Likewise, while most systems may render a standalone skin tone modifier as a block graphic, the example below is still forbidden because it is a modifier that is not modifying a character in the same string.

❌ **Bad:**
```swift
let diaeresis = "̈"
let skinToneType6 = "🏿"
```

### String Literals

Unicode escape sequences (`\u{????}`) and literal code points (for example, `Ü`) outside the 7-bit ASCII range are never mixed in the same string.

More specifically, string literals are either:

* composed of a combination of Unicode code points written literally and/or single character escape sequences (such as `\t`, but _not_ `\u{????}`), or
* composed of 7-bit ASCII with any number of Unicode escape sequences and/or other escape sequences.

The following example is correct because `\n` is allowed to be present among other Unicode code points.

✅ **Good:**
```swift
let size = "Übergröße\n"
```

The following example is allowed because it follows the rules above, but it is _not preferred_ because the text is harder to read and understand compared to the string above.

⚠️ **Not Preferred:**
```swift
let size = "\u{00DC}bergr\u{00F6}\u{00DF}e\n"
```

The example below is forbidden because it mixes code points outside the 7-bit ASCII range in both literal form and in escaped form.

❌ **Bad:**
```swift
let size = "Übergr\u{00F6}\u{00DF}e\n"
```

> **Aside:** Never make your code less readable simply out of fear that some programs might not handle non-ASCII characters properly. If that should happen, those programs are broken and must be fixed.

## Source File Structure

### File Comments

Comments describing the contents of a source file are optional. They are discouraged for files that contain only a single abstraction (such as a class declaration)—in those cases, the documentation comment on the abstraction itself is sufficient and a file comment is only present if it provides additional useful information. File comments are allowed for files that contain multiple abstractions in order to document that grouping as a whole.

### Import Statements

A source file imports exactly the top-level modules that it needs; nothing more and nothing less. If a source file uses definitions from both `UIKit` and `Foundation`, it imports both explicitly; it does not rely on the fact that some Apple frameworks transitively import others as an implementation detail.

Imports of whole modules are preferred to imports of individual declarations or submodules.

> There are a number of reasons to avoid importing individual members:
> 
> * There is no automated tooling to resolve/organize imports.
> * Existing automated tooling (such as Xcode's migrator) are less likely to work well on code that imports individual members because they are considered corner cases.
> * The prevailing style in Swift (based on official examples and community code) is to import entire modules.

Imports of individual declarations are permitted when importing the whole module would otherwise pollute the global namespace with top-level definitions (such as C interfaces). Use your best judgment in these situations.

Imports of submodules are permitted if the submodule exports functionality that is not available when importing the top-level module. For example, `UIKit.UIGestureRecognizerSubclass` must be imported explicitly to expose the methods that allow client code to subclass `UIGestureRecognizer`—those are not visible by importing `UIKit` alone.

Import statements are not line-wrapped.

Import statements are the first non-comment tokens in a source file. They are grouped in the following fashion, with each group separated by a blank line:

1. Module/submodule imports not under test
2. Individual declaration imports (`class`, `enum`, `func`, `struct`, `var`)
3. Modules imported with `@testable` (only present in test sources)

Within each group, imports are sorted lexicographically.

✅ **Good:**
```swift
import CoreLocation
import MyThirdPartyModule
import SpriteKit
import UIKit

import func Darwin.C.isatty

@testable import MyModuleUnderTest
```

### Type, Variable, and Function Declarations

In general, most source files contain only one top-level type, especially when the type declaration is large. Exceptions are allowed when it makes sense to include multiple related types in a single file. For example:

* A class and its delegate protocol may be placed in the same file.
* A type and its small related helper types may be placed in the same file. This can be useful when using a fileprivate helper type to implement a public type, or when multiple small types make up a single cohesive unit of functionality.

The order of types, variables, and functions in a source file, and the order of the members of those types, can have a great effect on readability. However, there is no single correct recipe for how to do it; different files and different types may order their contents in different ways.

What is important is that each file and type uses **some** logical order, which its maintainer could explain if asked. For example, new methods are not just habitually added to the end of the type, as that would yield "chronological by date added" ordering, which is not a logical ordering.

### Overloaded Declarations

When a type has multiple initializers or subscripts, or a file/type has multiple functions with the same base name (though perhaps with different argument labels), and when these overloads appear in the same type or extension scope, they appear sequentially with no other code in between.

### Extensions

Extensions can be used to organize functionality of a type across multiple "units." As with member ordering, the organization of extensions within a file and of members within each extension can have a great effect on readability. Again, there is no single correct recipe; different files and different types may order their contents in different ways, and the author should use judgment and be prepared to justify the organization.

## General Formatting

### Column Limit

Swift code has a column limit of 100 characters. Except as noted below, any line that would exceed this limit must be line-wrapped as described in Line-Wrapping.

**Exceptions:**

1. Lines where obeying the column limit is not possible without breaking a meaningful unit of text that should not be broken (for example, a long URL in a comment).
2. `import` statements.
3. Code generated by another tool.

### Braces

In general, braces follow Kernighan and Ritchie (K&R) style for non-empty blocks with the following refinements.

* There is no line break before the opening brace (`{`), unless that brace is the first character on a line.
* There is a line break after the opening brace (`{`), except
  * in closures, where the signature of the closure is placed on the same line as the curly brace, if it fits, and a line break follows the `in` keyword.
  * where it may be omitted as described in One Statement Per Line.
  * empty blocks may be written as `{}`.
* There is a line break before the closing brace (`}`), except where it may be omitted as described in One Statement Per Line, or it completes an empty block.
* There is a line break after the closing brace (`}`), if that brace terminates a statement or the body of a declaration. For example, there is no line break after the `}` if it is followed by `else` or `catch`, or if it is followed by a comma, semicolon, or closing parenthesis.

✅ **Good:**
```swift
func someMethod() -> Int {
  if x {
    doSomething()
  } else if y {
    doSomethingElse()
  } else {
    doSomethingDifferent()
  }

  return someValue
}

SomeClass {
  func someMethod() {}
}

let squares = numbers.map { $0 * $0 }

var someProperty: Int {
  get { return otherObject.property }
  set { otherObject.property = newValue }
}

var someProperty: Int { return otherObject.somethingElse() }

required init?(coder aDecoder: NSCoder) { fatalError("no coder") }
```

❌ **Bad:**
```swift
func someMethod() -> Int
{
  if x
  {
    doSomething()
  }
  else if y {
    doSomethingElse()
  }
  else {
    doSomethingDifferent()
  }

  return someValue
}

SomeClass {
  func someMethod()
  {}
}
```

### Semicolons

Semicolons (`;`) are not used, either to terminate or separate statements.

In other words, the only location where a semicolon may appear is inside a string literal or a comment.

✅ **Good:**
```swift
func printSum(_ a: Int, _ b: Int) {
  let sum = a + b
  print(sum)
}
```

❌ **Bad:**
```swift
func printSum(_ a: Int, _ b: Int) {
  let sum = a + b;
  print(sum);
}
```

❌ **Bad:**
```swift
func printSum(_ a: Int, _ b: Int) {
  let sum = a + b; print(sum)
}
```

### One Statement Per Line

There is at most one statement per line, and each statement is followed by a line break, except when the line ends with a block that also contains zero or one statements.

✅ **Good:**
```swift
guard let value = value else { return 0 }

defer { file.close() }

switch someEnum {
case .first: return 5
case .second: return 10
case .third: return 20
}

let squares = numbers.map { $0 * $0 }
var someProperty: Int { return otherObject.property }
```

❌ **Bad:**
```swift
guard let value = value else {
  return 0
}

defer {
  file.close()
}

switch someEnum {
case .first:
  return 5
case .second:
  return 10
case .third:
  return 20
}

let squares = numbers.map {
  $0 * $0
}
```

Wrapping the body of a single-statement block onto its own line is always allowed. Exercise best judgment when deciding whether to place a conditional statement and its body on the same line. For example, single line conditionals work well for early-return and basic cleanup tasks, but less so when the body contains a function call with significant logic. When in doubt, write it as a multi-line statement.

### Line-Wrapping

**Terminology Note:** Line-wrapping is the activity of dividing code into multiple lines that might otherwise legally occupy a single line.

For the purposes of Google Swift style, many declarations (such as type declarations and function declarations) and other expressions (such as function calls) can be partitioned into breakable units that are separated by unbreakable delimiting token sequences.

As an example, consider the following complex function declaration:

```swift
public func index<Elements: Collection, Element>(
  of element: Element,
  in collection: Elements
) -> Elements.Index?
where
  Elements.Element == Element,
  Element: Equatable
{
  // ...
}
```

This declaration is split into multiple lines by inserting line breaks between its breakable units. The rules for how these breaks are chosen in various contexts are described in the sections that follow.

In general:

1. Line-wrapping is permitted when the entire entity does not fit on a single line. The opening delimiter (for example, `(` in the example above) that follows the first breakable unit can stay attached to that unit, or can be placed on the following line.
2. Line breaks are inserted between any two breakable units such that the first unit is immediately followed by a comma (`,`), semicolon (`;`), or opening angle bracket (`<`).
3. When the entity is broken across multiple lines, the original breaks that existed in the first and second forms above are preserved.

✅ **Good:**
```swift
public func index<Elements: Collection, Element>(of element: Element,
                                                  in collection: Elements) -> Elements.Index?
    where Elements.Element == Element, Element: Equatable {
  // ...
}

public func index<Elements: Collection, Element>(
  of element: Element, in collection: Elements
) -> Elements.Index? where Elements.Element == Element, Element: Equatable {
  // ...
}

public func index<Elements: Collection, Element>(
  of element: Element, in collection: Elements) -> Elements.Index?
    where Elements.Element == Element, Element: Equatable {
  // ...
}
```

❌ **Bad:**
```swift
public func index<Elements: Collection, Element>(of element: Element, in collection: Elements) -> Elements.Index? where Elements.Element == Element, Element: Equatable {
  // ...
}

public func index<Elements: Collection, Element>(of element: Element, in collection: Elements) -> Elements.Index? where Elements.Element == Element, Element: Equatable { // ...
}
```

#### Function Declarations

When a function declaration does not fit on a single line, it is broken after each comma following a parameter. The opening brace (`{`) that follows a function declaration is always placed on the same line as the last line of the declaration, not on a line by itself.

✅ **Good:**
```swift
public func index<Elements: Collection, Element>(
  of element: Element,
  in collection: Elements
) -> Elements.Index? where Elements.Element == Element, Element: Equatable {
  // ...
}
```

❌ **Bad:**
```swift
public func index<Elements: Collection, Element>(
  of element: Element,
  in collection: Elements
) -> Elements.Index? where Elements.Element == Element, Element: Equatable
{
  // ...
}
```

#### Type and Extension Declarations

When a type or extension declaration does not fit on a single line, it is broken after each comma following a conformance/inheritance entry. Each conformance/inheritance entry following the first one is indented by 2 spaces.

✅ **Good:**
```swift
class MyClass:
  MySuperclass,
  MyProtocol,
  SomeoneElsesProtocol,
  SomeFrameworkProtocol
{
  // ...
}
```

❌ **Bad:**
```swift
class MyClass: MySuperclass, MyProtocol, SomeoneElsesProtocol,
    SomeFrameworkProtocol {
  // ...
}
```

#### Function Calls

When a function call does not fit on a single line, it is broken after each comma following an argument. The closing parenthesis (`)`) that follows the arguments is always placed on the same line as the last argument, not on a line by itself.

✅ **Good:**
```swift
let index = index(
  of: veryLongElementVariableName,
  in: aCollectionOfElementsThatAlsoHappensToHaveALongName)

let index = index(
  of: veryLongElementVariableName,
  in: aCollectionOfElementsThatAlsoHappensToHaveALongName
)
```

❌ **Bad:**
```swift
let index = index(
  of: veryLongElementVariableName,
  in: aCollectionOfElementsThatAlsoHappensToHaveALongName
  )
```

#### Control Flow Statements

When a control flow statement such as `if`, `guard`, `while`, or `switch` does not fit on a single line, it is broken after each comma following a condition. The opening brace (`{`) that follows a control flow statement is always placed on the same line as the last line of the statement, not on a line by itself.

✅ **Good:**
```swift
if let someValue = someOptional,
   let anotherValue = anotherOptional,
   let thirdValue = thirdOptional {
  // ...
}
```

❌ **Bad:**
```swift
if let someValue = someOptional,
   let anotherValue = anotherOptional,
   let thirdValue = thirdOptional
{
  // ...
}
```

#### Other Expressions

When line-wrapping other expressions that are not function declarations, type declarations, or function calls, the same rules apply as above. If such an expression contains a mix of these contexts, then the rules are applied to each part independently.

In the example below, the chain of member access expressions does not fit on a single line and is wrapped at each period (`.`).

✅ **Good:**
```swift
let result = numbers
  .filter { $0 > 0 }
  .map { $0 * 2 }
  .filter { $0 < 50 }
  .map { $0 * $0 }
```

❌ **Bad:**
```swift
let result = numbers.filter { $0 > 0 }.map { $0 * 2 }.filter { $0 < 50 }.map { $0 * $0 }
```

### Horizontal Whitespace

Beyond where required by the language or other style rules, and apart from literals and comments, a single Unicode space also appears in the following places **only**:

1. Separating any reserved word starting a conditional or switch statement (such as `if`, `guard`, `while`, or `switch`) from the expression that follows it if that expression starts with an open parenthesis (`(`).
2. Before any closing curly brace (`}`) that follows code on the same line, before any open curly brace (`{`), and after any open curly brace that is followed by code on the same line.
3. On both sides of any binary or ternary operator, including the "operator-like" symbols described below, with the following exceptions:
   * The dot (`.`) used to reference members.
   * The comma (`,`) if it is used in a list of three or more items.
   * The colon (`:`) when used in a dictionary literal.
   * The double slash (`//`) that begins an end-of-line comment and the slash-asterisk (`/*`) that begins a block comment.
   * The ampersand (`&`) in a protocol composition type.
4. After a comma (`,`) or colon (`:`) if it is used in a list of three or more items.
5. After the double slash (`//`) that begins an end-of-line comment.
6. On both sides of the double slash (`//`) or slash-asterisk (`/*`) and asterisk-slash (`*/`) that begin and end a documentation block comment.

For the purposes of this rule, "operator-like symbols" are any of the following:

* `=` in any context
* `&` as a prefix operator in any context
* `==`, `!=`, `<`, `>`, `<=`, `>=` in any context
* `?` and `:` in the ternary conditional operator
* `->` wherever it appears
* `?`, `!`, and `as` in any postfix/infix position

✅ **Good:**
```swift
let x = 5
var numbers = [1, 2, 3]
let y = x ? 1 : 0
let z = numbers[0]
let d = ["John": 30, "Jane": 29]
let next = current + 1
let previous = current - 1
let quotient = current / 2
let product = current * 2
let remainder = current % 2
let result = condition ? 1 : 0
let squared = current * current
let (quotient, remainder) = x.quotientAndRemainder(dividingBy: 2)
let chain = a || b || c
let chain2 = a && b && c
let result = a ? b : c ? d : e
```

❌ **Bad:**
```swift
let x=5
var numbers = [1,2,3]
let y = x?1:0
let z = numbers [0]
let d = ["John" : 30, "Jane" : 29]
let next = current+1
let previous = current -1
let quotient = current/ 2
let product = current *2
let remainder = current% 2
let result = condition? 1 : 0
let squared = current*current
let (quotient,remainder) = x.quotientAndRemainder(dividingBy: 2)
let chain = a||b||c
let chain2 = a&&b&&c
let result = a ? b : c?d:e
```

### Horizontal Alignment

**Terminology Note:** Horizontal alignment is the practice of adding a variable number of additional spaces in your code with the goal of making certain tokens appear directly below certain other tokens on previous lines.

Horizontal alignment is forbidden except when writing obviously tabular data where omitting the alignment would be harmful to readability. In other cases (for example, lining up the types of stored properties within a `struct` or the `=` signs of variable assignments in a block) horizontal alignment is an invitation for maintenance problems if a new member is introduced that requires every other member to be realigned.

✅ **Good:**
```swift
struct DataPoint {
  var value: Int
  var primaryColor: UIColor
}

let john = Human("John", 30)
let jane = Human("Jane", 29)
```

❌ **Bad:**
```swift
struct DataPoint {
  var value:        Int
  var primaryColor: UIColor
}

let john = Human("John", 30)
let jane = Human("Jane", 29)
```

### Vertical Whitespace

A single blank line appears in the following locations:

1. Between consecutive members of a type: properties, initializers, methods, enums, and nested types, except that:
   * A blank line is optional between two consecutive stored properties or two enum cases whose declarations fit entirely on a single line. Such blank lines can be used to create logical groupings of these declarations.
   * A blank line is optional between two extremely closely related items (such as a private stored property and a related computed property that provides public access to it).
2. As required by other sections of this document (such as Import Statements).
3. Anywhere inside a type or extension declaration, or at file scope, to divide code into logical subsections.

Multiple consecutive blank lines are permitted, but never required (nor encouraged).

### Parentheses

Parentheses are not used around the top-most expression that follows an `if`, `guard`, `while`, or `switch` keyword, or in an optional binding expression of an `if` or `while` statement.

✅ **Good:**
```swift
if x == 0 {
  print("x is zero")
}

if let y = y {
  print(y)
}
```

❌ **Bad:**
```swift
if (x == 0) {
  print("x is zero")
}

if let y = (y) {
  print(y)
}
```

Optional grouping parentheses are omitted only when the author and reviewer agree that there is no reasonable chance that the code will be misinterpreted without them, nor that they would have made the code easier to read. It is not reasonable to assume that every reader has the entire Swift operator precedence table memorized.

## Formatting Specific Constructs

### Non-Documentation Comments

Non-documentation comments always use the double-slash format (`//`), never the C-style block format (`/* ... */`).

### Properties

Local variables are declared close to the point at which they are first used (within reason) to minimize their scope.

With the exception of tuple destructuring, every `let` or `var` statement declares exactly one variable.

✅ **Good:**
```swift
var a = 5
var b = 10

let (quotient, remainder) = x.quotientAndRemainder(dividingBy: 2)
```

❌ **Bad:**
```swift
var a = 5, b = 10
```

### Switch Statements

Case statements are indented at the same level as the switch statement to which they belong; the statements inside the case blocks are then indented +2 spaces from that level.

✅ **Good:**
```swift
switch order {
case .ascending:
  print("Ascending")
case .descending:
  print("Descending")
case .same:
  print("Same")
}
```

❌ **Bad:**
```swift
switch order {
  case .ascending:
    print("Ascending")
  case .descending:
    print("Descending")
  case .same:
    print("Same")
}
```

❌ **Bad:**
```swift
switch order {
case .ascending:
print("Ascending")
case .descending:
print("Descending")
case .same:
print("Same")
}
```

### Enum Cases

In general, there is only one `case` per line in an `enum`. The comma-delimited form may be used only when none of the cases have associated values or raw values, all cases fit on a single line, and the cases do not need further documentation because their meanings are obvious from their names.

✅ **Good:**
```swift
public enum Token {
  case comma
  case semicolon
  case identifier(String)
}

public enum Token {
  case comma, semicolon
  case identifier(String)
}

public enum Token {
  case comma, semicolon, identifier(String)
}
```

❌ **Bad:**
```swift
public enum Token {
  case comma, semicolon,
       identifier(String)
}
```

When all cases of an `enum` fit on a single line, and when there are no other members in the declaration, cases can be written on the same line as the enum declaration. Otherwise, each case is written on its own line.

✅ **Good:**
```swift
public enum Result { case success, failure }

public enum OrderDirection {
  case ascending
  case descending
}

public enum OrderDirection {
  case ascending
  case descending

  var reversed: OrderDirection {
    switch self {
    case .ascending:
      return .descending
    case .descending:
      return .ascending
    }
  }
}
```

❌ **Bad:**
```swift
public enum Result {
  case success, failure
}

public enum OrderDirection { case ascending, descending

  var reversed: OrderDirection {
    switch self {
    case .ascending:
      return .descending
    case .descending:
      return .ascending
    }
  }
}
```

### Trailing Closures

Functions should not be overloaded such that two overloads differ only by the name of their trailing closure argument. Doing so prevents using trailing closure syntax—when the label is not present at the call site, a call to the function with a trailing closure is ambiguous.

✅ **Good:**
```swift
func greet(enthusiastically: Bool, then continuation: () -> Void) {
  // ...
}

func greet(mildly: Bool, then continuation: () -> Void) {
  // ...
}
```

❌ **Bad:**
```swift
func greet(enthusiastically: Bool, completion: () -> Void) {
  // ...
}

func greet(enthusiastically: Bool, then continuation: () -> Void) {
  // ...
}
```

If a function call has multiple closure arguments, none of them are called using trailing closure syntax; all are labeled and nested inside the argument list's parentheses.

**Exception:** If a function has multiple closure arguments and the final one is syntactically required to be a trailing closure because it is an autoclosure, then the penultimate closure may also be written as a trailing closure to avoid the inconsistency of having adjacent closure expressions with different delimiters.

✅ **Good:**
```swift
UIView.animate(
  withDuration: 0.3,
  animations: {
    // ...
  },
  completion: { finished in
    // ...
  })
```

❌ **Bad:**
```swift
UIView.animate(
  withDuration: 0.3,
  animations: {
    // ...
  }) { finished in
    // ...
  }
```

If a function has a single closure argument and it is the final argument, it is called using trailing closure syntax except in the following cases:

1. The name of the closure argument is not omitted at the call site.
2. The body of the closure consists of a single expression that is also a function call, and the closure argument label is semantically meaningful. In this case, the authors have a choice between using trailing closure syntax or not, depending on whether the two function names read better together with the label or without.

✅ **Good:**
```swift
let squares = [1, 2, 3].map { $0 * $0 }

let squares = [1, 2, 3].map({ $0 * $0 })

let sorted = names.sorted(by: <)

let sorted = names.sorted { $0 < $1 }
```

### Trailing Commas

Trailing commas in array and dictionary literals are optional. When such a literal spans multiple lines, the author has a choice of whether to use trailing commas or not; our style does not dictate one over the other.

✅ **Good:**
```swift
let units = [
  "meter",
  "centimeter",
  "millimeter",
]

let units = [
  "meter",
  "centimeter",
  "millimeter"
]
```

### Numeric Literals

Long numeric literals use underscores (`_`) to group digits for readability when it improves clarity.

It is recommended to group digits in numeric literals into groups of three for decimal (base-10) literals, or four (or two) for hexadecimal (base-16) literals. If such a grouping is not appropriate for a particular use case, use your best judgment.

Do not group digits if the literal is text that represents an identifier that is never displayed to the user, such as a phone number, credit card number, social security number, etc.

✅ **Good:**
```swift
let filePermissions = 0o644
let exampleNumber = 1_234.567_89
let hexColors = [
  0x00_00_00,
  0x00_00_FF,
  0x00_FF_00,
  0x00_FF_FF,
  0xFF_00_00,
  0xFF_00_FF,
  0xFF_FF_00,
  0xFF_FF_FF,
]

let phoneNumber = 5551234567
```

### Attributes

Parameterized attributes (such as `@available(...)` or `@objc(...)`) are written on the same line as the declaration to which they apply, or on the immediately preceding line if they do not fit on the same line. Non-parameterized attributes (such as `@objc` without arguments) may be placed on the same line if they would fit.

✅ **Good:**
```swift
@available(iOS 9.0, *)
public func coolNewFeature() {
  // ...
}

@objc public class MyClass {
  // ...
}
```

❌ **Bad:**
```swift
@available(iOS 9.0, *) public func coolNewFeature() {
  // ...
}

@objc
public class MyClass {
  // ...
}
```

## Naming

### Apple's API Style Guidelines

Swift's API Design Guidelines are considered part of this style guide. Familiarize yourself with them and follow their guidance. There are some additions below that are specific to Google Swift style.

### Naming Conventions Are Not Access Control

Using a leading underscore to distinguish private/fileprivate members from internal/public members is forbidden. Access control in Swift is specified with keywords, not naming conventions.

✅ **Good:**
```swift
public class MyClass {
  public var publicVariable = 0
  var internalVariable = 0
  private var privateVariable = 0
}
```

❌ **Bad:**
```swift
public class MyClass {
  public var publicVariable = 0
  var _internalVariable = 0
  private var _privateVariable = 0
}
```

### Identifiers

In general, identifiers contain only 7-bit ASCII characters. Unicode identifiers are allowed if they have a clear and legitimate meaning in the problem domain of the code (for example, Greek letters that represent mathematical concepts) and are well-understood by the team who owns the code.

✅ **Good:**
```swift
let smile = "😊"
let deltaX = newX - previousX
let Δx = newX - previousX
```

❌ **Bad:**
```swift
let 😊 = "😊"
```

### Initializers

For clarity, initializer arguments that correspond directly to a stored property have the same name as the property. Explicit `self.` is used during assignment to disambiguate them.

✅ **Good:**
```swift
public struct Person {
  public let name: String
  public let phoneNumber: String

  public init(name: String, phoneNumber: String) {
    self.name = name
    self.phoneNumber = phoneNumber
  }
}
```

❌ **Bad:**
```swift
public struct Person {
  public let name: String
  public let phoneNumber: String

  public init(name otherName: String, phoneNumber otherPhoneNumber: String) {
    name = otherName
    phoneNumber = otherPhoneNumber
  }
}
```

### Static and Class Properties

Static and class properties that return instances of the declaring type are not suffixed with the type name.

✅ **Good:**
```swift
public class UIColor {
  public class var red: UIColor {
    // ...
  }
}

public class URLSession {
  public class var shared: URLSession {
    // ...
  }
}
```

❌ **Bad:**
```swift
public class UIColor {
  public class var redColor: UIColor {
    // ...
  }
}

public class URLSession {
  public class var sharedSession: URLSession {
    // ...
  }
}
```

When a static or class property evaluates to a singleton instance of the declaring type, the names `shared` and `default` are commonly used. The former is better for situations where other instances can be created by the user.

### Global Constants

Global constants follow the same naming conventions as other constants; they are written in lowerCamelCase. Hungarian notation, such as a leading `g` or `k`, is not used.

✅ **Good:**
```swift
let secondsPerMinute = 60
```

❌ **Bad:**
```swift
let SecondsPerMinute = 60
let kSecondsPerMinute = 60
let gSecondsPerMinute = 60
let SECONDS_PER_MINUTE = 60
```

### Delegate Methods

Methods on delegate protocols and delegate-like protocols (such as data sources) are named using the linguistic syntax described below, which is inspired by Cocoa's protocols.

> The term "delegate's source object" refers to the object that invokes methods on the delegate. For example, a `UITableView` is the source object that invokes methods on the `UITableViewDelegate` that is set as its `delegate` property.

All methods take the delegate's source object as the first argument.

For methods that take the delegate's source object as their **only** argument:

* If the method returns `Void` (such as those used to notify the delegate that an event has occurred), then the method's base name is the delegate's source type followed by an indicative verb phrase describing the event. The argument is unlabeled.

✅ **Good:**
```swift
func scrollViewDidBeginScrolling(_ scrollView: UIScrollView)
```

* If the method returns `Bool` (such as those that make an assertion about the delegate's source object itself), then the method's name is the delegate's source type followed by an indicative or conditional verb phrase describing the assertion. The argument is unlabeled.

✅ **Good:**
```swift
func scrollViewShouldScrollToTop(_ scrollView: UIScrollView) -> Bool
```

* If the method returns some other value (such as those querying for information about a property of the delegate's source object), then the method's base name is a noun phrase describing the property being queried. The argument is labeled with a preposition or phrase with a trailing preposition that appropriately combines the noun phrase and the delegate's source object.

✅ **Good:**
```swift
func numberOfSections(in scrollView: UIScrollView) -> Int
```

For methods that take **additional** arguments after the delegate's source object, the method's base name is the delegate's source type by itself and the first argument is unlabeled. Then:

* If the method returns `Void`, the second argument is labeled with an indicative verb phrase describing the event that has the argument as its direct object or prepositional object, and any other arguments (if present) provide further context.

✅ **Good:**
```swift
func tableView(
  _ tableView: UITableView,
  willDisplayCell cell: UITableViewCell,
  forRowAt indexPath: IndexPath)
```

* If the method returns `Bool`, the second argument is labeled with an indicative or conditional verb phrase that describes the return value in terms of the argument, and any other arguments (if present) provide further context.

✅ **Good:**
```swift
func tableView(
  _ tableView: UITableView,
  shouldSpringLoadRowAt indexPath: IndexPath,
  with context: UISpringLoadedInteractionContext
) -> Bool
```

* If the method returns some other value, the second argument is labeled with a noun phrase and preposition that describes the return value in terms of the argument, and any other arguments (if present) provide further context.

✅ **Good:**
```swift
func tableView(
  _ tableView: UITableView,
  heightForRowAt indexPath: IndexPath
) -> CGFloat
```

Apple's documentation on [delegates and data sources](https://developer.apple.com/library/content/documentation/General/Conceptual/CocoaEncyclopedia/DelegatesandDataSources/DelegatesandDataSources.html) also contains some good general guidance about such names.

## Programming Practices

### Compiler Warnings

Code should compile without warnings when feasible. Any warnings that are able to be removed easily by the author must be removed.

A reasonable exception is deprecation warnings, where it may not be possible to immediately migrate to the replacement API, or where an API may be deprecated for external users but must still be supported inside a library during a deprecation period.

### Initializers

For `struct`s, Swift synthesizes a non-public memberwise initializer for use by the module and its submodules. When that initializer is suitable (that is, when it assigns values to all stored properties without further validation or logic), it is used rather than implementing a custom one that would result in the same behavior.

The initializers declared by a type should form a funnel, where convenience initializers (those that do not designate) eventually delegate to a designated initializer. This consistency makes it easier for maintainers to understand the flow of initialization and identify any problems that may occur.

✅ **Good:**
```swift
struct Foo {
  let bar: Int
  let baz: String

  init(bar: Int, baz: String) {
    self.bar = bar
    self.baz = baz
  }

  init(bar: Int) {
    self.init(bar: bar, baz: "default")
  }
}
```

❌ **Bad:**
```swift
struct Foo {
  let bar: Int
  let baz: String

  init(bar: Int, baz: String) {
    self.bar = bar
    self.baz = baz
  }

  init(bar: Int) {
    self.bar = bar
    self.baz = "default"
  }
}
```

### Properties

The `get` block for read-only computed properties is omitted and the body is written directly inside the property declaration unless it is necessary to document the behavior of the getter with one or more comments inside the block.

✅ **Good:**
```swift
var totalCost: Int {
  return items.sum { $0.cost }
}
```

❌ **Bad:**
```swift
var totalCost: Int {
  get {
    return items.sum { $0.cost }
  }
}
```

### Types with Shorthand Names

Arrays, dictionaries, and optional types are written in their shorthand form whenever possible; that is, `[Element]`, `[Key: Value]`, and `Wrapped?`. The long forms `Array<Element>`, `Dictionary<Key, Value>`, and `Optional<Wrapped>` are only written when required by the compiler; for example, `Swift.Array<Element>` is required in the extension `extension Swift.Array where Element == Int`.

✅ **Good:**
```swift
func enumerateValues(in array: [Int]) {
  // ...
}

func add(value: Int, to dictionary: inout [String: Int]) {
  // ...
}

func unionOptionals<T>(_ a: T?, _ b: T?) -> T? {
  // ...
}
```

❌ **Bad:**
```swift
func enumerateValues(in array: Array<Int>) {
  // ...
}

func add(value: Int, to dictionary: inout Dictionary<String, Int>) {
  // ...
}

func unionOptionals<T>(_ a: Optional<T>, _ b: Optional<T>) -> Optional<T> {
  // ...
}
```

**Exception:** When a type name inside a module name is required by the compiler to disambiguate, the long form is used even when it would otherwise be prohibited. If only the module name is needed to disambiguate, it is prefixed to the type's short form.

✅ **Good:**
```swift
extension MyModule.Array {
  // ...
}

extension Swift.Array where Element == Int {
  // ...
}
```

❌ **Bad:**
```swift
extension Array<Element> {
  // ...
}
```

Void is a `typealias` for the empty tuple `()`, so from a user's perspective they are equivalent. In function type declarations (such as closures, or variables holding a function reference), the return type is always written as `Void`, never as `()`. In functions declared with the `func` keyword, the `Void` return type is omitted entirely.

Empty argument lists are always written as `()`, never as `Void`.

✅ **Good:**
```swift
func doSomething() {
  // ...
}

let callback: () -> Void
```

❌ **Bad:**
```swift
func doSomething() -> Void {
  // ...
}

func doSomething2() -> () {
  // ...
}

let callback: () -> ()
```

### Optional Types

Sentinel values are avoided when designing algorithms (for example, an "index" of `-1` when an element was not found in a collection). Sentinel values can easily and accidentally propagate through other layers of logic because the type system cannot distinguish between them and valid outcomes.

`nil` (and types that wrap it, such as `Optional`) are used as sentinel values instead. Since Swift makes non-optional types explicit, it is clear by inspection which functions and properties can never be `nil` and which require additional handling.

✅ **Good:**
```swift
func index(of element: Element) -> Int?

if let index = index(of: someElement) {
  // Found at index
} else {
  // Not found
}
```

❌ **Bad:**
```swift
func index(of element: Element) -> Int  // Returns -1 if not found

let index = index(of: someElement)
if index != -1 {
  // Found at index
} else {
  // Not found
}
```

Optional types are unboxed using optional binding via `if let`, `guard let`, and `switch` statements. Optional chaining or forced unwrapping may be used in some cases where it improves readability. Highly chained expressions (such as `foo?.bar?.baz?.qux`) are avoided in favor of optional binding with clear variable names.

✅ **Good:**
```swift
if let value = optionalValue {
  doSomething(with: value)
}

guard let value = optionalValue else {
  return
}

switch optionalValue {
case .some(let value):
  doSomething(with: value)
case .none:
  doSomethingElse()
}
```

✅ **Good:**
```swift
if case let .some(value) = someOptional {
  doSomething(with: value)
}
```

❌ **Bad:**
```swift
if let value = someOptional {
  doSomething(with: value)
}
```

### Error Types

Error types are used for errors that may occur during the execution of a program that callers might reasonably want to handle. The domain and range of a function that can throw are well-described by its API documentation.

Errors are caught using `do`-`catch` statements. Errors are not caught using `try?` unless the specific error condition is not important in the specific context (for example, logging a failure is not important when the operation is already being retried). The error is always handled or propagated; `try!` is never used to call a throwing function whose error condition is unhandled.

✅ **Good:**
```swift
do {
  try somethingThatMightThrow()
} catch {
  // Handle error
}

if let result = try? somethingThatMightThrow() {
  // Use result
}
```

❌ **Bad:**
```swift
try! somethingThatMightThrow()
```

### Force Unwrapping and Force Casts

Force unwrapping and force casting are often code smells and are strongly discouraged. Unless it is extremely clear from surrounding code why such an operation is safe, a comment is required that describes the invariant that ensures that the operation is safe.

**Exception:** Force casts are allowed in test code without additional documentation. This keeps such code free of unnecessary control flow. If the cast fails, the test will fail, which is the desired result.

✅ **Good:**
```swift
let value = getSomeInteger()

// `value` is guaranteed to be divisible by 2 because ...
let half = value / 2

// `value` is guaranteed to be non-negative because ...
let bits = UInt32(value)
```

❌ **Bad:**
```swift
let value = getSomeInteger()
let half = value / 2  // No guarantee about divisibility
let bits = UInt32(value)  // No guarantee about sign
```

### Implicitly Unwrapped Optionals

Implicitly unwrapped optionals are inherently unsafe and should be avoided whenever possible in favor of non-optional declarations or regular `Optional` types. Exceptions are described below.

In some cases, a variable cannot be initialized with a non-nil value at the point of its declaration, but it can be guaranteed to be non-nil before it is used. In these cases, it is acceptable to use an implicitly unwrapped optional, but a regular optional combined with appropriate unwrapping is often a better approach.

✅ **Good:**
```swift
class MyViewController: UIViewController {
  @IBOutlet var button: UIButton!

  override func viewDidLoad() {
    super.viewDidLoad()
    populateUI()
  }

  private func populateUI() {
    button.setTitle("Hello", for: .normal)
  }
}
```

### Access Levels

Omitting an explicit access level is permitted on declarations. For top-level declarations, the default access level is `internal`. For nested declarations, the default access level is the same as the access level of the enclosing declaration.

Specifying an explicit access level at the extension level is forbidden when the extension is used only to group related members together. Access levels are specified on individual members within the extension.

✅ **Good:**
```swift
extension String {
  public var isUppercase: Bool {
    // ...
  }

  public var isLowercase: Bool {
    // ...
  }
}
```

❌ **Bad:**
```swift
public extension String {
  var isUppercase: Bool {
    // ...
  }

  var isLowercase: Bool {
    // ...
  }
}
```

Extensions that exist solely to adopt a protocol may have an access level applied to the entire group. The access level of individual members within such extensions is not specified unless it differs from that of the extension.

✅ **Good:**
```swift
public extension MyType: Equatable {
  static func == (lhs: MyType, rhs: MyType) -> Bool {
    // ...
  }
}
```

### Nesting and Namespacing

Swift allows `enum`s, `struct`s, and `class`es to be nested, which provides natural namespacing for scoped types. Nesting should reflect the natural relationship of the nested types.

✅ **Good:**
```swift
class Parser {
  enum Error: Swift.Error {
    case invalidToken(String)
    case unexpectedEOF
  }

  func parse(text: String) throws {
    // ...
  }
}
```

❌ **Bad:**
```swift
class Parser {
  func parse(text: String) throws {
    // ...
  }
}

enum ParseError: Error {
  case invalidToken(String)
  case unexpectedEOF
}
```

### guards for Early Exits

`guard` statements improve readability by eliminating extra levels of nesting (the "pyramid of doom"); failure conditions are closely coupled to the conditions that they are guarding, and the happy path proceeds down the left-hand edge.

✅ **Good:**
```swift
func discombobulate(_ values: [Int]) throws -> Int {
  guard let first = values.first else {
    throw DiscombobulationError.arrayWasEmpty
  }
  guard first >= 0 else {
    throw DiscombobulationError.negativeEnergy
  }

  var result = 0
  for value in values {
    result += invertedCombobulatoryFactory(of: value)
  }
  return result
}
```

❌ **Bad:**
```swift
func discombobulate(_ values: [Int]) throws -> Int {
  if let first = values.first {
    if first >= 0 {
      var result = 0
      for value in values {
        result += invertedCombobulatoryFactor(of: value)
      }
      return result
    } else {
      throw DiscombobulationError.negativeEnergy
    }
  } else {
    throw DiscombobulationError.arrayWasEmpty
  }
}
```

### for-where Loops

When the entirety of a `for` loop's body would be a single `if` block testing a condition of the element, the test is placed in the `where` clause of the `for` statement instead.

✅ **Good:**
```swift
for item in collection where item.hasProperty {
  // ...
}
```

❌ **Bad:**
```swift
for item in collection {
  if item.hasProperty {
    // ...
  }
}
```

### fallthrough in switch Statements

When multiple `case`s of a `switch` would execute the same statements, the `case` patterns are combined into a compound case. Multiple `case` statements that do nothing but `fallthrough` to a `case` below are not allowed.

✅ **Good:**
```swift
switch value {
case 1:
  doSomething()
case 2, 3, 4:
  doSomethingElse()
default:
  doDefaultThing()
}
```

❌ **Bad:**
```swift
switch value {
case 1:
  doSomething()
case 2:
  fallthrough
case 3:
  fallthrough
case 4:
  doSomethingElse()
default:
  doDefaultThing()
}
```

### Pattern Matching

The `let` and `var` keywords are placed individually in front of each element in a pattern that binds a variable, rather than grouping all of the bindings into a single `let`/`var` at the beginning of the pattern.

✅ **Good:**
```swift
switch point {
case (let x, let y):
  print("(\(x), \(y))")
}

switch value {
case let .number(x):
  print(x)
}
```

❌ **Bad:**
```swift
switch point {
case let (x, y):
  print("(\(x), \(y))")
}

switch value {
case .number(let x):
  print(x)
}
```

Shorthand pattern matching over optional values is preferred in `case` statements over boilerplate `if let` unboxing.

✅ **Good:**
```swift
switch someOptional {
case let .some(value):
  doSomething(with: value)
case .none:
  doSomethingElse()
}
```

✅ **Good:**
```swift
if case let .some(value) = someOptional {
  doSomething(with: value)
}
```

❌ **Bad:**
```swift
if let value = someOptional {
  doSomething(with: value)
}
```

### Tuple Patterns

Assigning variables through a tuple pattern (sometimes referred to as a tuple shuffle) is only permitted if the left-hand side of the assignment is unlabeled.

✅ **Good:**
```swift
let (a, b) = (y: 4, x: 5.0)
```

❌ **Bad:**
```swift
let (x: a, y: b) = (y: 4, x: 5.0)
```

Labels on tuple patterns are forbidden because they can lead to confusing code. The examples below illustrate this point.

```swift
// This declares two variables, `a` and `b`.
let (a, b) = (y: 4, x: 5.0)

// This declares two variables, `y` and `x`.
let (y, x) = (y: 4, x: 5.0)

// This declares two variables, `a` and `b`.
// `x` and `y` are labels.
let (x: a, y: b) = (y: 4, x: 5.0)
```

### Numeric and String Literals

Integer and string literals in Swift do not have an inherent type. For example, `5` by itself is not an `Int`; it is a special literal value that can express any type that conforms to `ExpressibleByIntegerLiteral` and only becomes an `Int` if type inference does not map it to a more specific type. Likewise, literals like `5.0` and `"hello"` are mapped to `Double` and `String` only by default, but they can express any type conforming to the corresponding protocol.

Thus, when a literal is used to initialize a value of a type other than its default, and when that type cannot be inferred from context, specify the type explicitly in the declaration or use an `as` expression to coerce it.

✅ **Good:**
```swift
// Without a more explicit type, x1 would be inferred to have type Int.
let x1: Int8 = 5

// Without a more explicit type, y1 would be inferred to have type Double.
let y1: Float = 5.0

// Without a more explicit type, z1 would be inferred to have type String.
let z1: StaticString = "a string"

// With type inference, x2 has type [Int8] because that is the type of the
// elements in the array literal.
let x2 = [5 as Int8, 3, 2]

// With type inference, y2 has type [Float] because that is the type of the
// elements in the array literal.
let y2 = [5.0 as Float, 3.0, 2.0]

// With type inference, z2 has type [StaticString] because that is the type of
// the elements in the array literal.
let z2 = ["a string" as StaticString, "another string"]
```

Using the literal's default type is always preferred when it is possible to do so. Conversions using initializer syntax are only used when the default type is unsuitable.

✅ **Good:**
```swift
let x = 50  // Int
let y = 50.0  // Double
let z = "some string"  // String
```

❌ **Bad:**
```swift
let x = Int(50)
let y = Double(50.0)
let z = String("some string")
```

### Playground Literals

The graphical playground literals `#colorLiteral(...)`, `#imageLiteral(...)`, and `#fileLiteral(...)` are forbidden in non-playground code.

### Trapping vs. Overflowing Arithmetic

The standard arithmetic operators `+`, `-`, `*`, `<<`, and `>>` are used for most normal arithmetic and shifts. The operators crash on overflow or underflow, which is the safest default behavior.

If the special semantics of overflow/underflow are needed (for example, when implementing a hashing algorithm), the masking operators (`&+`, `&-`, `&*`, `&<<`, `&>>`) are used instead. However, a comment should be present to explain why the use of these operators is required.

### Defining New Operators

Use of custom operators is strongly discouraged. It is much easier to understand a named function than a cryptic symbol.

If a custom operator is declared, the reasoning for it must be clearly documented. Use your best judgment when deciding whether the operator you are defining is appropriate for the given situation, understanding that you should have a high bar for doing so.

### Overloading Existing Operators

Overloading operators is permitted when your use of the operator is semantically equivalent to the existing uses. Examples of permitted use:

* `==` and other comparison operators to make types `Equatable`, `Comparable`, etc.
* Arithmetic operators for numeric types or analogous operations (such as defining `+` for concatenation on a custom string-like type, or `|` for a set union).

Overloading operators is strongly discouraged in cases where the semantics are not equivalent. For example:

* The `<<` and `>>` operators are reserved for bitwise shifting; they should not be repurposed for other functionality (such as collection insertion or function composition).
* Division (`/`) should not be repurposed for path building.

## Documentation Comments

### General Format

Documentation comments are written using the `///` syntax (three slashes, no asterisks).

✅ **Good:**
```swift
/// Returns the numeric value of the given digit represented as a Unicode scalar.
///
/// - Parameters:
///   - digit: The Unicode scalar whose numeric value should be returned.
///   - radix: The radix, between 2 and 36, used to compute the numeric value.
/// - Returns: The numeric value of the scalar.
func numericValue(of digit: UnicodeScalar, radix: Int = 10) -> Int {
  // ...
}
```

❌ **Bad:**
```swift
/**
 * Returns the numeric value of the given digit represented as a Unicode scalar.
 *
 * - Parameters:
 *   - digit: The Unicode scalar whose numeric value should be returned.
 *   - radix: The radix, between 2 and 36, used to compute the numeric value.
 * - Returns: The numeric value of the scalar.
 */
func numericValue(of digit: UnicodeScalar, radix: Int = 10) -> Int {
  // ...
}

/**
Returns the numeric value of the given digit represented as a Unicode scalar.

- Parameters:
  - digit: The Unicode scalar whose numeric value should be returned.
  - radix: The radix, between 2 and 36, used to compute the numeric value.
- Returns: The numeric value of the scalar.
*/
func numericValue(of digit: UnicodeScalar, radix: Int = 10) -> Int {
  // ...
}
```

### Single-Sentence Summary

Documentation comments begin with a brief **single-sentence** summary that describes the declaration. (This sentence may span multiple lines, but if it spans too many lines, the author should consider whether the summary can be simplified and details moved to a new paragraph.)

If more detail is needed than can be stated in the summary, additional paragraphs (each separated by a blank line) are added after it.

The single-sentence summary is not necessarily a complete sentence; for example, method summaries are generally written as verb phrases **without** "this method [...]" because it is already implied as the subject and writing it out would be redundant. Likewise, properties are often written as noun phrases **without** "this property is [...]". In any case, however, they are still terminated with a period.

✅ **Good:**
```swift
/// The background color of the view.
var backgroundColor: UIColor

/// Returns the sum of the numbers in the given array.
///
/// - Parameter numbers: The numbers to sum.
/// - Returns: The sum of the numbers.
func sum(_ numbers: [Int]) -> Int {
  // ...
}
```

❌ **Bad:**
```swift
/// This property is the background color of the view.
var backgroundColor: UIColor

/// This method returns the sum of the numbers in the given array.
///
/// - Parameter numbers: The numbers to sum.
/// - Returns: The sum of the numbers.
func sum(_ numbers: [Int]) -> Int {
  // ...
}
```

### Parameter, Returns, and Throws Tags

Clearly document the parameters, return value, and thrown errors of functions using the `Parameter(s)`, `Returns`, and `Throws` tags, in that order. None ever appears with an empty description. When a description does not fit on a single line, continuation lines are indented 2 spaces in from the position of the hyphen starting the tag.

The recommended way to write documentation comments in Xcode is to place the text cursor on the declaration and press **Command + Option + /**. This will automatically generate the correct format with placeholders to be filled in.

`Parameter(s)` and `Returns` tags may be omitted only if the single-sentence brief summary fully describes the meaning of those items and including the tags would only repeat what has already been said.

The content following the `Parameter(s)`, `Returns`, and `Throws` tags should be terminated with a period, even when they are phrases instead of complete sentences.

When a method takes a single argument, the singular inline form of the `Parameter` tag is used. When a method takes multiple arguments, the grouped plural form `Parameters` is used and each argument is written as an item in a nested list with only its name as the tag.

✅ **Good:**
```swift
/// Returns the output generated by executing a command.
///
/// - Parameter command: The command to execute in the shell environment.
/// - Returns: A string containing the contents of the invoked process's
///   standard output.
func execute(command: String) -> String {
  // ...
}

/// Returns the output generated by executing a command with the given string
/// used as standard input.
///
/// - Parameters:
///   - command: The command to execute in the shell environment.
///   - stdin: The string to use as standard input.
/// - Returns: A string containing the contents of the invoked process's
///   standard output.
func execute(command: String, stdin: String) -> String {
  // ...
}
```

The following examples are incorrect, because they use the plural form of `Parameters` for a single parameter or the singular form `Parameter` for multiple parameters.

❌ **Bad:**
```swift
/// Returns the output generated by executing a command.
///
/// - Parameters:
///   - command: The command to execute in the shell environment.
/// - Returns: A string containing the contents of the invoked process's
///   standard output.
func execute(command: String) -> String {
  // ...
}

/// Returns the output generated by executing a command with the given string
/// used as standard input.
///
/// - Parameter command: The command to execute in the shell environment.
/// - Parameter stdin: The string to use as standard input.
/// - Returns: A string containing the contents of the invoked process's
///   standard output.
func execute(command: String, stdin: String) -> String {
  // ...
}
```

### Apple's Markup Format

Use of [Apple's markup format](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/) is strongly encouraged to add rich formatting to documentation. Such markup helps to differentiate symbolic references (like parameter names) from descriptive text in comments and is rendered by Xcode and other documentation generation tools. Some examples of frequently used directives are listed below.

* Paragraphs are separated using a single line that starts with `///` and is otherwise blank.
* _*Single asterisks*_ and _\_single underscores\__ surround text that should be rendered in italic/oblique type.
* **\*\*Double asterisks\*\*** and **\_\_double underscores\_\_** surround text that should be rendered in boldface.
* Names of symbols or inline code are surrounded in `` `backticks` ``.
* Multi-line code (such as example usage) is denoted by placing three backticks (` ``` `) on the lines before and after the code block.

### Where to Document

At a minimum, documentation comments are present for every open or public declaration, and every open or public member of such a declaration, with specific exceptions noted below:

* Individual cases of an `enum` often are not documented if their meaning is self-explanatory from their name. Cases with associated values, however, should document what those values mean if it is not obvious.
* A documentation comment is not always present on a declaration that overrides a supertype declaration or implements a protocol requirement, or on a declaration that provides the default implementation of a protocol requirement in an extension.
  
  It is acceptable to document an overridden declaration to describe new behavior from the declaration that it overrides. In no case should the documentation for the override be a mere copy of the base declaration's documentation.
  
* A documentation comment is not always present on test classes and test methods. However, they can be useful for functional test classes and for helper classes/methods shared by multiple tests.
* A documentation comment is not always present on an extension declaration (that is, the `extension` itself). You may choose to add one if it help clarifies the purpose of the extension, but avoid meaningless or misleading comments.
  
  In the following example, the comment is just repetition of what is already obvious from the source code:
  
  ❌ **Bad:**
  ```swift
  /// Add `Equatable` conformance.
  extension MyType: Equatable {
    // ...
  }
  ```
  
  The next example is more subtle, but it is an example of documentation that is not scalable because the extension or the conformance could be updated in the future. Consider that the type may be made `Comparable` at the time of that writing in order to sort the values, but that is not the only possible use of that conformance and client code could use it for other purposes in the future.
  
  ❌ **Bad:**
  ```swift
  /// Make `Candidate` comparable so that they can be sorted.
  extension Candidate: Comparable {
    // ...
  }
  ```

In general, if you find yourself writing documentation that simply repeats information that is obvious from the source and sugaring it with words like "a representation of," then leave the comment out entirely.

However, it is _not_ appropriate to cite this exception to justify omitting relevant information that a typical reader might need to know. For example, for a property named `canonicalName`, don't omit its documentation (with the rationale that it would only say `/// The canonical name.`) if a typical reader may have no idea what the term "canonical name" means in that context. Use the documentation as an opportunity to define the term.

---

Source: https://google.github.io/swift/
</rewritten_file>