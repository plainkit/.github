# BloxUI

Type-safe HTML component library for Go with compile-time validation and zero runtime overhead.

## What is BloxUI?

BloxUI generates HTML using pure Go functions instead of template engines. Each HTML element is represented by a function that accepts typed arguments, providing compile-time safety and IDE autocomplete support.

```go
// This compiles and works
input := Input(
    InputType("email"),
    InputName("email"),
    Required(),
    Placeholder("Enter email"),
)

// This fails at compile time - Href is not valid for Input
input := Input(Href("/invalid")) // Compile error
```

## Core Libraries

**[blox](https://github.com/bloxui/blox)** - HTML component generation

- Type-safe HTML elements with element-specific attributes
- Compile-time validation prevents invalid HTML structures
- Zero runtime overhead through method dispatch resolution
- Modular architecture with one file per element type

**[ui](https://github.com/bloxui/ui)** - Pre-built components

- Modern UI components with consistent styling
- Accessibility features built-in
- Compose with core blox elements seamlessly

**[icons](https://github.com/bloxui/icons)** - Icon libraries

- Type-safe Lucide icons as Go functions
- Customizable size, stroke, and color properties
- Tree-shakeable - only icons you use are included

## How It Works

### Function Composition

```go
card := Div(
    Class("card"),
    H1(Text("Title"), Class("card-header")),
    P(Text("Content"), Class("card-body")),
)
```

### Type Safety

Each element only accepts valid attributes:

- `Input()` accepts `InputType()`, `InputName()`, `Required()`
- `A()` accepts `Href()`, `Target()`, `Rel()`
- `Button()` accepts `ButtonType()`, `Disabled()`

### Zero Runtime Cost

HTML generation uses compile-time method dispatch. No reflection, no template parsing:

```go
component := Div(Class("test"), Text("Hello"))
html := Render(component) // Direct string building
```

## Architecture

```
blox/           # Core HTML elements (div, input, form, etc.)
ui/             # Higher-level components (Card, Button, Modal)
icons/          # Icon libraries (Lucide)
examples/       # Complete applications
```

## Quick Start

```go
package main

import (
    "fmt"
    . "github.com/bloxui/blox"
)

func main() {
    page := Html(
        Lang("en"),
        Head(HeadTitle(Text("My Page"))),
        Body(
            H1(Text("Hello, World!")),
            P(Text("Built with Blox")),
        ),
    )

    fmt.Println("<!DOCTYPE html>")
    fmt.Println(Render(page))
}
```

## Benefits

- **IDE Support**: Full autocomplete, go-to-definition, and refactoring
- **Compile Safety**: Invalid HTML structures fail at build time
- **Performance**: Zero runtime parsing or reflection overhead
- **Testing**: Components are regular Go values - test like any Go code
- **Modularity**: Each HTML element in its own file for easy contribution

## License

MIT

---

Built with ❤️ for developers who value simplicity and speed.
