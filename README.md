# Publish Docker Image Action

[![CI](https://github.com/optivem/publish-docker-image-action/workflows/CI/badge.svg)](https://github.com/optivem/publish-docker-image-action/actions)
[![release](https://github.com/optivem/publish-docker-image-action/actions/workflows/release.yml/badge.svg)](https://github.com/optivem/publish-docker-image-action/actions/workflows/release.yml)

[![GitHub release](https://img.shields.io/github/release/optivem/publish-docker-image-action.svg)](https://github.com/optivem/publish-docker-image-action/releases)
[![GitHub marketplace](https://img.shields.io/badge/marketplace-publish--docker--image--action-blue?logo=github)](https://github.com/marketplace/actions/publish-docker-image-action)

A GitHub Action that builds Docker images and pushes them to GitHub Container Registry (ghcr.io). This action simplifies the Docker build and push workflow for your CI/CD pipelines.

## Features

- 🐳 Build Docker images from Dockerfile
- 📦 Push to GitHub Container Registry (ghcr.io)
- 🏷️ Automatic image tagging with Git SHA and latest
- 🔐 Secure authentication with GitHub tokens
- 📁 Support for custom working directories
- ✅ Composite action for fast execution

## Usage

### Basic Usage (GitHub Container Registry)

```yaml
- name: Build and Push Docker Image
  uses: optivem/publish-docker-image-action@v1
  with:
    image-name: my-image-name
  env:
    REGISTRY_PASSWORD: ${{ secrets.GITHUB_TOKEN }}
```

### Complete Example

```yaml
- name: Build and Push Docker Image
  uses: optivem/publish-docker-image-action@v1
  with:
    image-name: my-awesome-app
    image-latest-tag: "v1.0.0"
  env:
    REGISTRY_PASSWORD: ${{ secrets.GITHUB_TOKEN }}
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `working-directory` | Working directory where Dockerfile is located | No | `.` |
| `image-name` | Name of the Docker image (without registry prefix) | Yes | - |
| `registry` | Container registry URL | No | `ghcr.io` |
| `registry-username` | Username for registry authentication | No | `${{ github.actor }}` |
| `image-namespace` | Namespace/organization for the image | No | `${{ github.repository }}` |
| `commit-sha` | Git commit SHA for image tagging | No | `${{ github.sha }}` |
| `image-latest-tag` | Tag to apply for the latest image (single value, e.g., "latest") | No | `latest` |
| `dockerfile` | Path to Dockerfile relative to working directory | No | `Dockerfile` |

**Registry Authentication Environment Variable:**
- `REGISTRY_PASSWORD` - Password/token for registry authentication (Required)

## Outputs

| Name | Description |
|------|-------------|
| `image-latest-url` | Full URL of the pushed Docker image with latest tag |
| `image-digest-url` | Full URL with SHA256 digest of the pushed image |
| `image-created-timestamp` | Timestamp when the Docker image was created |

### Input Details

- **`working-directory`**: The directory containing your Dockerfile. Defaults to the repository root (`.`).
- **`image-name`**: The name you want to give your Docker image (e.g., `my-app`, `api-server`).
- **`registry`**: Container registry URL (defaults to `ghcr.io` for GitHub Container Registry).
- **`registry-username`**: Username for registry authentication (defaults to `${{ github.actor }}`).
- **`image-namespace`**: Namespace/organization for the image (defaults to `${{ github.repository }}`).
- **`commit-sha`**: Git commit SHA for image tagging (defaults to `${{ github.sha }}`).
- **`REGISTRY_PASSWORD`**: Environment variable containing the password/token for registry authentication.

## Examples

### GitHub Container Registry (Default)

```yaml
name: Build and Deploy

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
      
    - name: Build and Push to GHCR
      uses: optivem/publish-docker-image-action@v1
      with:
        image-name: my-awesome-app
        image-latest-tag: "${{ github.ref_name }}"
      env:
        REGISTRY_PASSWORD: ${{ secrets.GITHUB_TOKEN }}
```

### Docker Hub

```yaml
- name: Build and Push to Docker Hub
  uses: optivem/publish-docker-image-action@v1
  with:
    image-name: my-awesome-app
    registry: docker.io
    registry-username: ${{ secrets.DOCKERHUB_USERNAME }}
    image-namespace: myorganization
    image-latest-tag: "v1.0.0"
  env:
    REGISTRY_PASSWORD: ${{ secrets.DOCKERHUB_TOKEN }}
```

### Google Container Registry

```yaml
- name: Build and Push to GCR
  uses: optivem/publish-docker-image-action@v1
  with:
    image-name: my-awesome-app
    registry: gcr.io
    registry-username: _json_key
    image-namespace: my-gcp-project
  env:
    REGISTRY_PASSWORD: ${{ secrets.GCR_JSON_KEY }}
```

### Azure Container Registry

```yaml
- name: Build and Push to ACR
  uses: optivem/publish-docker-image-action@v1
  with:
    image-name: my-awesome-app
    registry: myregistry.azurecr.io
    registry-username: ${{ secrets.ACR_USERNAME }}
    image-namespace: myproject
  env:
    REGISTRY_PASSWORD: ${{ secrets.ACR_PASSWORD }}
```

### Amazon ECR

```yaml
- name: Configure AWS credentials
  uses: aws-actions/configure-aws-credentials@v4
  with:
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: us-east-1

- name: Login to Amazon ECR
  id: login-ecr
  uses: aws-actions/amazon-ecr-login@v2

- name: Build and Push to ECR
  uses: optivem/publish-docker-image-action@v1
  with:
    image-name: my-awesome-app
    registry: ${{ steps.login-ecr.outputs.registry }}
    registry-username: AWS
    image-namespace: my-repo
  env:
    REGISTRY_PASSWORD: ${{ steps.login-ecr.outputs.docker_password_stdout }}
```

### Custom Dockerfile and Multiple Tags

```yaml
- name: Build and Push with Custom Settings
  uses: optivem/publish-docker-image-action@v1
  with:
    working-directory: ./backend
    dockerfile: Dockerfile.prod
    image-name: my-backend-api
    image-latest-tag: "stable"
  env:
    REGISTRY_PASSWORD: ${{ secrets.GITHUB_TOKEN }}
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
