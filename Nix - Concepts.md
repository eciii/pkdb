
**Shell environments**

- Syntax: `nix-shell -p PKG... [--run "CMD"]`
- The `-p` parameter stands for`--packages`.
- If `--run` is omitted then an interactive shell is started.
- Shell environments can be nested.
- Such shell environments are not _fully reproducible_. This can be improved with two additional parameters: `-I` and `--pure`.

---

**Syntax**

_Values, types and literals_

| Type    | Sample Values           | Notes                                                                                                                                                                               |
| ------- | ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Null    | `null`                  | • Type with only this value                                                                                                                                                         |
| Boolean | `true`/`false`          | • Type with just these two values                                                                                                                                                   |
| Integer | `-1`, `0`, `1`          | • 64-bit signed (two's complement) integers<br>• Non-negative integers can be expressed as integer literals. Negative integers are created with the _arithmetic negation operator_. |
| Float   | `3.141`, `.12e3`        | • 64-bit IEEE 754 floating-point number<br>• Most non-negative floats can be expressed as float literals. Negative floats are created with the _arithmetic negation operator_       |
| String  | `"single-line string"`, |                                                                                                                                                                                     |
|         |                         |                                                                                                                                                                                     |
