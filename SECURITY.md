# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 1.x.x   | :white_check_mark: |
| < 1.0   | :x:                |

## Reporting a Vulnerability

We take security seriously. If you discover a security vulnerability in Desktop FS MCP, please report it responsibly.

### How to Report

1. **Do not** disclose the vulnerability publicly until it has been addressed.
2. Email your findings to the project maintainers (see repository contacts).
3. Include the following information in your report:
   - A clear description of the vulnerability
   - Steps to reproduce the issue
   - Potential impact of the vulnerability
   - Any suggested fixes (optional)

### What to Expect

- **Acknowledgment**: We will acknowledge receipt of your report within 48 hours.
- **Assessment**: We will investigate and assess the vulnerability within 7 days.
- **Resolution**: We aim to release a fix within 30 days for critical vulnerabilities.
- **Credit**: We will credit you in the release notes (unless you prefer to remain anonymous).

### Scope

This security policy covers:
- The Desktop FS MCP application
- The MCP server component
- Path security and containment mechanisms
- Permission enforcement systems

### Security Best Practices for Users

1. **Principle of Least Privilege**: Only grant read access by default. Enable write/delete only when necessary.
2. **Folder Selection**: Be cautious about which folders you expose to the MCP server.
3. **Extension Blocklist**: Use extension blocklists to prevent access to sensitive files (`.env`, `.pem`, `.key`).
4. **Regular Audits**: Review audit logs regularly to monitor file access patterns.
5. **Keep Updated**: Always use the latest version of Desktop FS MCP.

## Security Features

Desktop FS MCP includes several security features:

- **Path Normalization**: All paths are canonicalized before access checks.
- **Root Containment**: Operations are confined to explicitly allowed folder roots.
- **Symlink Protection**: Symlinks that escape allowed roots are denied.
- **CRUD Permissions**: Fine-grained per-folder permissions (Create, Read, Update, Delete).
- **Audit Logging**: All file operations are logged with allow/deny decisions.
