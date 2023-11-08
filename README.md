# Chose Action Runner

This action checks if some self-hosted runners are online. If they are offline, it falls back to, e.g. GithHub hosted runners. 
Self-hosted runners are selected based on their name (not label!), for compatibility with runner scale sets.
We add the possibility to check from runners from the repository (default), or the organization through the `runners-holder` parameter.

The GitHub API token should have the following permission:
  - read access to organization self-hosted runners
  - read access to repository administration and metadata

## Usage

```yaml
jobs:

  choose-runner:
    runs-on: ubuntu-latest
    outputs:
      runner: ${{ steps.choose-runner.outputs.runner }}
    steps:
      - id: choose-runner
        uses: QCDIS/chose-action-runner@v1
        with:
          preferred-runner: my-selfhosted-runner
          fallback-runner: ubuntu-latest
          github-token: ${{ secrets.REPO_ACCESS_TOKEN }}

  do-something:
    needs: [choose-runner]
    runs-on: ${{ needs.choose-runner.outputs.runner }}
    steps:
      - name: Do something
        run: |
          echo "Running on ${{ needs.choose-runner.outputs.runner }}"
```

## Acknowledgements

https://github.com/marketplace/actions/check-runner-availability
https://github.com/orgs/community/discussions/20019#discussioncomment-7173595
