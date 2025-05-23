```yaml

# .github/workflows/release-tag.yml

name: Create Version Tag on Package Update

on:
  push:
    branches:
      - main
      - "dev"
      - "qa"
      - "prod"
    paths:
      - "package.json"

jobs:
  tag-version:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0 # Important for git fetch --tags and git rev-parse to work correctly

      - name: Set up Node.js (for jq, if not pre-installed, though usually it is)
        uses: actions/setup-node@v4
        with:
          node-version: "18"

      - name: Tag on Package Version Change
        uses: ./.github/actions/tag-on-package-version-change
        with:
          # Provide valid JSON here
          branch-suffix-map: |
            {
              "main": "",
              "dev": "-alfa",
              "qa": "-beta",
              "prod": "-rc"
            }
```
