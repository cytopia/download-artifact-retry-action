# Download artifact retry action

[![GitHub release](https://img.shields.io/github/release/cytopia/download-artifact-retry-action.svg?logo=github)](https://github.com/cytopia/download-artifact-retry-action/releases/latest)
[![GitHub marketplace](https://img.shields.io/badge/marketplace-download--artifact--retry-blue?logo=github)](https://github.com/marketplace/actions/download-artifact-retry)
[![](https://img.shields.io/badge/github-cytopia%2Fdownload--artifact--retry--action-red.svg?logo=github)](https://github.com/cytopia/download-artifact-retry-action "github.com/cytopia/download-artifact-retry-action")
[![test](https://github.com/cytopia/download-artifact-retry-action/actions/workflows/test.yml/badge.svg)](https://github.com/cytopia/download-artifact-retry-action/actions/workflows/test.yml)

This action allows you to download an artifact with retries in case the download has failed.


## :arrow_forward: Inputs

The following inputs can be used to alter the Docker tag name determination:

| Input          | Required | Default | Description                               |
|----------------|----------|---------|-------------------------------------------|
| `name`         | Yes      | ``      | The artifact name.                        |
| `path`         | No       | `.`     | The local path to download to.            |



## :arrow_backward: Outputs

None


## :computer: Usage

### Simple
```yaml
on: [push]

jobs:
  job1:
    runs-on: ubuntu-latest
    name: Pull docker image
    steps:

      - name: Checkout repository
        uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: Create artifact
        id: file
        shell: bash
        run: |
          PRE_RAND="$( ${RAND} | md5sum | head -c 20;  )"
          PRE_DATE="$( date '+%s' )"
          NAME="${PRE_RAND}-${PRE_DATE}.txt"
          echo "somedata" > "${NAME}"
          echo "::set-output name=path::${NAME}"

      - name: upload artifact
        uses: cytopia/upload-artifact-retry-action@v0.1.2
        with:
          name: ${{ steps.file.outputs.path }}
          path: ${{ steps.file.outputs.path }}

      - name: download artifact
        uses: cytopia/download-artifact-retry-action@v0.1.0
        with:
          name: ${{ steps.file.outputs.path }}
```


## :exclamation: Keep up-to-date with GitHub Dependabot

Since [Dependabot](https://docs.github.com/en/github/administering-a-repository/keeping-your-actions-up-to-date-with-github-dependabot) has [native GitHub Actions support](https://docs.github.com/en/github/administering-a-repository/configuration-options-for-dependency-updates#package-ecosystem), to enable it on your GitHub repo all you need to do is add the `.github/dependabot.yml` file:

```yml
version: 2
updates:
  # Maintain dependencies for GitHub Actions
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "daily"
```


## :octocat: [cytopia](https://github.com/cytopia) GitHub Actions

| Name                             | Description |
|----------------------------------|-------------|
| [docker-tag-action]              | Determines Docker tags based on git branch, commit or git tag |
| [git-ref-matrix-action]          | Create stringified JSON list of git refs to be used as a build matrix |
| [shell-command-retry-action]     | Retries shell commands to avoid failing pipelines due to network issues |
| [upload-artifact-verify-action]  | Upload artifact and verifies it by downloading it again |
| [upload-artifact-retry-action]   | Retries `upload-artifact-verify-action` |
| [download-artifact-retry-action] | Download artifact with retry functionality |

[docker-tag-action]: https://github.com/cytopia/docker-tag-action
[git-ref-matrix-action]: https://github.com/cytopia/git-ref-matrix-action
[shell-command-retry-action]: https://github.com/cytopia/shell-command-retry-action
[upload-artifact-verify-action]: https://github.com/cytopia/upload-artifact-verify-action
[upload-artifact-retry-action]: https://github.com/cytopia/upload-artifact-retry-action
[download-artifact-retry-action]: https://github.com/cytopia/download-artifact-retry-action


## :page_facing_up: License

**[MIT License](LICENSE)**

Copyright (c) 2022 [cytopia](https://github.com/cytopia)
