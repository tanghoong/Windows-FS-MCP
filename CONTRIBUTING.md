# Contributing to Desktop FS MCP

Thank you for your interest in contributing to Desktop FS MCP! This document provides guidelines and information for contributors.

## Code of Conduct

By participating in this project, you agree to maintain a respectful and inclusive environment for everyone.

## Getting Started

### Prerequisites

- **Rust** (latest stable version)
- **Node.js** (v18 or later)
- **pnpm** (recommended) or npm
- **Tauri CLI** (`cargo install tauri-cli`)

### Development Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/tanghoong/Desktop-FS-MCP.git
   cd Desktop-FS-MCP
   ```

2. Install dependencies:
   ```bash
   pnpm install
   ```

3. Run in development mode:
   ```bash
   pnpm tauri dev
   ```

4. Build for production:
   ```bash
   pnpm tauri build
   ```

## How to Contribute

### Reporting Bugs

1. Check if the bug has already been reported in the [Issues](https://github.com/tanghoong/Desktop-FS-MCP/issues).
2. If not, create a new issue with:
   - A clear, descriptive title
   - Steps to reproduce the bug
   - Expected vs actual behavior
   - Your environment (OS, version, etc.)

### Suggesting Features

1. Check existing issues and discussions for similar suggestions.
2. Create a new issue with the `enhancement` label.
3. Describe the feature and its use case clearly.

### Submitting Pull Requests

1. Fork the repository.
2. Create a feature branch from `main`:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. Make your changes following our coding standards.
4. Write or update tests as needed.
5. Run the test suite:
   ```bash
   cargo test
   pnpm test
   ```
6. Run linting:
   ```bash
   cargo fmt --check
   cargo clippy
   pnpm lint
   ```
7. Commit your changes with clear messages.
8. Push to your fork and create a pull request.

## Coding Standards

### Rust Code

- Follow the [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/).
- Use `rustfmt` for formatting.
- Address all `clippy` warnings.
- Write documentation comments for public APIs.

### TypeScript/Svelte Code

- Use TypeScript for all new code.
- Follow the existing code style.
- Use meaningful variable and function names.

### Commit Messages

- Use clear, descriptive commit messages.
- Start with a verb in present tense (e.g., "Add", "Fix", "Update").
- Reference issue numbers when applicable.

## Testing

- Write unit tests for new functionality.
- Ensure all existing tests pass before submitting.
- Include integration tests for MCP tool implementations.
- Test security-critical code thoroughly (path handling, permissions).

## Documentation

- Update documentation for any user-facing changes.
- Add inline comments for complex logic.
- Keep the README and docs folder up to date.

## Review Process

1. All PRs require at least one review before merging.
2. CI checks must pass.
3. Address reviewer feedback promptly.

## Questions?

Feel free to open a discussion or issue if you have questions about contributing.

Thank you for helping make Desktop FS MCP better!
