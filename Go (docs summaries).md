### Effective Go

**Introduction**

To write Go well, it's important to understand its properties and idioms. It's also important to know the established conventions for programming in Go, such as naming, formatting, program construction, and so on, so that programs you write will be easy for other Go programmers to understand.

This document gives tips for writing clear, idiomatic Go code.

The Go standard library is intended to serve not only as the core library but also as an example of how to use the language.

**Formatting**

Formatting issues are the most contentious but the least consequential. People can adapt to different formatting styles but it's better if they don't have to, and less time is devoted to the topic if everyone adheres to the same style.

If you want to know how to handle some new layout situation, run `gofmt`; if the answer doesn't seem right, rearrange your program, don't work around it.

Some selected formatting details handled automatically by `gofmt`:

- Indentation is done using tabs.
- Go has no line length limit. Don't worry about overflowing a punched card. If a line feels too long, wrap it and indent with an extra tab.

**Comments**

While Go also provides block-style `/* */` comments, the line-style `//` comments are the norm; block comments appear mostly as package comments.

Comments that appear before top-level declarations, with no intervening newlines, are considered to document the declaration itself.

**Names**

The convention in Go is to use `MixedCaps` or `mixedCaps` rather than underscores to write multi-word names.

_Package names_

It's helpful if everyone using the package can use the same name to refer to its contents, which implies that the package name should be good: short, concise, evocative. By convention, packages are given lower case, single-word names (e.g `bytes`); there should be no need for underscores or mixedCaps. Err on the side of brevity, since everyone using your package will be typing that name.

Another convention is that the package name is the base name of its source directory; the package in `src/encoding/base64` is imported as `"encoding/base64"` but has name `base64`, not `encoding_base64` and not `encodingBase64`.

The importer of a package will use the name to refer to its contents, so exported names in the package can use that fact to avoid repetition. For instance, the buffered reader type in the `bufio` package is called `Reader`, not `BufReader`, because users see it as `bufio.Reader`, which is a clear, concise name. Moreover, because imported entities are always addressed with their package name, `bufio.Reader` does not conflict with `io.Reader`. Another example is `once.Do`; `once.Do(setup)` reads well and would not be improved by writing `once.DoOrWaitUntilDone(setup)`. Long names don't automatically make things more readable.

_Interface names_

By convention, one-method interfaces are named by the method name plus an -er suffix or similar modification to construct an agent noun: `Reader`, `Writer`, `Formatter`, `CloseNotifier` etc.

There are a number of such names and it's productive to honor them and the function names they capture. `Read`, `Write`, `Close`, `Flush`, `String` and so on have canonical signatures and meanings. To avoid confusion, don't give your method one of those names unless it has the same signature and meaning. Conversely, if your type implements a method with the same meaning as a method on a well-known type, give it the same name and signature; call your string-converter method `String` not `ToString`.

**Semicolons**

Go's formal grammar uses semicolons to terminate statements, but those semicolons do not appear in the source. Instead the lexer uses a simple rule to insert semicolons automatically as it scans, so the input text is mostly free of them. The rule can be summarized as, “if the newline comes after a token that could end a statement, insert a semicolon”. A semicolon can also be omitted immediately before a closing brace.

Idiomatic Go programs have semicolons only in places such as `for` loop clauses, to separate the initializer, condition, and continuation elements. They are also necessary to separate multiple statements on a line, should you write code that way.

