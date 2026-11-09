---
name: code-best-practices
description: Use when writing, refactoring or committing code. A clean-code workflow covering repo exploration, simple and reusable design, testing, an adversarial security/quality review loop, documentation, and Conventional Commits.
---

# Writing Clean Code

Write simple, readable code that humans (or AI) can maintain easily. Avoid unnecessary complexity: it makes future maintenance almost impossible.

## Workflow

Copy this checklist and track your progress:

```
- [ ] 1. Explore the repository (conventions and existing modules)
- [ ] 2. Write the code following the principles below
- [ ] 3. Test it in several scenarios
- [ ] 4. Run the adversary agent and fix its findings
- [ ] 5. Document key decisions
- [ ] 6. Commit using Conventional Commits
```

## 1. Before coding

- **Follow the existing code style.** Explore the repository to understand its conventions, then follow them. This helps future reviews and maintenance.
- **Reuse before you write.** Search the repository for existing code or modules that already solve the problem. Do not duplicate them (DRY: Don't Repeat Yourself). If nothing reusable exists, create a new small module.

## 2. While coding

- **Keep it simple.** Humans handle complexity poorly, so choose the simplest data structure and the simplest code structure that works.
- **Use obvious names.** Name functions, classes, and variables so their purpose is clear without extra explanation.
- **Comment with care.** Use comments to make your decisions clear, but avoid excess: too many comments clutter the code.
- **Give each function or class a single responsibility.** Split code into small, modular pieces that you can reuse.
- **Avoid global variables.** Pass dependencies as parameters instead.
- **Handle errors and exceptions.** Good error handling makes debugging easier when execution fails.

## 3. After coding

### Test

Test the code in various scenarios, including normal, edge, and error cases.

### Security review

Every time the code is ready:

1. Launch the adversary agent defined in `code-best-practices-adversary-agent/AGENT.md`. Give it **no context** about how or why the code was written, so it can search for problems and vulnerabilities with fresh eyes.
2. Fix every finding it reports.
3. Run the adversary agent again. Repeat until it reports no new findings.

### Document

Document crucial code decisions with comments or standard documentation, such as diagrams, environment setup, and how to deploy and test the code.

### Version control

Always use version control while you write code, and write commit messages using [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/):

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

- `feat`: introduces a new feature (SemVer MINOR).
- `fix`: patches a bug (SemVer PATCH).
- Other common types: `build`, `chore`, `ci`, `docs`, `style`, `refactor`, `perf`, `test`.
- **Breaking change** (SemVer MAJOR): add `!` after the type or scope, or add a `BREAKING CHANGE:` footer. This can be used with any type.
- A scope gives extra context, in parentheses: `feat(parser): add ability to parse arrays`