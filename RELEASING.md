# Release Guide

This document describes how to create releases for the Git History Timeline project.

## Semantic Versioning

This project uses [Semantic Versioning](https://semver.org/) **without** the `v` prefix:

- **Major version** (`X.0.0`): Breaking changes
- **Minor version** (`1.X.0`): New features, backward compatible
- **Patch version** (`1.0.X`): Bug fixes, backward compatible

## Creating a Release

### 1. Update Version (Optional)

If you track version in `package.json`, update it:

```bash
npm version 1.0.0 --no-git-tag-version
git add package.json package-lock.json
git commit -m "Bump version to 1.0.0"
```

### 2. Create and Push Tag

```bash
# Create the tag (without 'v' prefix)
git tag 1.0.0

# Push the tag to trigger deployment
git push origin 1.0.0
```

### 3. Monitor Deployment

1. Go to the **Actions** tab in your GitHub repository
2. Watch the "Build and Deploy to GitHub Pages" workflow
3. Once complete, your timeline will be live at: `https://<username>.github.io/git-history-timeline/`

## Pre-release Versions

For beta, release candidate, or alpha versions:

```bash
# Beta release
git tag 2.0.0-beta.1
git push origin 2.0.0-beta.1

# Release candidate
git tag 2.0.0-rc.1
git push origin 2.0.0-rc.1

# Alpha release
git tag 2.0.0-alpha.1
git push origin 2.0.0-alpha.1
```

## Valid Tag Examples

✅ **Valid** (will trigger deployment):
- `1.0.0`
- `2.1.3`
- `10.5.2`
- `1.0.0-beta.1`
- `2.0.0-rc.2`
- `3.1.0-alpha.3`

❌ **Invalid** (will NOT trigger deployment):
- `v1.0.0` (has `v` prefix)
- `v2.1.3` (has `v` prefix)
- `release-1.0.0` (wrong format)
- `1.0` (missing patch version)
- `latest` (not semver)

## Deleting a Tag

If you made a mistake:

```bash
# Delete local tag
git tag -d 1.0.0

# Delete remote tag
git push origin :refs/tags/1.0.0
```

## Listing Tags

```bash
# List all tags
git tag

# List tags matching pattern
git tag -l "1.*"

# Show tag with commit message
git show 1.0.0
```

## GitHub Release (Optional)

After pushing a tag, you can create a GitHub Release for better visibility:

1. Go to **Releases** in your repository
2. Click **"Draft a new release"**
3. Select your tag (e.g., `1.0.0`)
4. Add release notes
5. Click **"Publish release"**

This doesn't affect the deployment (which happens automatically on tag push), but provides a nice changelog for users.

## Automation Tips

### Automated Version Bumping

You can create npm scripts in `package.json`:

```json
{
  "scripts": {
    "release:patch": "npm version patch --no-git-tag-version && git add . && git commit -m 'Bump patch version' && npm run release:tag",
    "release:minor": "npm version minor --no-git-tag-version && git add . && git commit -m 'Bump minor version' && npm run release:tag",
    "release:major": "npm version major --no-git-tag-version && git add . && git commit -m 'Bump major version' && npm run release:tag",
    "release:tag": "git tag $(node -p \"require('./package.json').version\") && git push && git push --tags"
  }
}
```

Then simply run:

```bash
npm run release:patch  # 1.0.0 -> 1.0.1
npm run release:minor  # 1.0.1 -> 1.1.0
npm run release:major  # 1.1.0 -> 2.0.0
```

## Troubleshooting

### Tag pushed but workflow didn't run

1. Check the tag format matches the pattern (no `v` prefix)
2. Verify GitHub Actions is enabled in repository settings
3. Check the Actions tab for any errors

### Want to re-deploy same version

Delete and recreate the tag:

```bash
git tag -d 1.0.0
git push origin :refs/tags/1.0.0
git tag 1.0.0
git push origin 1.0.0
```

### Manual deployment without tag

Use the workflow dispatch feature:

1. Go to **Actions** tab
2. Select "Build and Deploy to GitHub Pages"
3. Click **"Run workflow"**
4. Select branch and click **"Run workflow"**
