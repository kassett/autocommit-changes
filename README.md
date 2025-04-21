# Autocommit GitHub Changes

**Author:** kassett  
**Description:** This GitHub Action allows you to automatically commit changes to a GitHub repository from within a GitHub Actions workflow. Supports direct commits or pull request creation.

---

## 🔧 Inputs

| Name                       | Required | Default                           | Description                                                                    |
|----------------------------|----------|-----------------------------------|--------------------------------------------------------------------------------|
| `branch`                   | ✅        | -                                 | The branch to commit to or base branch for PR.                                 |
| `github-token`             | ✅        | -                                 | GitHub token used by `gh` CLI.                                                 |
| `use-pull-request`         | ❌        | `"false"`                         | If `true`, creates a pull request instead of committing directly.              |
| `files`                    | ❌        | `""`                              | Comma-separated list of files to commit. Mutually exclusive with `commit-all`. |
| `commit-all`               | ❌        | `"false"`                         | If `true`, commits all tracked changes. Mutually exclusive with `files`.       |
| `commit-message`           | ❌        | `"Auto-committed changes"`        | The commit message.                                                            |
| `pull-request-title`       | ❌        | `"Auto-generated PR"`             | Title for the pull request.                                                    |
| `pull-request-body`        | ❌        | `"PR created by GitHub Actions."` | Body content of the pull request.                                              |
| `pull-request-labels`      | ❌        | `""`                              | Labels to attach to the PR. The labels must already exist for the repository.  |
| `pull-request-branch-name` | ❌        | `""`                              | Custom name for the PR branch. Random if not set.                              |
| `commit-untracked-files`   | ❌        | `"false"`                         | If `true`, includes untracked files.                                           |

---

## 📤 Outputs

| Name        | Description                                   |
|-------------|-----------------------------------------------|
| `sha`       | The SHA of the commit created.                |
| `branch`    | The name of the branch used for the PR.       |
| `pr-number` | The number of the pull request created.       |

---

## ▶️ Usage

```yaml
jobs:
  commit-changes:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Make changes
        run: echo "Hello world" > hello.txt

      - name: Commit changes
        uses: kassett/autocommit-action@v2
        with:
          branch: main
          github-token: ${{ secrets.GITHUB_TOKEN }}
          commit-all: "true"
          commit-message: "Automated commit from workflow"
```