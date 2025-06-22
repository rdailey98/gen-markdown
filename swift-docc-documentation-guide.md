# Swift DocC Documentation Guide

This guide combines documentation from multiple Swift.org DocC resources into a single comprehensive reference.

## Table of Contents

1. [Documenting a Swift Framework or Package](#documenting-a-swift-framework-or-package)
2. [Writing Symbol Documentation in Your Source Files](#writing-symbol-documentation-in-your-source-files)
3. [Adding Supplemental Content to a Documentation Catalog](#adding-supplemental-content-to-a-documentation-catalog)
4. [Linking to Symbols and Other Content](#linking-to-symbols-and-other-content)
5. [Formatting Your Documentation Content](#formatting-your-documentation-content)
6. [Adding Tables of Data](#adding-tables-of-data)
7. [Other Formatting Options](#other-formatting-options)
8. [Adding Images](#adding-images)
9. [Adding Structure to Your Documentation Pages](#adding-structure-to-your-documentation-pages)
10. [Customizing the Appearance of Your Documentation Pages](#customizing-the-appearance-of-your-documentation-pages)

---

## Documenting a Swift Framework or Package

DocC (Documentation Compiler) is Apple's tool for building documentation for Swift frameworks and packages. It transforms Markdown-based documentation comments into rich, interactive documentation.

### Getting Started

1. **Build Documentation in Xcode**: 
   - Select **Product > Build Documentation** (or press `Shift-Control-Command-D`)
   - The Developer Documentation window opens with your documentation

2. **Enable Automatic Documentation Building**:
   - In Build Settings, set "Build Documentation During 'Build'" to Yes
   - Or use `xcodebuild docbuild` in CI/CD pipelines

### How DocC Works

When Xcode builds your framework, it:
1. Saves details about public APIs and generated code
2. Supplies DocC with all public API information
3. DocC gathers API information and any additional documentation resources
4. Produces a final archive containing compiled documentation

---

## Writing Symbol Documentation in Your Source Files

Symbol documentation uses special comment formats that DocC recognizes and processes.

### Documentation Comment Formats

**Triple-slash comments** (for single lines):
```swift
/// This is a summary of the function
/// - Parameter name: The name to greet
/// - Returns: A greeting string
public func greet(name: String) -> String {
    return "Hello, \(name)!"
}
```

**Block comments** (for multiple lines):
```swift
/**
 This is a summary of the type.
 
 A more detailed discussion can go here,
 explaining how to use this type.
 
 - Note: This is an important note
 - Since: iOS 14.0
 */
public struct MyType {
    // Implementation
}
```

### Documentation Structure

1. **Summary**: The first line/paragraph becomes the summary
2. **Discussion**: Additional paragraphs after a blank line
3. **Parameters**: Document each parameter
4. **Returns**: Describe return values
5. **Throws**: List possible errors

### Adding Documentation in Xcode

1. Place cursor on a symbol
2. Press `Command-Option-/` or right-click and select "Add Documentation"
3. Xcode generates a documentation template

### Common Documentation Keywords

- `Parameter`: Describes a function parameter
- `Parameters`: Groups multiple parameters
- `Returns`: Describes the return value
- `Throws`: Lists errors that can be thrown
- `Note`: Adds a note callout
- `Important`: Highlights important information
- `Warning`: Adds a warning callout
- `Tip`: Provides helpful tips
- `Precondition`: Documents preconditions
- `Postcondition`: Documents postconditions
- `Requires`: Lists requirements
- `Since`: Indicates availability
- `Author`: Credits the author
- `Copyright`: Adds copyright information
- `Version`: Specifies version information

---

## Adding Supplemental Content to a Documentation Catalog

Documentation catalogs allow you to add articles, tutorials, and resources beyond inline documentation.

### Creating a Documentation Catalog

1. Right-click on your package/target in Xcode
2. Choose **New File > Documentation > Documentation Catalog**
3. A `.docc` folder is created with:
   - A main documentation file
   - A Resources folder for images and assets

### Documentation Catalog Structure

```
MyPackage.docc/
├── MyPackage.md          # Main landing page
├── Articles/             # Additional articles
│   ├── GettingStarted.md
│   └── AdvancedUsage.md
├── Tutorials/            # Step-by-step tutorials
│   └── Tutorial.tutorial
└── Resources/            # Images and media
    ├── image.png
    └── video.mp4
```

### Main Documentation Page

The main page uses your package name as the title:

```markdown
# ``MyPackage``

A brief description of your package.

## Overview

Detailed overview of what your package does and its main features.

## Topics

### Getting Started

- ``MyPackage/MyClass``
- <doc:GettingStarted>

### Advanced Features

- ``MyPackage/AdvancedClass``
- <doc:AdvancedUsage>
```

### Adding Articles

Create markdown files in your documentation catalog:

```markdown
# Getting Started with MyPackage

Learn the basics of using MyPackage.

## Overview

This article covers the fundamental concepts...

## Basic Usage

Here's how to get started:

```swift
import MyPackage

let instance = MyClass()
instance.doSomething()
```
```

### Extension Files

Extension files provide additional documentation for existing symbols:

```markdown
# ``MyPackage/MyClass``

@Metadata {
    @DocumentationExtension(mergeBehavior: append)
}

## Extended Discussion

Additional details about MyClass that supplement the inline documentation...

## Advanced Topics

### Performance Considerations

When using MyClass in performance-critical code...
```

---

## Linking to Symbols and Other Content

DocC provides several ways to create links between documentation pages and symbols.

### Symbol Links

Use double backticks to link to symbols:

```markdown
- ``MyClass`` - Links to a type
- ``MyClass/myMethod()`` - Links to a method
- ``MyClass/myProperty`` - Links to a property
- ``init(name:age:)`` - Links to an initializer
```

### Documentation Links

Link to other documentation pages:

```markdown
- <doc:GettingStarted> - Links to an article
- <doc:Tutorial-Table-of-Contents> - Links to tutorials
- <doc:MyClass> - Alternative way to link to type documentation
```

### External Links

Standard markdown links for external resources:

```markdown
- [Apple Developer](https://developer.apple.com)
- [Swift Forums](https://forums.swift.org)
```

### Linking Best Practices

1. Use symbol links for API references
2. Use doc links for articles and guides
3. Provide context for external links
4. Verify all links build correctly

---

## Formatting Your Documentation Content

DocC supports standard Markdown formatting with some extensions.

### Text Formatting

**Basic formatting:**
- `*italic*` or `_italic_` → *italic*
- `**bold**` or `__bold__` → **bold**
- `***bold italic***` → ***bold italic***
- `` `code` `` → `code`

**Headings:**
```markdown
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
```

### Lists

**Bulleted lists:**
```markdown
- First item
- Second item
  - Nested item
  - Another nested item
- Third item
```

**Numbered lists:**
```markdown
1. First step
2. Second step
   1. Sub-step
   2. Another sub-step
3. Third step
```

### Code Blocks

**Inline code:**
```markdown
Use `let` for constants and `var` for variables.
```

**Code blocks with syntax highlighting:**
````markdown
```swift
func greet(name: String) -> String {
    return "Hello, \(name)!"
}
```
````

### Callouts

DocC supports various callout types:

```markdown
> Note: General information

> Important: Important information

> Warning: Critical warnings

> Tip: Helpful tips

> Experiment: Try this out
```

### Custom Callouts

Create custom callouts:

```markdown
> Custom: Your custom callout content
```

---

## Adding Tables of Data

Tables help organize information in a structured format.

### Basic Table Syntax

```markdown
| Column 1 | Column 2 | Column 3 |
| -------- | -------- | -------- |
| Cell 1   | Cell 2   | Cell 3   |
| Cell 4   | Cell 5   | Cell 6   |
```

### Column Alignment

```markdown
| Left | Center | Right |
| :--- | :----: | ----: |
| Text | Text   | Text  |
```

### Tables in Documentation

**Parameter tables:**
```markdown
| Parameter | Type | Description |
| --------- | ---- | ----------- |
| name | String | The user's name |
| age | Int | The user's age |
| isActive | Bool | Whether the user is active |
```

**Comparison tables:**
```markdown
| Feature | Option A | Option B |
| ------- | -------- | -------- |
| Speed | Fast | Moderate |
| Memory | Low | High |
| Complexity | Simple | Complex |
```

---

## Other Formatting Options

### Horizontal Rules

Create horizontal rules with three or more hyphens:

```markdown
---
```

### Block Quotes

```markdown
> This is a block quote.
> It can span multiple lines.
```

### Metadata

Add metadata to documentation pages:

```markdown
@Metadata {
    @PageColor(purple)
    @PageImage(purpose: icon, source: "icon")
    @Available(iOS 15, macOS 12, *)
}
```

### Page Structure Directives

**Tab Navigator:**
```markdown
@TabNavigator {
    @Tab("Overview") {
        Content for overview tab
    }
    @Tab("Implementation") {
        Content for implementation tab
    }
}
```

**Row and Column Layout:**
```markdown
@Row {
    @Column {
        Left column content
    }
    @Column {
        Right column content
    }
}
```

---

## Adding Images

Images enhance documentation with visual examples and diagrams.

### Image Syntax

```markdown
![Alt text](image-name.png)
```

### Image Variants

DocC supports different image variants:

1. **Standard images**: `image.png`
2. **2x resolution**: `image@2x.png`
3. **Dark mode**: `image~dark.png`
4. **Dark mode 2x**: `image~dark@2x.png`

### Image Storage

Store images in the Resources folder of your documentation catalog:

```
MyPackage.docc/
└── Resources/
    ├── overview.png
    ├── overview@2x.png
    ├── overview~dark.png
    └── overview~dark@2x.png
```

### Image Best Practices

1. Use descriptive alt text for accessibility
2. Provide both light and dark variants
3. Optimize image file sizes
4. Use appropriate image formats (PNG for diagrams, JPEG for photos)

---

## Adding Structure to Your Documentation Pages

Structure helps readers navigate and understand your documentation.

### Topics Section

Organize content into topics:

```markdown
## Topics

### Getting Started
- ``BasicClass``
- ``BasicProtocol``
- <doc:QuickStart>

### Advanced Features
- ``AdvancedClass``
- ``AdvancedProtocol``
- <doc:AdvancedGuide>
```

### Hierarchical Organization

Create nested topic groups:

```markdown
## Topics

### Core Concepts
- ``CoreClass``
- ``CoreProtocol``

#### Data Models
- ``User``
- ``Product``
- ``Order``

#### Networking
- ``APIClient``
- ``Request``
- ``Response``
```

### See Also Sections

Add related links:

```markdown
## See Also

- ``RelatedClass``
- <doc:RelatedArticle>
- [External Resource](https://example.com)
```

---

## Customizing the Appearance of Your Documentation Pages

DocC allows customization through theme settings and metadata.

### Theme Settings

Create a `theme-settings.json` file in your documentation catalog:

```json
{
    "theme": {
        "color": {
            "documentation-intro-fill": "linear-gradient(to bottom, #1d1d1f, #8e8e93)",
            "documentation-intro-accent": "rgb(255, 149, 0)"
        },
        "aside": {
            "note": {
                "background-color": "#e8f4f8",
                "border-color": "#0070c9"
            }
        }
    }
}
```

### Page Colors

Set page colors using metadata:

```markdown
@Metadata {
    @PageColor(blue)
}
```

Available colors:
- `blue`
- `gray`
- `green`
- `orange`
- `purple`
- `red`
- `yellow`

### Page Images

Add hero images to pages:

```markdown
@Metadata {
    @PageImage(purpose: icon, source: "page-icon")
    @PageImage(purpose: card, source: "page-card")
}
```

### Custom Styling

1. **Accent colors**: Define primary colors for your documentation
2. **Gradient fills**: Create custom gradients for page headers
3. **Aside styling**: Customize callout appearances
4. **Font settings**: Adjust typography (limited support)

### Display Options

Control documentation display:

```markdown
@Options {
    @AutomaticSeeAlso(disabled)
    @AutomaticTitleHeading(disabled)
    @TopicsVisualStyle(compactGrid)
}
```

---

## Best Practices

1. **Write clear summaries**: The first line should concisely describe the symbol
2. **Use consistent formatting**: Follow a style guide for your documentation
3. **Provide examples**: Include code examples for complex APIs
4. **Link related content**: Help readers discover related APIs and articles
5. **Keep documentation updated**: Update docs when APIs change
6. **Test your documentation**: Build and review regularly
7. **Consider your audience**: Write for your documentation's intended readers

## Additional Resources

- [Swift.org DocC Documentation](https://www.swift.org/documentation/docc/)
- [Apple Developer - DocC](https://developer.apple.com/documentation/docc)
- [WWDC Videos on DocC](https://developer.apple.com/videos/all-videos/?q=docc)
- [Swift Forums - Documentation](https://forums.swift.org/c/development/documentation)

---

*This guide compiled from official Swift.org DocC documentation pages. For the most up-to-date information, please refer to the original sources.* 