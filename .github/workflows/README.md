# GitHub Actions Workflows

This directory contains the CI/CD workflows for automatically releasing `@patternfly/context-for-ai` to NPM.

## Workflows

### build-lint-test.yml
Runs on pull requests and pushes to main. This workflow:
- Type checks the code
- Runs ESLint
- Runs Jest tests
- Builds the distribution files

### release.yml
Runs on pushes to the `main` branch. This workflow:
- Calls the build-lint-test workflow first
- Uses semantic-release to automatically version and publish to NPM
- Creates GitHub releases with changelogs

## Required Secrets

You need to set up the following secrets in your GitHub repository:

### NPM_TOKEN
1. Log in to npm and go to https://www.npmjs.com/settings/YOUR_USERNAME/tokens
2. Click "Generate New Token" → "Classic Token"
3. Select "Automation" type
4. Copy the token
5. In your GitHub repo, go to Settings → Secrets and variables → Actions
6. Click "New repository secret"
7. Name: `NPM_TOKEN`
8. Value: Your npm token
9. Click "Add secret"

### GITHUB_TOKEN
This is automatically provided by GitHub Actions, no setup needed.

## Semantic Release

The release process uses [semantic-release](https://semantic-release.gitbook.io/) which automates version management and package publishing based on commit messages.

### Commit Message Format

Use conventional commits to trigger releases:

- `fix: ...` → Patch release (1.0.0 → 1.0.1)
- `feat: ...` → Minor release (1.0.0 → 1.1.0)
- `feat!: ...` or `BREAKING CHANGE:` in body → Major release (1.0.0 → 2.0.0)

Examples:
```
fix: resolve button styling issue
feat: add new Card component
feat!: rename all component props (breaking change)
```

### First Release

For your first release, you may want to run:
```bash
npm version 1.0.0
git add package.json
git commit -m "chore: initial release setup"
git push
```

Or let semantic-release handle it automatically based on commits.

## Testing Locally

Before pushing to main, you can test the workflow:

```bash
# Run all checks locally
npm run type-check
npm run lint
npm test
npm run build
```

## Troubleshooting

### Release not triggering
- Ensure your commit messages follow the conventional commit format
- Check the Actions tab in GitHub for error messages
- Verify NPM_TOKEN is set correctly

### NPM publish fails
- Verify the package name `@patternfly/context-for-ai` is available/you have access
- Ensure NPM_TOKEN has publish permissions
- Check if you need to be added to the @patternfly organization on npm

