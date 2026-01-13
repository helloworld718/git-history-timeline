# GitHub Actions Workflow

This directory contains the GitHub Actions workflow for automatically building and deploying the Git History Timeline to GitHub Pages.

## Workflow: `deploy.yml`

### What it does

1. **Build Job**:
   - Checks out the repository
   - Sets up Node.js 18
   - Installs dependencies
   - Runs the timeline generator (`node src/index.js`)
   - Uploads the generated `dist/index.html` as an artifact

2. **Deploy Job**:
   - Takes the artifact from the build job
   - Deploys it to GitHub Pages

### Triggers

- **Automatic**: Runs when you push a release tag with semantic versioning (without `v` prefix)
  - Examples: `1.0.0`, `2.1.3`, `1.0.0-beta.1`, `3.2.1-rc.2`
- **Manual**: Can be triggered manually from the Actions tab

#### Creating a Release

To trigger a deployment:

```bash
# Tag your release (without 'v' prefix)
git tag 1.0.0
git push origin 1.0.0

# Or with pre-release identifier
git tag 2.0.0-beta.1
git push origin 2.0.0-beta.1
```

### Required Setup

#### 1. Enable GitHub Pages

Go to your repository Settings → Pages and select **GitHub Actions** as the source.

#### 2. Add Repository Secret

The workflow needs a GitHub token to fetch your git history:

1. Generate a token at: https://github.com/settings/tokens/new
   - Select scopes: `repo` and `read:user`
2. Go to your repository Settings → Secrets and variables → Actions
3. Click "New repository secret"
4. Name: `GH_TIMELINE_TOKEN`
5. Value: Paste your token

### Tokens Explained

The workflow uses **two different tokens**:

| Token | Purpose | Setup Required |
|-------|---------|----------------|
| `GITHUB_TOKEN` | Deploy to GitHub Pages | ❌ No - automatically provided |
| `GH_TIMELINE_TOKEN` | Fetch git history via API | ✅ Yes - add as repository secret |

The built-in `GITHUB_TOKEN` has the necessary permissions to deploy to GitHub Pages, so you don't need to create a separate token for that.

### Permissions

The workflow requests these permissions for the `GITHUB_TOKEN`:

```yaml
permissions:
  contents: read      # Read repository contents
  pages: write        # Deploy to GitHub Pages
  id-token: write     # Required for Pages deployment
```

### Concurrency

The workflow uses concurrency control to prevent multiple deployments from running simultaneously:

```yaml
concurrency:
  group: "pages"
  cancel-in-progress: false
```

This ensures that if multiple commits are pushed quickly, they won't interfere with each other's deployments.

### Customization

If you want to customize the workflow:

- **Change tag pattern**: Modify the `tags` list under `on.push`
- **Use different Node version**: Change `node-version` in the setup step
- **Add build options**: Pass arguments to `node src/index.js` (e.g., `--repos owned`)
- **Schedule regular updates**: Add a `schedule` trigger with cron syntax

Example with scheduled updates (daily at midnight):

```yaml
on:
  push:
    tags:
      - '[0-9]+.[0-9]+.[0-9]+'
      - '[0-9]+.[0-9]+.[0-9]+-*'
  schedule:
    - cron: '0 0 * * *'  # Daily at midnight UTC
  workflow_dispatch:
```

Example to also run on main branch pushes:

```yaml
on:
  push:
    branches:
      - main
    tags:
      - '[0-9]+.[0-9]+.[0-9]+'
      - '[0-9]+.[0-9]+.[0-9]+-*'
  workflow_dispatch:
```

### Troubleshooting

#### Workflow fails with "Bad credentials"

Your `GH_TIMELINE_TOKEN` secret is missing or invalid. Make sure you've added it correctly in repository settings.

#### Workflow succeeds but page doesn't update

1. Check that GitHub Pages is enabled in Settings → Pages
2. Verify the source is set to "GitHub Actions"
3. Wait a few minutes - Pages deployment can take time

#### Want to deploy a different user's timeline

You can modify the workflow to pass the `--user` flag:

```yaml
- name: Generate timeline
  env:
    GITHUB_TOKEN: ${{ secrets.GH_TIMELINE_TOKEN }}
  run: |
    node src/index.js --user octocat
```

Note: Your token must have access to that user's repositories.
