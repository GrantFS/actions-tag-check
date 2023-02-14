# GitHub Action - Tag Check

[![Lint](https://github.com/GrantFS/actions-tag-check/actions/workflows/lint.yaml/badge.svg)](https://github.com/GrantFS/actions-tag-check/actions/workflows/lint.yaml)

Checks if the current NPM version has had a tagged release

## Usage

```yaml

name: Release

on:
  pull_request:
    branches:
      - main


permissions:
  id-token: write
  contents: write
  pull-requests: write
  actions: read

jobs:
  tag-check:
    name: Main Version
    runs-on: ubuntu-latest
    outputs:
      version: ${{ steps.check-tag.outputs.result }}
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3
        with:
          ref: main
          token: ${{ secrets.GITHUB_TOKEN }}
          fetch-depth: 0

      - name: Check tag
        id: check-tag
        uses: GrantFS/actions-tag-check@0.0.2

      - name: Is this tagged
        run: echo ${{ steps.check-tag.outputs.tagged }}

      - name: Already has this tag
        if: steps.tagged.outputs.tagged == 0
        run: echo Tagged already

```

## Outputs

| Output           | Description                  |
|------------------------------|-----------------------------------------------|
| tagged           | Is there a tag for this version  |
