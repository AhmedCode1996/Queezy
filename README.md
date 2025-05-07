# Queezy

## Git Workflow & Commit Standards

This project uses conventional commits to maintain a clean and meaningful commit history. We've implemented several tools to help you follow these standards.

### ✨ Interactive Commit Wizard

We've built a stylish and interactive commit message wizard to help you create perfect conventional commits without memorizing all the rules.

```bash
pnpm commit
```

The tool will guide you through the commit process with features like:

- 🎨 Visual commit type selection with emojis
- 🔍 Smart scope suggestions based on your branch name
- ✅ Validation to ensure proper formatting
- 📋 Preview of your commit message before finalizing

For more details, see the [Contributing Guide](./CONTRIBUTING.md#interactive-commit-tool).

> **For complete documentation on our development setup, including installation and detailed configuration instructions, see [CONTRIBUTING.md](./CONTRIBUTING.md).**

### Commit Message Format

All commits must follow this format:

```
<type>[optional scope]: <Subject starting with capital letter>

[optional body]

[optional footer(s)]
```

### Available Commit Types

| Type       | Description                                                   |
| ---------- | ------------------------------------------------------------- |
| `feat`     | A new feature                                                 |
| `fix`      | A bug fix                                                     |
| `docs`     | Documentation only changes                                    |
| `style`    | Changes that do not affect the meaning of the code            |
| `refactor` | A code change that neither fixes a bug nor adds a feature     |
| `perf`     | A code change that improves performance                       |
| `test`     | Adding missing tests or correcting existing tests             |
| `build`    | Changes that affect the build system or external dependencies |
| `ci`       | Changes to our CI configuration files and scripts             |
| `chore`    | Other changes that don't modify src or test files             |
| `revert`   | Reverts a previous commit                                     |

### Interactive Commit Tool

We've implemented an interactive commit message wizard to help you create perfect commit messages:

```bash
pnpm commit
```

This will launch a stylish CLI that guides you through creating a conventional commit message with proper formatting.

### Commit Message Examples

```bash
feat: Add user authentication
feat(auth): Add Google login option
fix: Resolve API timeout issue
docs: Update README with deployment instructions
refactor(core): Extract shared utility functions
```

### Branch Naming Convention

Branches should follow this format:

```
<type>/<scope>-<description>
```

Examples:

```
feat/auth-google-login
fix/api-timeout-handling
docs/readme-update
```

### Git Workflow Commands

Use these commands for your Git workflow:

#### Creating a new branch

```
create branch for: <description of what you're working on>
```

#### Committing changes

```
commit changes for: <description of what you changed>
```

Or use the interactive commit tool: `pnpm commit`

#### Creating a PR description

```
create PR description for: <description of your changes>
```

#### Complete workflow

```
handle git workflow for: <description of your feature/fix>
```

### Enforced Standards

This project automatically enforces code quality and commit message standards:

#### Code Quality

When you attempt to commit changes, the following will happen automatically:

1. **ESLint** runs on all JavaScript/TypeScript files to fix linting issues
2. **Prettier** formats all staged files according to project standards
3. Changes made by formatters are automatically added to your commit

This ensures consistent code style throughout the project without manual effort.

#### Commit Message Standards

Commit messages are automatically validated using:

1. **commitlint** - Validates commit message format
2. **husky** - Git hooks for running validation
3. **lint-staged** - Runs linters/formatters on staged files only

If your commit message doesn't meet the standards, you'll see a helpful error message with examples of correctly formatted messages.

#### Setup Details

The automation is configured in:

- `.husky/pre-commit` - Runs lint-staged before each commit
- `.husky/commit-msg` - Validates commit message format
- `package.json` - Contains lint-staged configuration
