# Contributing to Build and Push Docker Image Action

Thank you for your interest in contributing to this GitHub Action! We welcome contributions from the community.

## How to Contribute

### Reporting Issues

If you find a bug or have a suggestion for improvement:

1. Check if the issue already exists in our [GitHub Issues](https://github.com/optivem/build-push-docker-action/issues)
2. If not, create a new issue with:
   - A clear title and description
   - Steps to reproduce the issue
   - Expected vs actual behavior
   - Your environment details (OS, Docker version, etc.)

### Submitting Changes

1. **Fork the repository**
2. **Create a feature branch** from `main`:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes** following our guidelines below
4. **Test your changes** thoroughly
5. **Commit your changes** with clear commit messages
6. **Push to your fork** and submit a pull request

### Development Guidelines

#### Code Style
- Follow YAML best practices for `action.yml`
- Use clear, descriptive names for inputs and steps
- Add comments where necessary for complex logic

#### Testing
- Test your changes with a real Docker build scenario
- Ensure the action works with different working directories
- Verify authentication and push functionality

#### Documentation
- Update README.md if you add new features or change behavior
- Update CHANGELOG.md following the existing format
- Add examples for new functionality

### Pull Request Process

1. **Update documentation** as needed
2. **Add tests** for new functionality
3. **Ensure CI passes** - our automated tests must pass
4. **Update CHANGELOG.md** with your changes
5. **Request review** from maintainers

### Versioning

We use [Semantic Versioning](https://semver.org/):
- **MAJOR** version for incompatible API changes
- **MINOR** version for backwards-compatible functionality additions
- **PATCH** version for backwards-compatible bug fixes

### Release Process

1. Update CHANGELOG.md with the new version
2. Create a new tag: `git tag v1.x.x`
3. Push the tag: `git push origin v1.x.x`
4. The release workflow will automatically create a GitHub release

## Development Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/optivem/build-push-docker-action.git
   cd build-push-docker-action
   ```

2. **Make your changes** to `action.yml`

3. **Test locally** by creating a test workflow in another repository

4. **Validate YAML** syntax:
   ```bash
   python -c "import yaml; yaml.safe_load(open('action.yml'))"
   ```

## Questions?

If you have questions about contributing, feel free to:
- Open an issue for discussion
- Contact the maintainers
- Check existing issues and pull requests for similar topics

## Code of Conduct

We are committed to providing a welcoming and inspiring community for all. Please be respectful and constructive in all interactions.

## License

By contributing to this project, you agree that your contributions will be licensed under the MIT License.