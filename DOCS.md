# Documentation

Interface reference for the `okf` package.
Format rules live in the [OKF specification][okf].

## `okf.validate(path)`

Validates every concept document in a bundle and returns the errors found.

- Parameters: `path` — bundle root directory (string, default `"bundle"`).
- Returns: `list` of error strings; empty when the bundle is conformant.
- Errors: one entry per missing or empty `type`, `title`, or `description`
  field, formatted as `Missing or empty <field> in <file>`.
- Prints: `🎉 OK! No errors found` to stdout when the list is empty.
- Notes: only `.md` files directly inside `path` are inspected, not
  subdirectories. Frontmatter must be a `---` delimited block at the start of
  the file, parsed as flat `key: value` lines without a YAML library.
  Deterministic: no clock, network, or filesystem-order dependence.

[okf]: https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md
