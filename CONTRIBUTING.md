# Contributing to Queezy

This document provides comprehensive documentation about our development setup, including our code style enforcement tools, commit conventions, and Git workflow.

## Table of Contents

- [Development Environment Setup](#development-environment-setup)
- [Code Quality Tools](#code-quality-tools)
  - [ESLint Configuration](#eslint-configuration)
  - [Prettier Configuration](#prettier-configuration)
- [Git Workflow](#git-workflow)
  - [Conventional Commits](#conventional-commits)
  - [Branch Naming Convention](#branch-naming-convention)
  - [Pull Request Process](#pull-request-process)
- [Automation Tools](#automation-tools)
  - [Husky](#husky)
  - [lint-staged](#lint-staged)
  - [commitlint](#commitlint)
- [Interactive Commit Tool](#interactive-commit-tool)
- [Troubleshooting](#troubleshooting)

## Development Environment Setup

To contribute to this project, you'll need to set up your development environment:

```bash
# Clone the repository
git clone https://github.com/AhmedCode1996/queezy.git
cd queezy

# Install dependencies
pnpm install

# Start development server
pnpm dev
```

The `prepare` script will automatically set up Husky Git hooks when you run `pnpm install`.

## Code Quality Tools

### ESLint Configuration

We use ESLint to enforce code quality rules.

**Configuration:** Our ESLint configuration is in `eslint.config.js`:

```javascript
// eslint.config.js
import js from "@eslint/js";
import globals from "globals";
import reactHooks from "eslint-plugin-react-hooks";
import reactRefresh from "eslint-plugin-react-refresh";
import tseslint from "typescript-eslint";
import eslintConfigPrettier from "eslint-config-prettier";

export default tseslint.config(
  { ignores: ["dist"] },
  {
    extends: [js.configs.recommended, ...tseslint.configs.recommended],
    files: ["**/*.{ts,tsx}"],
    languageOptions: {
      ecmaVersion: 2020,
      globals: globals.browser,
    },
    plugins: {
      "react-hooks": reactHooks,
      "react-refresh": reactRefresh,
    },
    rules: {
      ...reactHooks.configs.recommended.rules,
      "react-refresh/only-export-components": [
        "warn",
        { allowConstantExport: true },
      ],
    },
  },
  eslintConfigPrettier.config,
);
```

**Usage:**

```bash
# Run ESLint on the entire project
pnpm lint

# Fix automatically fixable issues
pnpm lint --fix
```

### Prettier Configuration

We use Prettier to enforce consistent code formatting.

**Configuration:** Our Prettier configuration is in `.prettierrc`:

```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```

**Usage:**

```bash
# Format all files
pnpm format

# Format specific files
pnpm exec prettier --write src/components/Button.tsx
```

## Git Workflow

### Conventional Commits

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification for commit messages. This enables automatic versioning and changelog generation.

**Commit Message Format:**

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Types:**

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

**Examples:**

```
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

**Examples:**

```
feat/auth-google-login
fix/api-timeout-handling
docs/readme-update
```

### Pull Request Process

1. Create a branch following the branch naming convention
2. Make your changes with descriptive commit messages
3. Submit a pull request with a detailed description
4. Address any feedback from code reviews
5. Once approved, your code will be merged into the main branch

### Husky

[Husky](https://typicode.github.io/husky/) is a tool that lets us run scripts before Git actions like commit and push.

**Installation:**
Husky is installed automatically when you run `pnpm install` via the `prepare` script.

**Configuration:**
Our Husky hooks are defined in the `.husky` directory:

- `.husky/pre-commit`: Runs before commits to ensure code quality
- `.husky/commit-msg`: Validates commit messages

**Customizing Hooks:**
To modify a hook, edit the corresponding file in the `.husky` directory.

### lint-staged

[lint-staged](https://github.com/okonet/lint-staged) runs linters on staged files only, which makes the pre-commit hook run faster by only checking files you're committing.

**Configuration:**
Our lint-staged configuration is in `package.json`:

```json
"lint-staged": {
  "*.{js,jsx,ts,tsx}": [
    "eslint --fix",
    "prettier --write"
  ],
  "*.{json,css,scss,md,html}": [
    "prettier --write"
  ]
}
```

### commitlint

[commitlint](https://commitlint.js.org/) checks if your commit messages meet the conventional commit format.

**Configuration:**
Our commitlint configuration is in `commitlint.config.js`:

```javascript
export default {
  extends: ["@commitlint/config-conventional"],
  rules: {
    // Enforce conventional commit types
    "type-enum": [
      2,
      "always",
      [
        "feat", // New feature
        "fix", // Bug fix
        "docs", // Documentation only
        "style", // Code style/formatting
        "refactor", // Code restructure (no fixes/features)
        "perf", // Performance improvements
        "test", // Test-related changes
        "build", // Build system/dependencies
        "ci", // CI configuration
        "chore", // Maintenance tasks
        "revert", // Revert changes
      ],
    ],
    // Enforce type is not empty
    "type-empty": [2, "never"],
    // Enforce scope format if provided
    "scope-case": [2, "always", "kebab-case"],
    // First letter of subject should be capitalized
    "subject-case": [2, "always", "sentence-case"],
    // Subject cannot be empty
    "subject-empty": [2, "never"],
    // Subject should not end with period
    "subject-full-stop": [2, "never", "."],
    // Body should use proper line wrapping
    "body-max-line-length": [2, "always", 100],
    // No leading blank lines in body or footer
    "body-leading-blank": [2, "always"],
    "footer-leading-blank": [2, "always"],
  },
};
```

## Interactive Commit Tool

We've implemented an interactive commit tool that guides you through creating perfect conventional commits. This tool ensures consistency in our commit messages and makes it easier to generate changelogs and understand project history.

### Usage

```bash
pnpm commit
```

The tool will:

1. Show you which files are staged
2. Guide you through selecting a commit type (feat, fix, docs, etc.) with emojis
3. Help you determine the appropriate scope based on your current branch
4. Prompt for a short description of your changes
5. Allow you to add a longer description, breaking changes, and issue references
6. Format everything into a conventional commit message
7. Preview the final message before committing

### Smart Features

- **Branch-based scope suggestions**: The tool automatically suggests a scope based on your current branch name (e.g., `feature/auth` would suggest `auth` as the scope)
- **Validation**: Ensures your commit message meets our standards (proper capitalization, no trailing periods in subject)
- **Rich formatting**: Uses terminal colors and formatting for a better user experience
- **Pre-filled values**: Intelligently suggests values to make the commit process faster

### Examples

Here are some examples of commit messages generated by the tool:

```
✨ feat(auth): Add login with Google option
```

```
🐛 fix(ui): Resolve button alignment issue on mobile devices

Buttons were not properly aligned on smaller screens, causing overlap with other elements.

Closes: #123
```

```
📚 docs(api): Update API documentation with new endpoints

BREAKING CHANGE: The /users endpoint now requires authentication
```

### Keyboard Shortcuts

- Use arrow keys to navigate options
- Press `Enter` to select an option
- Press `Ctrl+C` at any time to cancel the commit process

### Bypassing the Tool

While we recommend using the interactive tool, you can still use standard Git commands if needed:

```bash
git commit -m "type(scope): message"
```

However, note that your commit will still be validated against our conventional commit standards by commitlint. 2. Allow you to select a commit type 3. Prompt for a scope (optional) 4. Help you write a properly formatted subject line 5. Allow for additional details like body and breaking changes 6. Preview the final commit message before committing

This is much easier than manually formatting commit messages and helps ensure consistency.

## Troubleshooting

### Common Issues

**"Husky hook not running"**

- Make sure you installed dependencies with `pnpm install`
- Check if the hook files in `.husky` are executable (`chmod +x .husky/*`)

**"ESLint/Prettier conflicts"**

- We use `eslint-config-prettier` to disable ESLint rules that might conflict with Prettier
- If you see conflicts, run `pnpm format` followed by `pnpm lint --fix`

**"Commit rejected due to commitlint"**

- Use `pnpm commit` to create valid commit messages
- Check the error message for guidance on fixing your commit message format

### Skipping Hooks

In rare cases when you need to bypass the pre-commit or commit-msg hooks:

```bash
# Skip all hooks
git commit --no-verify -m "feat: Add new feature"

# Skip specific hook
HUSKY=0 git commit -m "feat: Add new feature"
```
