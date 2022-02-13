# reenable-automerge-action

Re-enable disabled GitHub Automerge

## Motivation

Sometimes GitHub Automerge becomes disabled because the base branch was modified.

e.g.

![image](https://user-images.githubusercontent.com/13323303/150432569-0b1f3f01-d09d-4b26-842e-3d0cccb24f33.png)

> auto-merge was automatically disabled 7 hours ago
> Base branch was modified

This is inconvenient especially when you want to merge [Renovate](https://docs.renovatebot.com/) Pull Request automatically with [platformAutomerge](https://docs.renovatebot.com/configuration-options/#platformautomerge).

GitHub Actions supports triggering a workflow when the automerge becomes disabled.
This action updates a pull request branch and enable automerge.
By running this action when the automerge becomes disabled, you can automerge pull requests.

## Requirement

GitHub Personal Access Token or GitHub App token are required.
`secrets.GITHUB_TOKEN` can't be used.

### GitHub Access Token's required permission

* pull-requests: write (update branch, enable automerge)
* contents: write (update branch)

## Example

```yaml
---
name: Reenable automerge
on:
  pull_request:
    branches: [main]
    types:
      - auto_merge_disabled
permissions:
  # "permissions" section should not be empty
  # This action doesn't use secrets.GITHUB_TOKEN
  contents: read
jobs:
  main:
    if: "github.event.reason == 'Base branch was modified'"
    runs-on: ubuntu-latest
    steps:
      - name: Generate token
        id: generate_token
        uses: tibdex/github-app-token@v1
        with:
          app_id: ${{ secrets.APP_ID }}
          private_key: ${{ secrets.APP_PRIVATE_KEY }}

      - uses: suzuki-shunsuke/reenable-automerge-action@main
        with:
          github_token: ${{ steps.generate_token.outputs.token }}
```

## Inputs

### Required Inputs

name | description
--- | ---
github_token | GitHub Access Token. `secrets.GITHUB_TOKEN` can't be used

### Optional Inputs

Nothing.

## Outputs

Nothing.

## LICENSE

[MIT](LICENSE)
