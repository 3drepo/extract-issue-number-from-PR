# Extract Issue number from PR merge commit
A composition action to extract issue number from the branch name on a merge commit.

This assumes branch is named in the format `ISSUE_<issue number>`

so a a merge commit to merge ISSUE_123, `issue-number` will be 123.

## Inputs

### `pr`
pull request ID to identify source issue.

### `base`
optional target branch to identify if merged

### `github-token`
github token to allow access to the API

## Outputs

### `issue-number`

Issue number extracted from the PR

### `issue-content-id`

GraphQL API issue-content-id for the issue extracted from the PR

### `pr-content-id`

GraphQL API pr-content-id for the issue extracted from the PR

### `merged`

If base provided, detect if we have merged the PR.

### Example Workflow
```
on:
  push:
    branches:
      - master

jobs:
  get-issue-number:
    name: Get Current Issue Number
    needs: [ get-pr-number ]
    runs-on: ubuntu-latest
    outputs: 
      issue-number: ${{ steps.commitMsgParser.outputs.issue-number }}
      issue-content-id: ${{ steps.commitMsgParser.outputs.issue-content-id }}
      pr-content-id: ${{ steps.commitMsgParser.outputs.pr-content-id }}
      merged: ${{ steps.commitMsgParser.outputs.merged }}
    steps:
      - name: Extract Issue Number from PR
        uses: 3drepo/extract-pr-information@v1
        with:
          pr: ${{ needs.get-pr-number.outputs.pr-number }}
          base: staging
          github-token: ${{ secrets.GITHUB_TOKEN }}
        id: commitMsgParser

```
