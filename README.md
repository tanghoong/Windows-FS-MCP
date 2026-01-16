# Desktop FS MCP

[![CI](https://github.com/tanghoong/Desktop-FS-MCP/actions/workflows/ci.yml/badge.svg)](https://github.com/tanghoong/Desktop-FS-MCP/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Overview

Desktop FS MCP is a cross-platform Tauri desktop application that runs a Rust-based MCP (Model Context Protocol) server for controlled local filesystem access. It allows ChatGPT Desktop to securely access local files with fine-grained permissions.

## Features

- **Cross-Platform**: Fully functional on Windows, macOS, and Linux
- **MCP Server**: Rust-based server exposing controlled filesystem tools
- **Multiple Folder Roots**: Add multiple folder roots with independent settings
- **CRUD Permissions**: Fine-grained per-folder permissions (Create, Read, Update, Delete)
- **Security First**: Path normalization, containment checks, symlink protection
- **Audit Logging**: Track all file operations with allow/deny decisions
- **UI Framework**: Built with **Svelte** and **TypeScript** for a responsive interface
- **Database Storage**: **SQLite** for efficient configuration and log storage

## Tech Stack

- **Desktop Shell**: [Tauri](https://tauri.app/)
- **UI**: [Svelte](https://svelte.dev/) with TypeScript
- **Core Runtime**: Rust
- **Storage**: SQLite
- **Transport**: stdio-launched MCP server

## Documentation

- [Product Requirements Document](docs/PRD.md)
- [Technical Stack](docs/TECHSTACK.md)
- [Task List](docs/TASKS.md)
- [Versioning Policy](docs/VERSIONING.md)
- [Changelog](CHANGELOG.md)

## Getting Started

### Prerequisites

- Rust (latest stable)
- Node.js (v18+)
- pnpm (recommended) or npm

### Development

```bash
# Install dependencies
pnpm install

# Run in development mode
pnpm tauri dev

# Build for production
pnpm tauri build
```

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

## Security

For security concerns, please read our [Security Policy](SECURITY.md).

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.