
# GitHub Action

This GitHub Action checks if a pull request (PR) contains the required labels for deployment. It retrieves the labels of a PR and returns an array of environments to deploy based on those labels.

## Description

The `Check PR Labels` action fetches the labels of a given PR from the GitHub API and determines the environments to deploy to based on specific labels. If the labels include `avalon` or `zircon`, the action returns environments prefixed with `production-`. If no such labels are found, it returns a default value of `["none"]`.

## Input Parameters

This action requires two input parameters:

- **`pr_number`**: The number of the pull request to check. This is required.
- **`github_token`**: A GitHub token with appropriate permissions to access the PR labels. This is required.

## Output

The action provides one output:

- **`environments`**: A JSON string array containing the environments to deploy to, based on the PR labels. If no relevant labels are found, the default value `["none"]` is returned.

## Usage

You can use this action in your workflows to check for the required labels before triggering a deployment process.

### Example Usage

Here’s an example of how to use this action in a workflow file:

```yaml
name: Check PR Labels

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  check-pr-labels:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Check PR Labels
        id: check_labels
        uses: your-org/your-central-repo/.github/actions/check-labels-action@main
        with:
          pr_number: ${{ github.event.pull_request.number }}
          github_token: ${{ secrets.GITHUB_TOKEN }}

      - name: Display Environments
        run: echo "Environments to deploy: ${{ steps.check_labels.outputs.environments }}"
```
