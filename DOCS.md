# Documentation

Technical reference for the `okf` package and the `make` targets.
Format rules live in the [OKF specification][okf]; what is enforced *today* is
listed below.

## `okf.validate(path)`

- Parameters: `path` — bundle root directory (string, default `"bundle"`).
- Returns: `list` of error strings. Empty list when the bundle is conformant.
- Prints: `🎉 OK! No errors found` to stdout when the list is empty.
- Deterministic: no clock, network, or filesystem-order dependence.

Only `.md` files directly inside `path` are inspected, not subdirectories.
Frontmatter must be a `---` delimited block at the start of the file. It is
parsed as flat `key: value` lines, without a YAML library.

## Rules enforced today

| Rule | Error message |
|------|---------------|
| `type` missing or empty | `Missing or empty type in {file}` |
| `title` missing or empty | `Missing or empty title in {file}` |
| `description` missing or empty | `Missing or empty description in {file}` |

Rules that are specified but not yet implemented are listed in `TODO.md`.

## Make targets

| Target | Runs | Purpose |
|--------|------|---------|
| `validate` | `okf.validate()` | Check `bundle/` content against implemented rules. |
| `verify` | `qed verify specs` | Check `specs/` criteria against the test suite. |
| `tests` | `pytest --verbose tests` | Run the test suite. |
| `check` | `black`, `flake8`, `mypy` | Lint `okf/` and `tests/`. |
| `format` | `black` | Auto-format `okf/` and `tests/`. |
| `setup` | `clean` then `install` | Clean caches, editable install. |
| `clean` | `rm -rf tests/__pycache__` | Remove caches left root-owned by Docker. |

`validate` and `verify` answer different questions. `validate` asks whether the
content is conformant; `verify` asks whether the validator implements the
specification, and fails when a criterion's test selection matches nothing.

Both targets need the `wiki_ci` container. `python` and `qed` are not installed
on the host.

[okf]: https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md
