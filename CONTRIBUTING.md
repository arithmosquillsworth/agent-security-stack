# Contributing to Agent Security Stack

Thank you for your interest in contributing! This document provides guidelines for contributing to the Agent Security Stack.

## Getting Started

1. **Fork the repository** you want to contribute to
2. **Clone your fork** locally
3. **Create a branch** for your changes
4. **Make your changes** following our guidelines
5. **Submit a pull request**

## Development Setup

### Prerequisites

- Go 1.21+ (for Go-based tools)
- Node.js 18+ (for TypeScript/JavaScript tools)
- Foundry (for Solidity contracts)
- Git

### Installing Dependencies

Each tool has its own dependencies. Check the individual README in each repository.

```bash
# Example for Go tools
cd agent-cli
go mod download

# Example for Node tools
cd prompt-guard
npm install
```

## How to Contribute

### Reporting Bugs

1. Check if the bug has already been reported in [Issues](../../issues)
2. If not, create a new issue with:
   - Clear title and description
   - Steps to reproduce
   - Expected vs actual behavior
   - Environment details (OS, Go version, etc.)
   - Any relevant logs or screenshots

### Suggesting Features

1. Check existing [Issues](../../issues) and [Discussions](../../discussions)
2. Open a new issue with the `enhancement` label
3. Describe:
   - The problem you're trying to solve
   - Your proposed solution
   - Any alternatives you've considered

### Pull Requests

1. **Create a feature branch**
   ```bash
   git checkout -b feature/my-feature
   # or
   git checkout -b fix/my-bugfix
   ```

2. **Make your changes**
   - Follow existing code style
   - Add tests for new functionality
   - Update documentation as needed

3. **Test your changes**
   ```bash
   # Run tests
   go test ./...
   
   # Run linter
   golangci-lint run
   ```

4. **Commit your changes**
   ```bash
   git add .
   git commit -m "type: description"
   ```

   Follow [Conventional Commits](https://www.conventionalcommits.org/):
   - `feat:` new feature
   - `fix:` bug fix
   - `docs:` documentation
   - `test:` tests
   - `refactor:` code refactoring
   - `security:` security fix

5. **Push and create PR**
   ```bash
   git push origin feature/my-feature
   ```
   Then open a pull request on GitHub.

## Code Style

### Go

- Follow [Effective Go](https://golang.org/doc/effective_go.html)
- Use `gofmt` for formatting
- Run `golangci-lint` before submitting
- Document all exported functions
- Keep functions focused and small

### TypeScript/JavaScript

- Use TypeScript for new code
- Follow [TypeScript Style Guide](https://google.github.io/styleguide/tsguide.html)
- Run `eslint` and `prettier`

### Solidity

- Follow [Solidity Style Guide](https://docs.soliditylang.org/en/latest/style-guide.html)
- Use `forge fmt` for formatting
- Include NatSpec comments

## Testing

- All new features must include tests
- Aim for >80% code coverage
- Include integration tests where applicable
- Test edge cases and error conditions

## Documentation

- Update README.md if adding new features
- Add examples for new functionality
- Update CHANGELOG.md following [Keep a Changelog](https://keepachangelog.com/)

## Security

- **Never** commit private keys, API keys, or secrets
- Use `git secrets` to scan before committing
- Report security issues privately to the maintainers
- Follow responsible disclosure

## Code Review Process

1. All PRs require at least one review
2. Address review feedback promptly
3. Maintain respectful communication
4. Be open to suggestions and alternative approaches

## Recognition

Contributors will be:
- Listed in the repository's CONTRIBUTORS file
- Mentioned in release notes for significant contributions
- Added to the project's hall of fame (coming soon)

## Questions?

- Open a [Discussion](../../discussions)
- DM on X: [@0xarithmos](https://x.com/0xarithmos)
- Email: See agent profile on [8004scan.io](https://8004scan.io/agents/base/1941)

## Code of Conduct

- Be respectful and inclusive
- Welcome newcomers
- Focus on constructive feedback
- Assume good intentions

---

**Thank you for helping make AI agents more secure! 🔒**
