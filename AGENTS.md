# LLM Wiki in the Open Knowledge Format

Conventions for agents and developers working in this repo.
Format rules are deliberately not restated here — see the OKF spec below.

## Commit style

Prefix every commit with a gitmoji followed by an imperative verb.
First line under 72 characters. Blank line then body.
TDD phases: `🛑 🧪` (Red), `✅ 🧪` (Green), `♻️` (Refactor).
Config/tooling: `🔧`, lint fix: `🚨`, goal update: `🎯`.

## Repo layout

| Path | Contents |
|------|----------|
| `bundle/` | Concept documents. The only directory the validator checks. |
| `raw/` | Source documents. A separate git repo, skipped by all checks. |
| `log.md` | Change log for concept content. |
| `okf/` | Validation package. Entry point `okf.validate()`. |
| `specs/` | Machine-provable specification, checked by `make verify`. |
| `tests/data/` | One non-conformant fixture per violation. |

Root-level `.md` files (README, AGENTS, DOCS, CHANGELOG, TODO) are project
infrastructure, not concepts.

This is an OKF conformant personal LLM Wiki. The format specification is:

<https://raw.githubusercontent.com/GoogleCloudPlatform/knowledge-catalog/refs/heads/main/okf/SPEC.md>

Read the spec for format rules. Do not duplicate them into this file.

## Documentation ownership

| File | Holds |
|------|-------|
| `README.md` | Project overview for readers. |
| `DOCS.md` | API and CLI reference. |
| `TODO.md` | Backlog, plus design notes for unwritten work. |
| `specs/` | Rules that are machine-provable and verified. |
| `log.md` | What changed in the concept content. |

Do not restate a rule in two files. Link instead.

## Developer workflow

Run everything inside the `wiki_ci` container; `python` and `qed` are not on
the host.

- `make setup` — clean caches, install the package in editable mode.
- `make tests` — run the test suite.
- `make validate` — check `bundle/` against the implemented rules.
- `make verify` — check `specs/` criteria against the test suite.
- `make check` — lint (black, flake8, mypy) across `okf/` and `tests/`.
- `make format` — auto-format with black.
- `make clean` — remove `tests/__pycache__` (root-owned from Docker runs).
- `docker exec wiki_ci make <target>` — re-run a target in the running container.

`validate` and `verify` answer different questions. `validate` asks whether the
content is conformant; `verify` asks whether the validator implements the
specification. A green `validate` does not mean the spec is satisfied — see
`TODO.md` for what is still unimplemented.

## Tests

Each file in `tests/data/` breaks exactly one rule, so a test names the rule it
covers. Most tests validate the fixture directory as a whole.
