# Conventional Commit Skill for Claude Code

A Claude Code skill that automatically generates [Conventional Commits](https://www.conventionalcommits.org) format messages based on your git changes, with intelligent type and scope detection.

## Features

- **Smart Type Detection**: Automatically determines commit type (`feat`, `fix`, `chore`, etc.) from your changes
- **Intelligent Scope Inference**: Detects scope from file paths and directory structure
- **Breaking Change Detection**: Identifies API changes and adds `!` notation
- **Selective File Staging**: Auto-stages relevant files while excluding secrets
- **Two-Phase Review**: Shows generated message before committing for approval
- **Manual Override**: Force specific type/scope via arguments
- **Security-First**: Never stages `.env`, credentials, or sensitive files

## Installation

### Prerequisites

- [Claude Code CLI](https://github.com/anthropics/claude-code) installed and configured

### Install via Claude Code Marketplace (recommended)

Add the marketplace and install the plugin from within Claude Code:

```
/plugin marketplace add TroodoNmike/conventional-commit
/plugin install conventional-commit@conventional-commit
```


### Install manually

1. Clone this repository:

```bash
git clone https://github.com/TroodoNmike/conventional-commit.git
```

2. Add it as a local marketplace in `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "conventional-commit": {
      "source": {
        "source": "directory",
        "path": "/path/to/conventional-commit"
      },
      "autoUpdate": false
    }
  }
}
```

3. Install the plugin:

```
/plugin install conventional-commit@conventional-commit
```

## Usage

### Basic Usage (Auto-detect Type & Scope)

```bash
/conventional-commit
```

Claude will:
1. Analyze your git changes
2. Auto-detect the appropriate commit type and scope
3. Stage relevant files
4. Present a Conventional Commits message for review
5. Wait for your approval before committing

### Force Commit Type

```bash
/conventional-commit feat
```

Forces type to `feat`, auto-detects scope.

### Force Type and Scope

```bash
/conventional-commit fix api
```

Forces type to `fix` and scope to `api`.

## Example Output

```
Generated Conventional Commit message:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
feat(auth): add JWT token refresh endpoint
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Files staged (4 total):
- src/auth/token.js
- src/auth/refresh.js
- tests/auth/token.test.js
- docs/api/auth.md

To commit with this message, I can run:
git commit -m "feat(auth): add JWT token refresh endpoint"

Would you like me to:
1. Commit with this message
2. Commit and push with this message
3. Discuss the message
```

## Commit Types

The skill recognizes these standard Conventional Commits types:

| Type | Description | Example |
|------|-------------|---------|
| `feat` | New feature | `feat(api): add user endpoint` |
| `fix` | Bug fix | `fix(auth): resolve token expiry` |
| `docs` | Documentation | `docs: update README` |
| `style` | Formatting | `style: run prettier` |
| `refactor` | Code restructuring | `refactor(api): extract handlers` |
| `perf` | Performance | `perf(db): add query caching` |
| `test` | Tests | `test(api): add integration tests` |
| `build` | Build system | `build: update webpack config` |
| `ci` | CI configuration | `ci: add github actions` |
| `chore` | Maintenance | `chore(deps): update express` |
| `revert` | Revert commit | `revert: rollback feature X` |

### Breaking Changes

Add `!` after type/scope for breaking changes:

```
feat(api)!: change authentication response format
fix!: remove deprecated user endpoints
```

## Type Detection Logic

The skill uses intelligent pattern matching:

| Changes Detected | Type | Example |
|-----------------|------|---------|
| New files/functions/features | `feat` | New API endpoint |
| Bug fixes, error handling | `fix` | Null check fix |
| Only dependencies | `chore(deps)` | npm update |
| Only documentation | `docs` | README update |
| Only tests | `test` | Add unit tests |
| Code restructuring | `refactor` | Extract functions |
| Only formatting | `style` | Prettier format |
| Performance optimizations | `perf` | Cache implementation |

## Scope Detection

Automatically inferred from file paths:

- `src/api/file.js` → `api`
- `src/auth/login.js` → `auth`
- `package.json` changes → `deps`
- `*.test.js` → `tests`
- Common parent directory → directory name

Scope is optional and omitted if unclear.

## Security Features

The skill never stages sensitive files:

- `.env`, `.env.*`
- `credentials.json`
- `*.key`, `*.pem`
- Files containing `secret`, `password`, `token`

If detected, you'll see:

```
⚠️  Detected potential secret files: .env.local, api-key.txt
These will NOT be staged.
```

## Description Guidelines

Generated descriptions follow best practices:

- ✅ **Imperative mood**: "add" not "added"
- ✅ **Lowercase**: Start with lowercase after colon
- ✅ **No period**: Don't end with period
- ✅ **Length**: 50-72 characters
- ✅ **Specific**: What changed, not why/how

**Good Examples:**
- `feat(api): add user authentication endpoint`
- `fix(auth): resolve token expiration issue`
- `chore(deps): update express to 4.18.2`

**Bad Examples:**
- ❌ `feat(api): Added a new endpoint for user authentication.`
- ❌ `fix: fixed bug`
- ❌ `Update files`

## Edge Cases

### No Changes

```
No changes to commit. Working tree is clean.
```

### Already Staged Changes

```
Using already staged changes (3 files)
```

### Mixed Changes

If changes span multiple concerns:

```
Detected mixed changes (feat + docs + tests)
Using dominant type: feat

Consider splitting into multiple commits for: docs, tests
```

## Configuration

The skill uses these tool permissions:

```yaml
allowed-tools:
  - Bash(git add:*)
  - Bash(git status:*)
  - Bash(git diff:*)
  - Bash(git log:*)
```

Only read-only git operations and selective staging are permitted.

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines on:

- Reporting bugs
- Suggesting enhancements
- Development setup
- Testing procedures
- Submitting pull requests

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Related

- [Conventional Commits Specification](https://www.conventionalcommits.org)
- [Claude Code Documentation](https://github.com/anthropics/claude-code)
- [Semantic Versioning](https://semver.org)

## Changelog

### v1.0.0 (2026-02-08)

- Initial release
- Auto-detection of commit type and scope
- Selective file staging with secret exclusion
- Two-phase review process
- Breaking change detection
- Manual type/scope override support

---

Made with [Claude Code](https://claude.com/claude-code)
