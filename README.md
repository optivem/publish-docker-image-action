# Build and Push Docker Image Action

GitHub Action to build and push Docker images to GitHub Container Registry.

## Usage

```yaml
- name: Build and Push Docker Image
  uses: your-username/build-push-docker-action@v1
  with:
    image-name: my-app
    github-token: ${{ secrets.GITHUB_TOKEN }}
    github-actor: ${{ github.actor }}
    github-repository: ${{ github.repository }}
    github-sha: ${{ github.sha }}

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

### Example Usage

```yaml
- name: Build and Push Docker Image
  uses: ./.github/actions/build-push-docker
  with:
    working-directory: ./src
    image-name: my-awesome-app
    github-token: ${{ secrets.GITHUB_TOKEN }}
    github-actor: ${{ github.actor }}
    github-repository: ${{ github.repository }}
    github-sha: ${{ github.sha }}