# CodingStyle

## Code Organization

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

### Language

Prefer using US syntax over UK syntax, this will avoid confusion whenever
using native variables such as `UIColor.gray` vs `MyColour.grey`

i.e:

US - Preferred | UK - Not preferred
------------ | -------------
Color | Colour
Gray | Grey
