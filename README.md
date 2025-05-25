# GitHub Action: Auto Tag on Package Version Change

This GitHub Action automates the creation of Git tags when the `version` field in a `package.json` file is updated.

## Key Features

- Automatically creates tags based on the `package.json` version.
- Allows branch-specific suffixes for tags (e.g., `-alpha`, `-beta`, `-rc`).
- Customizable Git user for tagging.
- Easy to integrate into existing GitHub workflows.

## Inputs

### `git-user-name`

- **Description:** Git user.name.
- **Default:** `github-actions[bot]`
- **Required:** No

### `git-user-email`

- **Description:** Git user.email.
- **Default:** `github-actions[bot]@users.noreply.github.com`
- **Required:** No

### `branch-suffix-map`

- **Description:** A JSON string mapping branch names to tag suffixes. e.g., `'{"main": "-rc", "develop": "-dev"}'`. Must be valid JSON.
- **Default:** `{}`
- **Required:** No

## Example Usage

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

## Author

This project is maintained by the repository owners. You can specify an author in your `package.json` or update this section directly.

## License

This project is licensed under the ISC License. See the `LICENSE` file for more details (if one exists, or add a generic ISC license text if appropriate).
