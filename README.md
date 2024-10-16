# Manage Dependabot PRs Workflow

> This repository contains a reusable GitHub Actions workflow to manage pull requests generated by Dependabot. The workflow is designed to selectively close or enable auto-merge for Dependabot PRs based on specific criteria. 

## Workflow Overview

The workflow named `Manage Dependabot PRs` handles the following tasks:

- **Close PRs** that are not grouped version/security updates.
- **Enable auto-merge** for PRs that are marked as grouped security updates or version updates.

## Usage

To use this workflow in your repository, you can reference it in your own GitHub Actions workflow file. Here's an example:

```yaml
jobs:
  dependabot:
    if: github.actor == 'dependabot[bot]'
    uses: bolah2009/reusable-gha-workflow/.github/workflows/dependabot.yml
    with:
      PR_URL: ${{ github.event.pull_request.html_url }}
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Inputs

The following inputs are required for this workflow:

- `PR_URL` (string, required): The URL of the pull request to be managed. This should be passed when calling the workflow. Defaults to the pull request URL from the GitHub event context if not provided.

### Secrets

The workflow requires the following secrets:

- `GH_TOKEN` (required): GitHub token for authentication, allowing the workflow to manage pull requests.

### Permissions

The workflow requires permissions to:
- **write** to pull requests, issues, and repository projects, which enables it to close PRs, set auto-merge, and make comments on PRs.

## Workflow Logic

1. **Fetch Dependabot Metadata**: Retrieves metadata for the Dependabot pull request to determine the type of update.
2. **Close Non-Group PRs**: If the PR is not part of the "security-updates" or "version-updates" groups, it will be closed with a comment.
3. **Enable Auto-Merge for Selected PRs**: If the PR is classified under the "security-updates" or "version-updates" groups, it will be configured to auto-merge.

## Example Workflow Configuration

In your repository, create a new workflow file (e.g., `.github/workflows/dependabot-management.yml`) and reference this reusable workflow as follows:

```yaml
name: Manage Dependabot PRs

on:
  pull_request:
    types: [opened, synchronize]
    branches:
      - main

jobs:
  dependabot:
    if: github.actor == 'dependabot[bot]'
    uses: bolah2009/reusable-gha-workflow/.github/workflows/dependabot.yml
    with:
      PR_URL: ${{ github.event.pull_request.html_url }}
    secrets:
      GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

This example configuration ensures that the `dependabot` job will only run for PRs opened by Dependabot, managing the PR according to the workflow's logic.


## 🤝🏾 Contributing

Contributions, issues and feature requests are welcome!

Feel free to check the [issues page](../../issues).

## ⭐️ Show your support

Give a ⭐️ if you like this project!

## 👨🏽‍💻 Author

- [@bolah2009](https://github.com/bolah2009/)

## 📝 License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for more details.
