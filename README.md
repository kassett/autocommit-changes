# Autocommit GitHub Changes

This GitHub Action allows you to commit changes to a Git repository within a GitHub Actions runner. You can commit directly to a branch, create a pull request, or even automate merging and cleanup.

<b><i>This action is managed by a private repository.</i></b>

## What's new in V2
Version v2 added supported for the following:
* Specifying a specific branch name when using the pull request workflow
* Assigning pull requests to teams or users for review after the pull request has been created

## Inputs

| Input                         | Description                                                                                                           | Default                                      | Required |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------------|----------------------------------------------|----------|
| `branch`                      | The branch to be committed to, or the base branch if `use-pull-request` is set to `true`.                             | N/A                                          | true     |
| `github-token`                | The token that the GH cli tool will use.                                                                              | N/A                                          | true     |
| `use-pull-request`            | If `true`, changes will be committed to a separate branch and a PR will be opened to merge changes.                   | `false`                                      | false    |
| `merge-pull-request`          | Automatically merge the pull request into the base branch after creation.                                             | `false`                                      | false    |
| `files`                       | A list of files to be committed. If left empty, changes will be dynamically detected.                                 | `""`                                         | false    |
| `commit-untracked-files`      | Whether to include untracked (new) files in the commit.                                                               | `false`                                      | false    |
| `commit-message`              | The message to use for the commit.                                                                                    | `Auto-committed files [skip ci]`             | false    |
| `pull-request-title`          | The title of the pull request created by this action.                                                                 | `Auto-generated PR`                          | false    |
| `pull-request-body`           | The body of the pull request created by this action.                                                                  | `Pull request opened by Github Actions Bot.` | false    |
| `delete-branch-after-merge`   | If `true`, the branch used for the pull request will be deleted after the merge.                                      | `false`                                      | false    |
| `pull-request-branch-name`    | The name of the feature branch created when a PR is opened. Default to a branch name generated from the timestamp.    | `""`                                         | false    |
| `pull-request-user-reviewers` | A comma-separated list of users to be assigned for review when a pull request is opened but not merged automatically. | `""`                                         | false    |
| `pull-request-team-reviewers` | A comma-separated list of teams to be assigned for review when a pull request is opened but not merged automatically. | `""`                                         | false    |

## Outputs

| Output  | Description                                                     |
|---------|-----------------------------------------------------------------|
| `sha`   | The SHA of the commit made. Will be `""` if no commit was made. |

## Usage

You can easily integrate this action into your workflow by adding it to your GitHub Actions configuration. Here’s an example:

```yaml
name: Autocommit Changes

on: [push]

jobs:
  autocommit:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
        
      - name: Autocommit Changes
        uses: kassett/autocommit-changes@v1
        with:
          branch: main
          use-pull-request: true
          merge-pull-request: false
          commit-message: "Committing changes automatically"
          pull-request-title: "Auto-commit PR"
          pull-request-body: "This PR was generated automatically by GitHub Actions."
          delete-branch-after-merge: true
```

One important thing to note is that this action should never run on a branch that has a detached HEAD; therefore, 
when using this action in a workflow invoked on `pull_request`, the workflow should checkout to the feature-branch.
Below is an example:

```yaml
name: Autocommit Changes

on: [pull_request]

jobs:
  autocommit:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
        with:
          ref: ${{ github.head_ref }}
        
      - name: Autocommit Changes
        uses: kassett/autocommit-changes@v1
        with:
          branch: ${{ github.head_ref }}
```