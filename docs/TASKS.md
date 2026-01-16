# TASKS — Desktop FS MCP

> This task list is organized by milestones. Each item can become a GitHub Issue.

## Milestone 0 — Repo & Foundations
- [x] Add LICENSE
- [x] Add SECURITY.md (vuln reporting)
- [x] Add CONTRIBUTING.md (optional)
- [x] Define SemVer + release naming
- [x] Add basic CI (format/lint/test)

## Milestone 1 — Data Model + SQLite
- [ ] Define SQLite schema:
  - `settings` (global enable flag, transport settings, etc.)
  - `roots` (folder roots + permissions + active)
  - `audit_log` (optional for MVP but recommended)
- [ ] Implement migrations (versioned schema)
- [ ] CRUD APIs in Rust for roots/settings
- [ ] Import/export configuration JSON (optional)

## Milestone 2 — Policy Engine (Cross-platform)
- [ ] Path normalization + canonicalization (Windows/macOS/Linux)
- [ ] Root containment checks (deny traversal)
- [ ] Symlink/junction policy:
  - deny escape at minimum
  - decide allow-within-root vs deny-all
- [ ] Overlapping roots policy:
  - most-specific root wins
  - tie => most restrictive
- [ ] Unit tests for all edge cases

## Milestone 3 — MCP Server (Rust)
- [ ] Implement MCP server entry point (stdio-launched preferred)
- [ ] Implement tools (MVP):
  - [ ] `list_roots` (recommended)
  - [ ] `list_dir`
  - [ ] `read_file`
  - [ ] `stat`
  - [ ] `write_file` (create vs overwrite)
  - [ ] `create_dir`
  - [ ] `delete_path`
- [ ] Enforce per-root active + CRUD on every call
- [ ] Good error messages for denials

## Milestone 4 — Audit Logging
- [ ] Define audit record fields:
  - timestamp, tool, requested path(s), matched root, decision, reason, bytes, duration
- [ ] Persist logs (SQLite table recommended)
- [ ] Add export logs feature (CSV/JSONL)

## Milestone 5 — Tauri UI (Svelte)
- [ ] Svelte UI scaffolding
- [ ] Folder roots screen:
  - list roots (name/path)
  - active toggle
  - CRUD toggles
  - add/edit/remove
- [ ] Status screen (server running/stopped, diagnostics)
- [ ] Logs screen (filters + export)

## Milestone 6 — ChatGPT Desktop Integration
- [ ] Document setup for Windows/macOS/Linux
- [ ] Add “copy config snippet” to UI
- [ ] Add a “connection test” action

## Milestone 7 — Guardrails (Recommended)
- [ ] Extension allow/deny list per root (block `.env`, `.pem`, `.key` etc.)
- [ ] Max file size limits (read/write)
- [ ] Rate limiting / abuse protection
- [ ] Optional confirm-on-write/delete flow

## Milestone 8 — Packaging & Release (Cross-platform)
- [ ] Windows installer build
- [ ] macOS package build
- [ ] Linux package build (AppImage or deb/rpm)
- [ ] Release pipeline + artifacts
- [ ] Smoke tests for packaged apps

## Definition of Done (MVP)
- CRUD enforcement works per root
- No access outside configured active roots
- Works end-to-end with ChatGPT Desktop on Windows/macOS/Linux
- Audit logs show allow/deny with reason