
**Shell environments**

- Syntax: `nix-shell -p PKG... [--run "CMD"]`
- The `-p` parameter stands for`--packages`.
- If `--run` is omitted then an interactive shell is started.
- Shell environments can be nested.
- Such shell environments are not _fully reproducible_. This can be improved with two additional parameters: `-I` and `--pure`.

---

**Syntax**

_Values, types and literals_

Types can be categorized like this:

- Primitive
	- Fixed-size
		- Symbolic: _Null_, _Boolean_
		- Numeric: _Integer_, _Float_
	- Variable-size: _String_
- Composite: _List_, _Attribute set_

About primitive fixed-size types:

| Type    | Sample Values    | Notes                                   |
| ------- | ---------------- | --------------------------------------- |
| Null    | `null`           | • Type with only this value             |
| Boolean | `true`/`false`   | • Type with just these two values       |
| Integer | `-1`, `0`, `1`   | • 64-bit signed integers                |
| Float   | `3.141`, `.12e3` | • 64-bit IEEE 754 floating-point number |
About strings

Two kinds of literals: `"single-line"` and

```
''
  Multi-line string with least common indentation removed.
  The opening and ending quotes may also be indented.
''
```

String Interpolation

