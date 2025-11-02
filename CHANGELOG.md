# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- .gitignore templates for Python, Node.js, Java, and Go
- GitHub issue templates (Bug Report, Feature Request, Question)
- Pull Request template with comprehensive checklist
- CHANGELOG.md for tracking project history

## [2.0.0] - 2025-11-02

### Added
- **Advanced Git Techniques Guide**: Comprehensive guide covering rebase, cherry-pick, stash, reset/revert/checkout, reflog, bisect, and submodules
- **Security Best Practices**: Complete security guide with .gitignore patterns, credential management, GPG signing, branch protection, and sensitive data removal
- **Troubleshooting Guide**: Solutions to common Git problems including wrong branch commits, force push recovery, and merge conflicts
- **FAQ Section**: 40+ frequently asked questions with detailed answers
- **Glossary**: Comprehensive Git terminology reference
- **Quiz & Self-Assessment**: 56 questions across beginner, intermediate, and advanced levels
- **Real-World Scenarios**: Team collaboration case studies and practical examples
- **External Resources**: Curated list of learning materials, tools, and references

### Enhanced
- **README.md**: Added badges, quick start guide, learning paths, and better organization
- **git-basics.md**: Added "Try It Yourself" exercises and next steps
- **commit-message-best-practices.md**: Added conventional commits format, examples, and tools
- **branching-strategies.md**: Added visual diagrams, practice exercises, and best practices
- **git-workflows.md**: Added workflow comparison table, decision tree, and practice scenarios
- **contributing.md**: Added comprehensive code of conduct, PR templates, and style guidelines

### Improved
- Cross-references between all documents
- Hands-on exercises throughout
- Visual aids and diagrams
- Code examples with explanations
- Navigation and discoverability

## [1.0.0] - 2025-11-02

### Added
- Initial repository structure
- Git Basics documentation
- Git Workflows guide
- Resolving Merge Conflicts guide
- Commit Message Best Practices
- Branching Strategies documentation
- Contributing guidelines
- LICENSE.txt (MIT License)
- README.md with project overview

---

## Version History

- **2.0.0**: Major enhancement with 8 new comprehensive guides and enhanced existing documentation
- **1.0.0**: Initial release with core Git documentation

## Upgrade Guide

### From 1.x to 2.x

The 2.0 release is fully backward compatible. All original documentation remains available with significant enhancements. New users should follow the learning path in README.md.

Key improvements:
- 8 new comprehensive guides added
- All existing guides enhanced with exercises
- Better organization with cross-references
- Security-focused best practices
- Professional community guidelines

## Contributing to the Changelog

When making changes:
1. Add your changes under `[Unreleased]`
2. Use categories: Added, Changed, Deprecated, Removed, Fixed, Security
3. Link to relevant issues/PRs when applicable
4. Maintain chronological order (newest first)

Example:
```markdown
### Added
- New feature description [#123](link-to-pr)

### Fixed
- Bug fix description [#456](link-to-issue)
```

## Semantic Versioning

We follow [Semantic Versioning](https://semver.org/):

- **MAJOR** (X.0.0): Incompatible changes or major restructuring
- **MINOR** (x.X.0): New features or content, backward compatible
- **PATCH** (x.x.X): Bug fixes, typos, small improvements

Given this is a documentation project:
- Major: Complete reorganization or removed sections
- Minor: New guides, sections, or substantial additions
- Patch: Typo fixes, clarifications, small improvements

## Links

- [Keep a Changelog](https://keepachangelog.com/)
- [Semantic Versioning](https://semver.org/)
- [Contributing Guidelines](contributing.md)

[unreleased]: https://github.com/Kubomu/Best-Practices-for-Version-Control-GIT/compare/v2.0.0...HEAD
[2.0.0]: https://github.com/Kubomu/Best-Practices-for-Version-Control-GIT/compare/v1.0.0...v2.0.0
[1.0.0]: https://github.com/Kubomu/Best-Practices-for-Version-Control-GIT/releases/tag/v1.0.0
