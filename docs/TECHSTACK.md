# TECHSTACK — Desktop FS MCP

## Desktop shell
- **Tauri** (cross-platform desktop shell)

## UI
- **Svelte** (recommended with TypeScript)

## Core runtime
- **Rust**

## Storage (preferred)
- **SQLite**
  - Stores folder roots, permissions, settings, and (optionally) audit logs
  - Use schema migrations (versioned)

## MCP Integration
- Local MCP server compatible with ChatGPT Desktop.
- Preferred transport: **stdio-launched process** (ChatGPT Desktop launches the MCP binary and communicates via stdio).
- Alternative transport: **local loopback socket** only if required by host constraints.

## Security (implementation-critical)
- Path normalization + canonicalization before policy checks
- Root containment checks (deny traversal)
- Prevent symlink/junction escape outside allowed roots
- Least-privilege defaults (read-only by default; write/delete off)
- Optional guardrails:
  - extension allow/deny lists (block `.env`, `.pem`, `.key`)
  - size limits for read/write
  - rate limiting

## Logging / Observability
- Audit log stored in SQLite (preferred) or JSONL
- UI log viewer with filters (root/tool/allow/deny)

## Testing
- Rust unit tests: path/policy edge cases across Windows/macOS/Linux
- Rust integration tests: MCP tool calls + CRUD enforcement
- Cross-platform smoke tests for packaged apps

## CI/CD
- GitHub Actions:
  - rustfmt, clippy, tests
  - build Tauri bundles for Windows/macOS/Linux
  - upload artifacts to releases

## Packaging
- Windows: MSI/EXE via Tauri bundler
- macOS: app bundle / dmg
- Linux: AppImage and/or deb/rpm
- Code signing (recommended, can be phased)