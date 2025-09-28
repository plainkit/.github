# Plain

Type‑safe HTML for Go. Server‑first, hypermedia by default, htmx as an optional extension. Compile‑time validation, zero hydration, no client runtime.

## What is Plain?

Plain generates HTML using pure Go functions instead of template engines. Each element is a typed function with compile‑time validation and IDE autocomplete. You build pages from links and forms, keep state on the server, and add interactivity progressively (e.g., with htmx) — no SSR/hydration/SPA machinery.

```go
// This compiles and works
input := Input(
    AType("email"),
    AName("email"),
    ARequired(),
    APlaceholder("Enter email"),
)

// This fails at compile time - Href is not valid for Input
input := Input(AHref("/invalid")) // Compile error
```

## Core Libraries

**`html`** — https://github.com/plainkit/html

- Type-safe HTML elements with element-specific attributes
- Compile-time validation prevents invalid HTML structures
- Zero runtime overhead through method dispatch resolution
- Modular architecture with one file per element type

**`icons`** — https://github.com/plainkit/icons

- Type-safe Lucide icons as Go functions
- Customizable size, stroke, and color properties
- Tree-shakeable - only icons you use are included

**`htmx`** — https://github.com/plainkit/htmx

- Server-driven interactions via HTML attributes
- No client bundler, no hydration, no virtual DOM
- Works as a small, optional enhancement layer

## Philosophy

- **Keep the web’s contract**: links and forms as the API, HTML as the media type, the server as the source of truth.
- **Cut the ceremony**: no bundlers, no client routers, no hydration steps to re‑build on the client what the server already rendered.
- **Complexity where it pays**: use htmx or small JS where UX truly benefits; avoid shipping a framework to simulate navigation.
- **Respect constraints**: favor resilience, first‑load speed, and cacheability over client‑side state machines.
- **Prefer compile‑time guarantees**: catch invalid HTML/attributes during build; avoid runtime checks, reflection, and hydration errors.

A bit blunt, because it matters: modern FE stacks often make simple things hard. SSR + hydration tries to undo SPA costs, yet you still ship the SPA. Plain targets server‑shaped apps — documents, dashboards, CRUD, back‑offices — where HTML + server actions are simpler to build, easier to reason about, and cheaper to run. When you truly need heavy client‑side state, offline mode, or complex realtime UX, use client frameworks where they shine.

## How It Works

### Function Composition

```go
card := Div(
    AClass("card"),
    H1(T("Title"), AClass("card-header")),
    P(T("Content"), AClass("card-body")),
)
```

### Zero Runtime Cost

HTML generation uses compile-time method dispatch. No reflection, no template parsing:

```go
component := Div(AClass("test"), T("Hello"))
html := Render(component) // Direct string building
```

## Architecture

```
html/           # Core HTML elements (div, input, form, etc.)
ui/             # Higher-level components (Card, Button, Modal)
icons/          # Icon libraries (Lucide)
htmx/           # htmx helpers and integration
examples/       # Complete applications
```

## Quick Start

```go
package main

import (
    "fmt"
    . "github.com/plainkit/html"
)

func main() {
    page := Html(
        ALang("en"),
        Head(Title(T("My Page"))),
        Body(
            H1(T("Hello, World!")),
            P(T("Built with Plain")),
        ),
    )

    fmt.Println("<!DOCTYPE html>")
    fmt.Println(Render(page))
}
```

## Benefits

- **IDE support**: Full autocomplete, go-to-definition, and refactoring
- **Compile safety**: Invalid HTML structures fail at build time
- **Performance**: Zero runtime parsing or reflection overhead
- **Testing**: Components are regular Go values — test like any Go code
- **Modularity**: Each HTML element in its own file for easy contribution
- **No hydration**: First paint is the final paint; progressive enhancement only

## License

MIT

---

Built with ❤️ for developers who value simplicity, speed, and the web’s original strengths.
