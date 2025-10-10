# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed
- Repository renamed from `build-push-docker-action` to `publish-docker-image-action`
- Action name updated from "Build and Push Docker Image" to "Publish Docker Image"

## [1.0.1] - 2025-10-09

### Added
- Echo step at the beginning of the action for better debugging output
- Display image name being built in the initial echo step

### Changed
- Improved action output visibility for troubleshooting

## [1.0.0] - 2025-10-09

### Added
- Initial release of the Publish Docker Image Action
- Support for building Docker images from Dockerfile
- Automatic push to GitHub Container Registry (ghcr.io)
- Image tagging with Git SHA and latest tag
- Secure authentication using GitHub tokens
- Support for custom working directories
- Comprehensive input validation

### Features
- Composite action for fast execution
- Automatic login to GitHub Container Registry
- Proper image tagging strategy
- Clean and simple interface

[Unreleased]: https://github.com/optivem/publish-docker-image-action/compare/v1.0.1...HEAD
[1.0.1]: https://github.com/optivem/publish-docker-image-action/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/optivem/publish-docker-image-action/releases/tag/v1.0.0