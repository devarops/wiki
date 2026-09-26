# Backlog

`specs/` is the specification: every rule there is machine-provable and each
criterion names a test selection, so `make verify` enforces it. Those rules are
not repeated here.

Everything below is backlog — work that is unimplemented, or that is not
deterministically machine-provable. Status: ✅ done, ❌ not started.

## Validator phases

| Phase | Spec | Status |
|-------|------|--------|
| Entry point, frontmatter, required fields | — | ✅ |
| File discovery | — | ❌ |
| Filename convention | — | ❌ |
| Reserved files | `reserved-files` | ❌ |
| Concept field limits | `concept-fields` | ❌ |
| Cross-link resolution | — | ❌ |
| Prose style | — | ❌ |
| Error output | `error-output` | ❌ |

The last five are deterministically testable and are candidates for new
`specs/` files, so that `make verify` covers them.

## File discovery

Only these are part of a bundle: root `index.md`, root `log.md`, and every
`.md` file under `bundle/`. Skip `raw/`. Silently ignore every other root file
and all non-`.md` files.

## Filename convention

Every concept filename matches `<digits>[<letter>]...md`, one or more segments.
Examples: `2.md`, `3a.md`, `1a.2b.md`. Reserved and project files are exempt.

`AGENTS.md` used to describe one trailing letter per segment. That is narrower
than what `next_child_filename` produces, and the description was dropped rather
than corrected — restore it once the convention settles.

## Cross-link resolution

Check every markdown link in a concept body, inline and reference-style.

- Strip any `#fragment` before resolving.
- Skip `http://` and `https://` targets.
- Resolve `/`-prefixed paths against the bundle root, everything else against
  the containing file's directory.
- A target that is missing, or that resolves to a directory, is an error.

Links in `index.md` and `log.md` are not checked.

## Prose style

Applies to concept bodies, excluding frontmatter, headings, fenced and indented
code blocks, and table rows.

- One sentence per line, ending in `.`, `?`, or `!`.
- Each sentence ≤ 25 words.
- Each document ≤ 200 words in total.

Reserved and project files are exempt.

## Edge cases

| Situation | Handling |
|-----------|----------|
| Frontmatter only, no body | Passes. |
| Body only, no frontmatter | Error. |
| Symlink to a `.md` file under `bundle/` | Followed and validated. |
| `.md` file in `raw/` | Skipped. |
| Non-`.md` file | Ignored. |
| Link to a missing non-`.md` target | Error. |
| Root project file | Skipped, not a concept. |
| Root `index.md` | Absent. Create it. |

## Design: `next_child_filename`

New module `okf/naming.py`. Not machine-provable as stated, so it stays here.

```python
def next_child_filename(parent: str, bundle_path: str = "bundle") -> str
```

Returns a bare filename such as `1a.md`, with no directory prefix.

1. Parse the last segment: `parent.split('.')[-1]`.
2. All digits, so `1` gives candidate `parent + "a"`. Digits plus a letter, so
   `1a` gives candidate `parent + ".1"`.
3. On collision, scan `bundle_path` for existing `*.md` names once, then
   increment the appended suffix until a gap is found. Both sequences are
   infinite, so a gap always exists.
4. `parent` is assumed conformant. Validation is the caller's job.
