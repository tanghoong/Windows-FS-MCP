# Versioning

Desktop FS MCP follows [Semantic Versioning 2.0.0](https://semver.org/).

## Version Format

```
MAJOR.MINOR.PATCH[-PRERELEASE][+BUILD]
```

- **MAJOR**: Incompatible API changes or breaking changes to MCP tools
- **MINOR**: New features added in a backward-compatible manner
- **PATCH**: Backward-compatible bug fixes

### Pre-release Identifiers

- `alpha.N` - Early development, unstable
- `beta.N` - Feature complete, testing phase
- `rc.N` - Release candidate

### Examples

- `0.1.0` - Initial development release
- `1.0.0` - First stable release
- `1.1.0-beta.1` - Beta for version 1.1.0
- `2.0.0-rc.1` - Release candidate for version 2.0.0

## Release Naming

Releases are named using the version number prefixed with `v`:

- `v0.1.0` - Initial development release
- `v1.0.0` - First stable release (MVP)
- `v1.1.0` - First feature update

## Changelog

All notable changes are documented in the [CHANGELOG.md](../CHANGELOG.md) file following the [Keep a Changelog](https://keepachangelog.com/) format.

## Release Process

1. Update version in `Cargo.toml` and `package.json`
2. Update `CHANGELOG.md` with release notes
3. Create a git tag: `git tag v1.0.0`
4. Push tag: `git push origin v1.0.0`
5. CI will automatically build and create release artifacts

## Version Policy

### Pre-1.0.0 (Development Phase)

- API may change without notice
- Minor version bumps may include breaking changes
- Patch version for bug fixes only

### Post-1.0.0 (Stable)

- Strict semantic versioning
- Breaking changes only in major versions
- Deprecation warnings before removal
