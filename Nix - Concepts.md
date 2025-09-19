
**Shell environments**

- Syntax: `nix-shell -p PKG... [--run "CMD"]`
- The `-p` parameter stands for`--packages`.
- If `--run` is omitted then an interactive shell is started.
- Shell environments can be nested.
- Such shell environments are not _fully reproducible_. This can be improved with two additional parameters: `-I` and `--pure`.