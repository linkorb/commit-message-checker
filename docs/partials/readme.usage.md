### Example Workflow

```yml
name: 'Commit Message Check'
on:
  pull_request:
    types:
      - opened
      - edited
      - reopened
      - synchronize
  pull_request_target:
    types:
      - opened
      - edited
      - reopened
      - synchronize

jobs:
  check-commit-message:
    name: Check Commit Message
    runs-on: ubuntu-latest
    steps:
      - name: Check Commit Type
        uses: gsactions/commit-message-checker@v2
        with:
          pattern: '\[[^]]+\] .+$'
          flags: 'gm'
          error: 'Your first line has to contain a commit type like "[BUGFIX]".'
      - name: Check Line Length
        uses: gsactions/commit-message-checker@v2
        with:
          pattern: '^[^#].{74}'
          error: 'The maximum line length of 74 characters is exceeded.'
          excludeDescription: 'true' # optional: this excludes the description body of a pull request
          excludeTitle: 'true' # optional: this excludes the title of a pull request
          checkAllCommitMessages: 'true' # optional: this checks all commits associated with a pull request
          accessToken: ${{ secrets.GITHUB_TOKEN }} # github access token is only required if checkAllCommitMessages is true
      - name: Check for Resolves / Fixes
        uses: gsactions/commit-message-checker@v2
        with:
          pattern: '^.+(Resolves|Fixes): \#[0-9]+$'
          error: 'You need at least one "Resolves|Fixes: #<issue number>" line.'
```

## Configuration

See also [action definition](action.yml) and the following example workflow.

More information about `pattern` and `flags` can be found in the
[JavaScript reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp).

`flags` is optional and defaults to `gm`.

`excludeDescription`, `excludeTitle` and `checkAllCommitMessages` are optional.
Default behavior is to include the description and title and not check pull
request commit messages.
