# GitHub Action: Auto Tag on Package Version Change

[![License: ISC](https://img.shields.io/badge/License-ISC-blue.svg)](https://opensource.org/licenses/ISC)
[![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=flat&logo=githubactions&logoColor=white)](https://github.com/features/actions)

Automatically create Git tags when the `version` field in your `package.json` file changes. Perfect for maintaining version control and release management in your Node.js projects.

## 📋 Overview

This GitHub Action streamlines your release process by:

- Monitoring changes to `package.json` version
- Creating Git tags automatically
- Supporting different environments with branch-specific tag suffixes
- Maintaining clean version control

## ⚙️ Features

- **Automated Tagging**: Creates tags based on `package.json` version changes
- **Environment Support**: Configure different tag suffixes for various branches
- **Customizable**: Flexible Git user configuration
- **Lightweight**: Minimal setup required
- **Safe**: Prevents duplicate tags and validates version format

## 🚀 Quick Start

1. Create `.github/workflows/release-tag.yml`:

````markdown
```yaml
name: Create Version Tag
on:
  push:
    branches: [main, dev, qa, prod]
    paths: ["package.json"]

jobs:
  tag-version:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - uses: ./.github/actions/tag-on-package-version-change
        with:
          branch-suffix-map: |
            {
              "main": "",
              "dev": "-alpha",
              "qa": "-beta",
              "prod": "-rc"
            }
```
````
