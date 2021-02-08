# CodingStyle

This guide relies heavily on [Ray Wenderlich's swift style-guide](https://github.com/raywenderlich/swift-style-guide) while adding some more rules that we find revelant and removing those which don't really make a difference to us.

## Code Organization

### MARKing

Use `// MARK: -` whenever separating code in the same file allows to have a nice separation as well as adding a reference in the quick jump bar.

### Protocol conformance

Prefer adding a separate extension for each protocol conformance instead of declaring each protocol conformance on the same line.

i.e:

#### Preferred:

```Swift
// MARK: - My view controller base
class MyViewController: UIViewController {
  // class stuff here
}

// MARK: - Table view datasource
extension MyViewController: UITableViewDataSource {
  // table view data source methods
}

// MARK: - Scroll view delegate
extension MyViewController: UIScrollViewDelegate {
  // scroll view delegate methods
}
```

#### Not preferred:

```Swift
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```

Re-declaration of protocol conformance is not always necessary, especially when the class referring to is a final or a sub class.
If that is the case, you can still separate the methods through extensions with `// MARK: -` to explain those extensions' utility.
This will allow to quickly nail down the blocks containing methods of the same category that one would be looking for, without having to go through
the entirety of the code to find them.

## Syntax

### Usage of self

Avoid using `self` when not necessary.
There are 2 places where `self` should be used:
  - To dissociate properties from parameters whenever allocating them, and only if they have the same name (i.e: `self.myVariable = myVariable`).
  - On `@escaping` closures where they are required (i.e: `myMethod() { self.myOtherMethod() }`).

This also allow to easily find completion blocks and initialization. Also lightens code readability:

#### Preferred:

```Swift
updateMyViewController(with: myViewModel)

myMethod() { [weak self] in
  self?.myOtherMethod()
}
```

#### Not preferred:

```Swift
self.updateMyViewController(with: self.myViewModel)

self.myMethod() { [weak self] in
  self?.myOtherMethod()
}
```

### Usage of return

If the method or computed property is made of a single expression, you can now avoid using the `return` statement as it is implicitly implied.

#### Preferred:

```Swift
var myVariable: Bool {
  true
}

func myMethod() -> String {
  "Works perfectly"
}
```

#### Not preferred:

```Swift
var myVariable: Bool {
  return true
}

func myMethod() -> String {
  return "Works perfectly"
}
```

### Computed properties

For computed properties, unless a `set` is defined, avoid using the `get` if the property is read-only.

#### Preferred:

```Swift
var myVariable: Bool {
  true
}
```

#### Not preferred:

```Swift
var myVariable: Bool {
  get {
    return true
  }
}
```

### Blocks, methods and declarations spacing

There should be one blank line at the beginning and ending of each class / extension block for more readability.
There should be one blank line between methods and up to one blank line between type declarations to aid in visual clarity and organization.
Whitespace within methods should separate functionality, but having too many sections in a method often means you should refactor into several methods.

```Swift
class MyClass {

  var myVariable: Bool = true

  func myMethod() {
    var insideVariable: String = ""
    
    AnotherClass.doSomethingWithString(insideVariable)
  }

}
```

### Statement spacing

Statements such as `if`/`else if`/`else`/`while`/`for` shoulf sit on the same line as their `{`,`}`.

#### Preferred:

```Swift
if condition {
  // Do that
} else if {
  // Do this
} else {
  // Do not
}
```

#### Not preferred:

```Swift
if condition
{
  // Do that
}
else
{
  // Do this
}
```

### Trailing whitespaces and whitespaces only lines

Avoid trailing whitespaces and whitespaces only, as those may create additional diffs on PRs, ultimately, it is also clearer to know when your lines ends.

There's an option in XCode that will automatically do that for you.

![XCode option to trim trailing whitespaces and whitespaces only lines](https://pbs.twimg.com/media/DTawMEMWsAAbbbs?format=png&name=medium)

### Comments

Don't hesitate to comment your code, if you want it to be parsed as documentation by XCode, use `///` or `/** [...] */`. If you don't want
your comment to be documented, prefer `//` or `/* [...] */`.

### Language

Prefer using US syntax over UK syntax, this will avoid confusion whenever
using native variables such as `UIColor.gray` vs `MyColour.grey`

i.e:

US - Preferred | UK - Not preferred
------------ | -------------
Color | Colour
Gray | Grey
