# Changelog

Changes to the public interface of the `okf` module, following
[Keep a Changelog](https://keepachangelog.com/) and
[Semantic Versioning](https://semver.org/) with 0.y.z — increment the minor
version for every release.

Nothing else belongs here. Make targets, specs, and repo conventions are
internal. Concept content changes are recorded in `log.md`.

## [Unreleased]

### Added

- (none)

## [0.1.0] - 2026-06-22

### Added

- `okf` Python package with `okf.validate()` function for bundle validation
- Required field checks for `type`, `title`, and `description` in concept
  document frontmatter (presence and non-empty value)
- Success message printed to stdout when the bundle is clean

[Unreleased]: https://github.com/devarops/okf/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/devarops/okf/releases/tag/v0.1.0
