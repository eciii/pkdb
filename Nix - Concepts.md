
**Shell environments**

- Syntax: `nix-shell -p PKG... [--run "CMD"]`
- The `-p` parameter stands for`--packages`.
- If `--run` is omitted then an interactive shell is started.
- Shell environments can be nested.
- Such shell environments are not _fully reproducible_. This can be improved with two additional parameters: `-I` and `--pure`.

---

## Syntax

**White space**

White space is used to delimit lexical tokens, where required. It is otherwise insignificant. Line breaks, indentation, and additional spaces are for the reader’s convenience.

**Values, types and literals**

_Types (categorized)_

- Primitive
	- Fixed-size
		- Symbolic: _Null_, _Boolean_
		- Numeric: _Integer_, _Float_
	- Variable-size: _String_
- Composite: _List_, _Attribute set_

_About primitive fixed-size types_

| Type    | Sample Values    | Notes                                   |
| ------- | ---------------- | --------------------------------------- |
| Null    | `null`           | • Type with only this value             |
| Boolean | `true`/`false`   | • Type with just these two values       |
| Integer | `-1`, `0`, `1`   | • 64-bit signed integers                |
| Float   | `3.141`, `.12e3` | • 64-bit IEEE 754 floating-point number |

_About strings_

Three kinds of literals:

- Standard strings:
	- start with a double-quote character (`"`)

Characters with special meanings are `\`, `$` and `"`:

- `\`:
	- if followed by a special character (`\`, `$` or `"`), escapes it
	- if followed by `n`, `r` or `t`, resolves to new line, carriage return or tab respectively
	- otherwise it is ignored
- `$`
	- if followed by `{` starts string interpolation (which is terminated using `}`)
	- otherwise it is not special at all
- `"`:
	- terminates the string

Any other character within the string is perfectly legal. In particular newlines are allowed.


- Indented strings:
	- start with two single-quote characters
	- strips from each line a number of spaces equal to the minimal indentation of the string as a whole (disregarding the indentation of empty lines)

Characters with special meanings are `'` or `$`:

- `'`:
	- if followed by another `'` terminates the string, unless
		- it is followed by a special character (`'` or `$`), in which case escapes it
		- or it is followed by a `\`, in which case
			- if followed by `n`, `r` or `t`, resolves to new line, carriage return or tab respectively
			- otherwise the whole sequence `''\` is ignored
	- otherwise it is not special at all
- `$`:
	- if followed by `{` starts string interpolation (which is terminated using `}`)
	- otherwise it is not special at all


- URIs:
	- as defined in appendix B of [RFC 2396](http://www.ietf.org/rfc/rfc2396.txt) can be written _as is_, without quotes
	- This is just a convenience (which to me seems quite unjustified, since, most probably, URIs do not come up that often)


_String Interpolation_

