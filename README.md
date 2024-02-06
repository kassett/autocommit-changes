# autocommit-changes

A Github Action for automatically committing all files that have changes in a repository.

## Description

This Github Action automates the process of committing changes to a repository. It is useful in scenarios where you want to commit changes automatically without manual intervention, such as during CI/CD workflows.

## Features

- Automatically commits changes to the repository.
- Supports running on push events to specific branches and scheduled events.
- Handles the installation of necessary dependencies and configuration setup.
- Enables seamless integration into existing workflows.

## Usage

To use this action in your workflow, you can include it in your workflow YAML file as follows:

```yaml
name: CI

on:
  push:
    branches:
      - main
      - dev
  schedule:
    - cron: "0 5 * * *"

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      # Add other steps or use autocommit-changes action directly here

      - name: Autocommit changes
        uses: <github_username>/<github_repo_name>@<release_tag>
        with:
          commit_message: "Automated commit by autocommit-changes action"
