# PRD: Tauri (Rust) Desktop MCP Server for Controlled Local Folder Access

- **Date**: 2026-01-16 13:38:37  
- **Owner**: TBD  
- **Status**: Draft  
- **Target platforms**: Windows (MVP), macOS/Linux (Phase 2)  
- **Primary integration**: ChatGPT Desktop via MCP (local tools)

---

## 1) Overview

### 1.1 Problem
Users want ChatGPT Desktop to access local files to summarize documents, manipulate project files, and automate workflows. Current approaches (e.g., ad-hoc scripts or generic filesystem MCP servers) often lack:
- a user-friendly GUI for selecting multiple folders,
- fine-grained permissions per folder (CRUD),
- quick enable/disable controls,
- auditing/visibility into what the model accessed.

### 1.2 Solution
Build a **Tauri desktop application** that runs a **Rust-based MCP server** exposing controlled filesystem tools to ChatGPT Desktop. Users manage access through a GUI:
- Add **multiple folder roots**
- Toggle **Active** per folder
- Configure **CRUD permissions** per folder
- (Recommended) Add extension allow/deny rules, size limits, and audit logs

### 1.3 Guiding Principles
- **Least privilege by default**
- **Explicit user intent**
- **Secure path handling** (no traversal, symlink escape)
- **Transparent auditing**

---

## 2) Goals & Success Metrics

### 2.1 Goals (MVP)
1. Provide a stable MCP server that ChatGPT Desktop can launch/connect to.
2. Allow users to manage multiple folder roots with per-folder:
   - active status
   - CRUD permissions
3. Enforce permissions at runtime for every tool call.
4. Provide an audit trail of accesses and denials.

### 2.2 Success Metrics
- **Adoption/Activation**
  - % of installs that enable MCP server
  - # of configured roots per user
- **Safety**
  - 0 known path traversal escapes in security tests
  - % denied operations due to policy (expected non-zero)
- **Reliability**
  - Server crash-free sessions
  - Tool call success rate
- **UX**
  - Time-to-first-successful connection with ChatGPT Desktop

---

## 3) Personas & Primary Use Cases

### 3.1 Personas
- **Developer**: wants project folder access with write support but no deletes.
- **Knowledge worker**: wants read-only access to documents.
- **Security-conscious user**: wants strict allowlist and detailed logs.

### 3.2 Use Cases
1. Add multiple folders and grant read-only to “Documents”, read/write to “Project”.
2. Temporarily deactivate a folder without removing it.
3. Allow creating/updating files in a folder but disallow deletion.
4. Review audit logs to understand what was accessed and when.
5. Deny access outside configured roots, including traversal attempts.

---

## 4) Scope

### 4.1 In Scope (MVP)
- Windows desktop app (Tauri) UI
- Local configuration storage
- MCP server (Rust) exposing filesystem tools
- Per-folder Active + CRUD
- Path security + enforcement
- Basic audit logging + export

### 4.2 Out of Scope (MVP)
- Remote/network access (non-localhost)
- Full-text indexing / embeddings store
- Cloud sync of settings
- Enterprise policy management
- Advanced secret scanning/redaction (Phase 2+)

---

## 5) Functional Requirements

### 5.1 Folder Registry & Governance (UI)
**FR-1 Add Folder Root**
- User can add a folder via folder picker and/or manual path input.
- Validate path exists and is accessible.
- Store: `id`, `display_name`, `absolute_path`, timestamps.

**FR-2 Multiple Roots**
- Support multiple folder roots.
- Each root can be enabled/disabled via `active` toggle.

**FR-3 Per-Folder Permissions (CRUD)**
- Each root has independent permission flags:
  - **C**: create files/directories
  - **R**: list/read files + metadata
  - **U**: update/overwrite existing files
  - **D**: delete files/directories
- Defaults: `R=true`, `C/U/D=false` (recommended safest default).

**FR-4 Edit/Remove Root**
- Users can update display name, toggle active, update permissions.
- Remove root (hard delete from config).

**FR-5 Global MCP Enable**
- Global toggle to enable/disable MCP server.
- When disabled, all tool calls are refused.

### 5.2 MCP Server (Rust) Tools
**FR-6 Supported Tools (MVP)**
Minimum tool set:
- `list_roots()` → returns active roots and their permissions (optional but recommended)
- `list_dir(path)`
- `read_file(path)`
- `stat(path)`
- `write_file(path, content, mode)` where mode indicates create vs overwrite
- `create_dir(path)`
- `delete_path(path)`

Optional (nice-to-have):
- `move_path(src, dst)`
- `copy_path(src, dst)`
- `search_files(query, root, glob)`

**FR-7 Transport**
- Support the transport required by ChatGPT Desktop for local MCP servers.
- Preferred: host-launched process using stdio.
- Alternative: local loopback socket with explicit configuration.

**FR-8 Error Semantics**
- When denied, return a clear error: `DeniedByPolicy` with reason (inactive root, missing permission, outside roots, etc).

---

## 6) Security & Policy Enforcement Requirements

### 6.1 Path Normalization & Containment
**SR-1 Canonicalization**
- Normalize and canonicalize paths before evaluating containment.
- Deny invalid paths and reserved device names where relevant (Windows).

**SR-2 Root Containment**
- Every file operation must be confined to an allowed root.
- Deny path traversal patterns and any resolved path outside roots.

**SR-3 Symlink / Junction Handling**
- MVP recommended policy: **deny symlinks/junctions that escape root**.
- Decide whether to allow symlinks that remain within root (Open Question).

### 6.2 Permissions
**SR-4 Per-Operation Permission Check**
- Enforce CRUD on every operation.
- Examples:
  - `read_file` requires `R`
  - `list_dir` requires `R`
  - `write_file` requires `C` (if new) or `U` (if overwrite)
  - `delete_path` requires `D`

### 6.3 Audit Logging
**SR-5 Audit Record**
Each tool call logs:
- timestamp
- tool name
- requested path(s)
- matched root id/path
- allow/deny + reason
- bytes read/written (approx)
- execution time (optional)

**SR-6 Privacy**
- Logs are local.
- Provide option to redact file contents from logs (default).

---

## 7) Suggested Enhancements (Strong Recommendations)

### 7.1 Extension Allow/Deny Lists (Per Root)
**Rationale**: users frequently have `.env`, `.pem`, `.key`, credentials in folders.
- Per root:
  - allowlist extensions (e.g. `.md`, `.txt`, `.json`)
  - blocklist extensions (e.g. `.env`, `.key`, `.pem`)
- Default: no allowlist, but blocklist common secret types (configurable).

### 7.2 Size Limits
- Max readable file size (e.g. 10 MB default).
- Max single write size (e.g. 5 MB default).

### 7.3 Confirm-on-Write/Delete (Guardrail)
- Per-root setting to require user confirmation for write/delete.
- If interactive approval is not feasible with ChatGPT Desktop, fallback to deny + prompt user to toggle permission.

### 7.4 Workspaces/Profiles
- Switch between sets of roots (Work/Personal) quickly.

### 7.5 Rate Limiting / Abuse Protection
- Basic throttling to avoid accidental infinite loops or high-frequency operations.

---

## 8) UX Requirements (MVP)

### 8.1 Screens
1. **Status**
   - MCP enabled toggle
   - Running/stopped indicator
   - Connection / diagnostics info
2. **Folders**
   - List: Name, Path, Active, CRUD flags, Edit, Remove
   - Add folder button (picker)
3. **Logs**
   - Table of audit entries
   - Filters (allowed/denied, tool, root)
   - Export logs

### 8.2 Accessibility
- Keyboard navigation for toggles and list actions.
- Clear permission labels (avoid jargon-only UI).

---

## 9) Technical Requirements

### 9.1 Storage
- Local database (SQLite recommended) or structured config file.
- Export/import configuration (JSON).

### 9.2 Packaging
- Windows installer (MSI/EXE) via Tauri bundling.
- Code signing (later).

### 9.3 Compatibility
- Windows 11 (MVP baseline).
- Consider long path support and unicode paths.

---

## 10) Risks & Mitigations

### Risk: Path traversal / symlink escape
- **Mitigation**: canonicalize, containment checks, conservative symlink policy, security tests.

### Risk: ChatGPT Desktop MCP transport differences
- **Mitigation**: validate against target Desktop versions early; implement stdio mode first.

### Risk: User accidentally grants too much access
- **Mitigation**: safe defaults, warnings, extension blocklist, confirm-on-write/delete modes.

---

## 11) Acceptance Criteria (MVP)
1. User can add at least 2 roots; each has active toggle and CRUD flags.
2. When a root is inactive, all operations under it are denied.
3. With `R` only, server permits list/read/stat but denies create/update/delete.
4. With `C` enabled, server can create new files/dirs under that root.
5. With `U` enabled, server can overwrite/modify existing files under that root.
6. With `D` disabled, delete operations are denied even if other permissions are enabled.
7. Attempts to access outside configured roots are denied (including `..` traversal).
8. Audit log records allow/deny decisions with reason.

---

## 12) Open Questions
1. **Overlap policy**: If roots overlap, which permissions apply?
   - Recommendation: most-specific-root match wins; if tie, most restrictive.
2. **Symlink policy**: Deny all symlinks vs allow within-root symlinks?
3. **Transport model**: stdio-launched vs background server?
4. **Cross-platform**: ship Windows-only MVP or include macOS/Linux in MVP?