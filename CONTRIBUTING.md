# Contributing to Conventional Commit Skill

Thank you for your interest in contributing! This document provides guidelines for contributing to the project.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Setup](#development-setup)
- [Testing Your Changes](#testing-your-changes)
- [Submitting Changes](#submitting-changes)
- [Style Guidelines](#style-guidelines)

## Code of Conduct

This project follows a simple code of conduct:

- Be respectful and considerate
- Welcome newcomers and help them learn
- Focus on constructive feedback
- Respect differing opinions and experiences

## How Can I Contribute?

### Reporting Bugs

Before creating bug reports, please check existing issues to avoid duplicates.

When creating a bug report, include:

- **Description**: Clear description of the issue
- **Steps to Reproduce**: Detailed steps to reproduce the behavior
- **Expected Behavior**: What you expected to happen
- **Actual Behavior**: What actually happened
- **Environment**:
  - Claude Code version
  - Operating System
  - Git version
- **Example**: Git diff or file changes that triggered the issue

**Example Bug Report:**

```markdown
**Description:** Skill fails to detect scope for deeply nested files

**Steps to Reproduce:**
1. Create file `src/features/auth/services/token/refresh.js`
2. Make changes to the file
3. Run `/conventional-commit`

**Expected:** Scope should be `auth` or `token`
**Actual:** No scope detected

**Environment:**
- Claude Code: 1.5.0
- OS: macOS 14.2
- Git: 2.42.0
```

### Suggesting Enhancements

Enhancement suggestions are welcome! Please include:

- **Use Case**: Why this enhancement would be useful
- **Current Behavior**: What currently happens
- **Proposed Behavior**: What you'd like to happen
- **Examples**: Concrete examples of the enhancement in action
- **Alternatives**: Other solutions you've considered

### Contributing Code

We welcome pull requests for:

- Bug fixes
- New features (after discussion in an issue)
- Documentation improvements
- Test additions
- Example additions

## Development Setup

### Prerequisites

- [Claude Code](https://github.com/anthropics/claude-code) installed
- Git 2.x or higher
- Basic understanding of YAML and Claude Code skills

### Local Setup

1. **Fork the repository** on GitHub

2. **Clone your fork:**
   ```bash
   cd ~/.claude/skills
   git clone https://github.com/YOUR_USERNAME/conventional-commit.git
   cd conventional-commit
   ```

3. **Create a test repository** for experimentation:
   ```bash
   mkdir ~/test-commit-skill
   cd ~/test-commit-skill
   git init
   ```

4. **Make changes** to `SKILL.md`

5. **Test immediately** in Claude Code:
   ```bash
   # In your test repository
   # Make some changes
   echo "console.log('test')" > test.js

   # Invoke the skill
   /conventional-commit
   ```

## Testing Your Changes

### Manual Testing Checklist

Before submitting a PR, test these scenarios:

#### Basic Scenarios

- [ ] **New feature** - Add new files, expect `feat` type
- [ ] **Bug fix** - Modify existing file with fix, expect `fix` type
- [ ] **Dependency update** - Update package.json, expect `chore(deps)`
- [ ] **Documentation** - Edit README, expect `docs` type
- [ ] **No changes** - Clean tree, expect "No changes" message

#### Edge Cases

- [ ] **Secret files** - Add .env file, confirm it's excluded
- [ ] **Already staged** - Stage files first, confirm skip staging
- [ ] **Mixed changes** - Multiple types, confirm dominant type chosen
- [ ] **Large diff** - Many files, confirm performance is acceptable
- [ ] **Binary files** - Add image, confirm appropriate handling

#### Manual Override

- [ ] **Type only** - `/conventional-commit feat` forces feat type
- [ ] **Type and scope** - `/conventional-commit fix api` forces both
- [ ] **Invalid arguments** - Test with invalid input

#### Scope Detection

- [ ] `src/api/users.js` → scope: `api`
- [ ] `src/auth/login.js` → scope: `auth`
- [ ] `package.json` → scope: `deps`
- [ ] `tests/unit/api.test.js` → scope: `api` or `tests`
- [ ] Single file at root → no scope or project name

#### Breaking Changes

- [ ] API signature change → adds `!` notation
- [ ] Removed public function → adds `!` notation
- [ ] Regular change → no `!`

### Test Template

Create different scenarios in your test repo:

```bash
# Scenario 1: New feature
mkdir -p src/api
echo "export const getUser = () => {}" > src/api/users.js
/conventional-commit
# Expected: feat(api): add user endpoint

# Scenario 2: Bug fix
echo "// Fixed null check" >> src/api/users.js
/conventional-commit
# Expected: fix(api): handle null user case

# Scenario 3: Dependency update
echo '{"dependencies": {"express": "4.18.2"}}' > package.json
/conventional-commit
# Expected: chore(deps): update express to 4.18.2
```

## Submitting Changes

### Pull Request Process

1. **Create a feature branch:**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes** with clear, focused commits

3. **Update documentation** if needed:
   - Update SKILL.md for behavior changes
   - Update README.md for user-facing changes
   - Add examples to EXAMPLES.md

4. **Test thoroughly** using the checklist above

5. **Commit your changes** (using this skill, of course!):
   ```bash
   /conventional-commit
   ```

6. **Push to your fork:**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Create a Pull Request** on GitHub with:
   - Clear title following Conventional Commits
   - Description of changes
   - Testing performed
   - Screenshots/examples if applicable

### PR Title Format

Use Conventional Commits for PR titles:

- `feat: add scope detection for monorepos`
- `fix: handle edge case in type detection`
- `docs: improve installation instructions`
- `refactor: simplify file staging logic`

### PR Description Template

```markdown
## Description
Brief description of what this PR does.

## Motivation
Why is this change needed? What problem does it solve?

## Changes Made
- Bullet point list of changes
- Another change
- Another change

## Testing
- [ ] Tested scenario A
- [ ] Tested scenario B
- [ ] Updated documentation
- [ ] Added examples

## Screenshots/Examples
If applicable, add examples of the new behavior.

## Related Issues
Fixes #123
Related to #456
```

## Style Guidelines

### SKILL.md Format

- Use clear, imperative language ("Generate", "Detect", "Stage")
- Include examples for complex logic
- Maintain consistent markdown formatting
- Keep line length reasonable (80-100 chars for readability)
- Use tables for structured information
- Add code blocks with appropriate language tags

### Documentation

- Write in clear, simple English
- Use examples liberally
- Include both success and failure cases
- Format code blocks properly
- Use consistent terminology

### YAML Frontmatter

```yaml
---
name: conventional-commit
description: Brief description (single line)
allowed-tools:
  - Bash(git add:*)
argument-hint: "[type] [scope]"
disable-model-invocation: true
---
```

### Git Commits

- Use this skill to generate your commit messages!
- Keep commits focused and atomic
- Write descriptive commit messages
- Reference issues when applicable

## Questions?

- **General questions**: Open a GitHub Discussion
- **Bug reports**: Open a GitHub Issue
- **Feature requests**: Open a GitHub Issue with `enhancement` label
- **Security issues**: Email privately (see README for contact)

## Recognition

Contributors will be:
- Listed in the README.md Contributors section
- Mentioned in release notes for significant contributions
- Credited in commit messages via Co-Authored-By

Thank you for contributing! 🎉
