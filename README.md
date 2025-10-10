# Build and Push Docker Image Action

[![GitHub release](https://img.shields.io/github/release/optivem/build-push-docker-action.svg)](https://github.com/optivem/build-push-docker-action/releases)
[![GitHub marketplace](https://img.shields.io/badge/marketplace-build--push--docker--action-blue?logo=github)](https://github.com/marketplace/actions/build-and-push-docker-image)
[![CI](https://github.com/optivem/build-push-docker-action/workflows/CI/badge.svg)](https://github.com/optivem/build-push-docker-action/actions)

A GitHub Action that builds Docker images and pushes them to GitHub Container Registry (ghcr.io). This action simplifies the Docker build and push workflow for your CI/CD pipelines.

## Features

- 🐳 Build Docker images from Dockerfile
- 📦 Push to GitHub Container Registry (ghcr.io)
- 🏷️ Automatic image tagging with Git SHA and latest
- 🔐 Secure authentication with GitHub tokens
- 📁 Support for custom working directories
- ✅ Composite action for fast execution

## Usage

```yaml
- name: Build and Push Docker Image
  uses: optivem/build-push-docker-action@v1
  with:
    image-name: my-image-name
    github-token: ${{ secrets.GITHUB_TOKEN }}
    github-actor: ${{ github.actor }}
    github-repository: ${{ github.repository }}
    github-sha: ${{ github.sha }}
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `working-directory` | Working directory where Dockerfile is located | No | `.` |
| `image-name` | Name of the Docker image | Yes | - |
| `github-token` | GitHub token for registry authentication | Yes | - |
| `github-actor` | GitHub actor for registry authentication | Yes | - |
| `github-repository` | GitHub repository for image tagging | Yes | - |
| `github-sha` | GitHub SHA for image tagging | Yes | - |

### Input Details

- **`working-directory`**: The directory containing your Dockerfile. Defaults to the repository root (`.`).
- **`image-name`**: The name you want to give your Docker image (e.g., `my-app`, `api-server`).
- **`github-token`**: GitHub token for authenticating with GitHub Container Registry. Typically `${{ secrets.GITHUB_TOKEN }}`.
- **`github-actor`**: GitHub username for registry authentication. Typically `${{ github.actor }}`.
- **`github-repository`**: Full repository name for image tagging. Typically `${{ github.repository }}`.
- **`github-sha`**: Git commit SHA for image tagging. Typically `${{ github.sha }}`.

## Examples

### Basic Usage

```yaml
name: Build and Deploy

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
      
    - name: Build and Push Docker Image
      uses: optivem/build-push-docker-action@v1
      with:
        image-name: my-awesome-app
        github-token: ${{ secrets.GITHUB_TOKEN }}
        github-actor: ${{ github.actor }}
        github-repository: ${{ github.repository }}
        github-sha: ${{ github.sha }}
```

### With Custom Working Directory

```yaml
- name: Build and Push Docker Image
  uses: optivem/build-push-docker-action@v1
  with:
    working-directory: ./backend
    image-name: my-backend-api
    github-token: ${{ secrets.GITHUB_TOKEN }}
    github-actor: ${{ github.actor }}
    github-repository: ${{ github.repository }}
    github-sha: ${{ github.sha }}
```

### Complete Workflow Example

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Run Tests
      run: |
        # Add your test commands here
        echo "Running tests..."
        
  build-and-push:
    needs: test
    runs-on: ubuntu-latest
    if: github.event_name == 'push'
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
      
    - name: Build and Push Docker Image
      uses: optivem/build-push-docker-action@v1
      with:
        image-name: ${{ github.event.repository.name }}
        github-token: ${{ secrets.GITHUB_TOKEN }}
        github-actor: ${{ github.actor }}
        github-repository: ${{ github.repository }}
        github-sha: ${{ github.sha }}
```

## How It Works

This action performs the following steps:

1. **Echo Information**: Displays the image name being built for debugging
2. **Build Docker Image**: Builds the Docker image using your Dockerfile
3. **Login to GHCR**: Authenticates with GitHub Container Registry
4. **Tag Image**: Tags the image for the registry with both SHA and latest tags
5. **Push Image**: Pushes the tagged image to GitHub Container Registry

## Prerequisites

- A `Dockerfile` in your repository (or in the specified working directory)
- GitHub Container Registry enabled for your repository
- Appropriate repository permissions for the `GITHUB_TOKEN`

## Permissions

Make sure your workflow has the necessary permissions:

```yaml
permissions:
  contents: read
  packages: write
```

## Image Location

Your Docker image will be available at:
```
ghcr.io/{owner}/{repository}/{image-name}:latest
ghcr.io/{owner}/{repository}/{image-name}:{sha}
```

For example: `ghcr.io/optivem/my-app/api-server:latest`

## Troubleshooting

### Authentication Issues
- Ensure `GITHUB_TOKEN` has package write permissions
- Check that GitHub Container Registry is enabled for your repository

### Build Failures
- Verify your Dockerfile is valid and in the correct location
- Check the `working-directory` input if your Dockerfile is not in the repo root

### Permission Denied
- Make sure your workflow has `packages: write` permission
- Verify the repository settings allow GitHub Actions to write packages

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

## Changelog

### v1.0.1
- Added echo step for better debugging output

### v1.0.0
- Initial release
- Basic Docker build and push functionality

## License

MIT License

## Contributors

- [Valentina Jemuović](https://www.linkedin.com/in/valentinajemuovic/)
- [Jelena Cupać](https://www.linkedin.com/in/jelenacupac/)