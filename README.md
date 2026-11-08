# github-workflows-library

Reusable GitHub Actions workflows used by my projects. Third-party actions are pinned to
full commit SHAs and kept current by Dependabot.

## Workflows

### node-ci.yml
Installs dependencies, then builds and tests one npm workspace.

| Input | Default | Description |
|---|---|---|
| node-version | 22 | Node.js version |
| workspace | repo root | npm workspace to build and test, for example apps/api |
| needs-mongo | false | Start a throwaway MongoDB 7 container for the tests |

### docker-build.yml
Builds an image with Buildx (with layer caching) and scans it with Trivy. It never pushes.

| Input | Default | Description |
|---|---|---|
| image-name | required | Local image name |
| dockerfile | required | Path to the Dockerfile |
| context | . | Build context |
| severity | CRITICAL,HIGH | Severities that count as findings |
| fail-on-findings | true | Fail the job when Trivy reports findings |

## Usage

    jobs:
      api:
        uses: Yordiih/github-workflows-library/.github/workflows/node-ci.yml@v1
        with:
          workspace: apps/api
          needs-mongo: true

## Design notes

- Each workflow requests the minimum token permissions (contents: read).
- Inputs reach shell steps through env, never through direct interpolation.
- Third-party actions are pinned by SHA because version tags can be moved.
