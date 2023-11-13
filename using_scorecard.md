---
title: Using Scorecard
layout: default
nav_order: 2
---

## Using Scorecard

### Scorecard GitHub Action

The easiest way to use Scorecard on GitHub projects you own is with the
[Scorecard GitHub Action](https://github.com/ossf/scorecard-action). The Action
runs on any repository change and issues alerts that maintainers can view in the
repository’s Security tab. For more information, see the Scorecard GitHub
Action
[installation instructions](https://github.com/ossf/scorecard-action#installation).

### Scorecard REST API

To query pre-calculated scores of OSS projects, use the [REST API](https://api.securityscorecards.dev).

To enable your project to be available on the REST API, set
[`publish_results: true`](https://github.com/ossf/scorecard-action/blob/dd5015aaf9688596b0e6d11e7f24fff566aa366b/action.yaml#L35)
in the Scorecard GitHub Action setting.

Data provided by the REST API is licensed under the [CDLA Permissive 2.0](https://cdla.dev/permissive-2-0).

### Scorecard Badges

Enabling [`publish_results: true`](https://github.com/ossf/scorecard-action/blob/dd5015aaf9688596b0e6d11e7f24fff566aa366b/action.yaml#L35)
in Scorecard GitHub Actions also allows maintainers to display a Scorecard badge on their repository to show off their
hard work. This badge also auto-updates for every change made to the repository. See more details on [this OSSF blogpost](https://openssf.org/blog/2022/09/08/show-off-your-security-score-announcing-scorecards-badges/).

To include a badge on your project's repository, simply add the following markdown to your README:

```
[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/{owner}/{repo}/badge)](https://securityscorecards.dev/viewer/?uri=github.com/{owner}/{repo})
```

### Scorecard Command Line Interface

To run a Scorecard scan on projects you do not own, use the command line
interface installation option.

#### Prerequisites

Platforms: Currently, Scorecard supports OSX and Linux platforms. If you are
using a Windows OS you may experience issues. Contributions towards supporting
Windows are welcome.

Language: You must have GoLang installed to run Scorecard
(https://golang.org/doc/install)

#### Installation

##### Docker

`scorecard` is available as a Docker container:

```shell
docker pull gcr.io/openssf/scorecard:stable
```

To use a specific scorecard version (e.g., v3.2.1), run:

```shell
docker pull gcr.io/openssf/scorecard:v3.2.1
```

##### Standalone

To install Scorecard as a standalone:

Visit our latest [release page](https://github.com/ossf/scorecard/releases/latest) and
download the correct zip file for your operating system.

Add the binary to your `GOPATH/bin` directory (use `go env GOPATH` to identify your directory if necessary).

###### Verifying SLSA provenance for downloaded releases

We generate [SLSA3 signatures](https://slsa.dev) using the OpenSSF's [slsa-framework/slsa-github-generator](https://github.com/slsa-framework/slsa-github-generator) during the release process. To verify a release binary:
1. Install the verification tool from [slsa-framework/slsa-verifier#installation](https://github.com/slsa-framework/slsa-verifier#installation).
2. Download the signature file `attestation.intoto.jsonl` from the [GitHub releases page](https://github.com/GoogleContainerTools/jib/releases/latest).
3. Run the verifier:

```shell
slsa-verifier -artifact-path <the-zip> -provenance attestation.intoto.jsonl -source github.com/ossf/scorecard -tag <the-tag>
```

##### Using package managers

Package Manager                                            | Supported Distribution | Command
---------------------------------------------------------- | ---------------------- | -------
Nix                                                        | NixOS                  | `nix-shell -p nixpkgs.scorecard`
[AUR helper](https://wiki.archlinux.org/title/AUR_helpers) | Arch Linux             | Use your AUR helper to install `scorecard`
[Homebrew](https://brew.sh/)                               | macOS or Linux         | `brew install scorecard`

#### Authentication

GitHub imposes [api rate limits](https://developer.github.com/v3/#rate-limiting)
on unauthenticated requests. To avoid these limits, you must authenticate your
requests before running Scorecard. There are two ways to authenticate your
requests: either create a GitHub personal access token, or create a GitHub App
Installation.

-   [Create a classic GitHub personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#creating-a-personal-access-token-classic).
    When creating the personal access token, we suggest you choose the
    `public_repo` scope. Set the token in an environment variable called
    `GITHUB_AUTH_TOKEN`, `GITHUB_TOKEN`, `GH_AUTH_TOKEN` or `GH_TOKEN` using the
    commands below according to your platform.

```shell
# For posix platforms, e.g. linux, mac:
export GITHUB_AUTH_TOKEN=<your access token>
# Multiple tokens can be provided separated by comma to be utilized
# in a round robin fashion.
export GITHUB_AUTH_TOKEN=<your access token1>,<your access token2>

# For windows:
set GITHUB_AUTH_TOKEN=<your access token>
set GITHUB_AUTH_TOKEN=<your access token1>,<your access token2>
```

OR

-   [Create a GitHub App Installation](https://docs.github.com/en/developers/apps/building-github-apps/creating-a-github-app)
    for higher rate-limit quotas. If you have an installed GitHub App and key
    file, you can use the three environment variables below, following the
    commands (`set` or `export`) shown above for your platform.

```
GITHUB_APP_KEY_PATH=<path to the key file on disk>
GITHUB_APP_INSTALLATION_ID=<installation id>
GITHUB_APP_ID=<app id>
```

These variables can be obtained from the GitHub
[developer settings](https://github.com/settings/apps) page.

#### Basic Usage

##### Using repository URL

Scorecard can run using just one argument, the URL of the target repo:

```shell
$ scorecard --repo=github.com/ossf-tests/scorecard-check-branch-protection-e2e
Starting [CII-Best-Practices]
Starting [Fuzzing]
Starting [Pinned-Dependencies]
Starting [CI-Tests]
Starting [Maintained]
Starting [Packaging]
Starting [SAST]
Starting [Dependency-Update-Tool]
Starting [Token-Permissions]
Starting [Security-Policy]
Starting [Signed-Releases]
Starting [Binary-Artifacts]
Starting [Branch-Protection]
Starting [Code-Review]
Starting [Contributors]
Starting [Vulnerabilities]
Finished [CI-Tests]
Finished [Maintained]
Finished [Packaging]
Finished [SAST]
Finished [Signed-Releases]
Finished [Binary-Artifacts]
Finished [Branch-Protection]
Finished [Code-Review]
Finished [Contributors]
Finished [Dependency-Update-Tool]
Finished [Token-Permissions]
Finished [Security-Policy]
Finished [Vulnerabilities]
Finished [CII-Best-Practices]
Finished [Fuzzing]
Finished [Pinned-Dependencies]

RESULTS
-------
Aggregate score: 7.9 / 10

Check scores:
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
|  SCORE  |          NAME          |             REASON             |                         DOCUMENTATION/REMEDIATION                         |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| 10 / 10 | Binary-Artifacts       | no binaries found in the repo  | github.com/ossf/scorecard/blob/main/docs/checks.md#binary-artifacts       |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| 9 / 10  | Branch-Protection      | branch protection is not       | github.com/ossf/scorecard/blob/main/docs/checks.md#branch-protection      |
|         |                        | maximal on development and all |                                                                           |
|         |                        | release branches               |                                                                           |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| ?       | CI-Tests               | no pull request found          | github.com/ossf/scorecard/blob/main/docs/checks.md#ci-tests               |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| 0 / 10  | CII-Best-Practices     | no badge found                 | github.com/ossf/scorecard/blob/main/docs/checks.md#cii-best-practices     |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| 10 / 10 | Code-Review            | branch protection for default  | github.com/ossf/scorecard/blob/main/docs/checks.md#code-review            |
|         |                        | branch is enabled              |                                                                           |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| 0 / 10  | Contributors           | 0 different companies found -- | github.com/ossf/scorecard/blob/main/docs/checks.md#contributors           |
|         |                        | score normalized to 0          |                                                                           |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| 0 / 10  | Dependency-Update-Tool | no update tool detected        | github.com/ossf/scorecard/blob/main/docs/checks.md#dependency-update-tool |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| 0 / 10  | Fuzzing                | project is not fuzzed in       | github.com/ossf/scorecard/blob/main/docs/checks.md#fuzzing                |
|         |                        | OSS-Fuzz                       |                                                                           |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| 1 / 10  | Maintained             | 2 commit(s) found in the last  | github.com/ossf/scorecard/blob/main/docs/checks.md#maintained             |
|         |                        | 90 days -- score normalized to |                                                                           |
|         |                        | 1                              |                                                                           |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| ?       | Packaging              | no published package detected  | github.com/ossf/scorecard/blob/main/docs/checks.md#packaging              |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| 8 / 10  | Pinned-Dependencies    | unpinned dependencies detected | github.com/ossf/scorecard/blob/main/docs/checks.md#pinned-dependencies    |
|         |                        | -- score normalized to 8       |                                                                           |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| 0 / 10  | SAST                   | no SAST tool detected          | github.com/ossf/scorecard/blob/main/docs/checks.md#sast                   |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| 0 / 10  | Security-Policy        | security policy file not       | github.com/ossf/scorecard/blob/main/docs/checks.md#security-policy        |
|         |                        | detected                       |                                                                           |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| ?       | Signed-Releases        | no releases found              | github.com/ossf/scorecard/blob/main/docs/checks.md#signed-releases        |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| 10 / 10 | Token-Permissions      | tokens are read-only in GitHub | github.com/ossf/scorecard/blob/main/docs/checks.md#token-permissions      |
|         |                        | workflows                      |                                                                           |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
| 10 / 10 | Vulnerabilities        | no vulnerabilities detected    | github.com/ossf/scorecard/blob/main/docs/checks.md#vulnerabilities        |
|---------|------------------------|--------------------------------|---------------------------------------------------------------------------|
```

###### Docker

The `GITHUB_AUTH_TOKEN` has to be set to a valid [token](#Authentication)

```shell
docker run -e GITHUB_AUTH_TOKEN=token gcr.io/openssf/scorecard:stable --show-details --repo=https://github.com/ossf/scorecard
```

To use a specific scorecard version (e.g., v3.2.1), run:

```shell
docker run -e GITHUB_AUTH_TOKEN=token gcr.io/openssf/scorecard:v3.2.1 --show-details --repo=https://github.com/ossf/scorecard
```

##### Showing Detailed Results

For more details about why a check fails, use the `--show-details` option:

```
./scorecard --repo=github.com/ossf-tests/scorecard-check-branch-protection-e2e --checks Branch-Protection --show-details
Starting [Pinned-Dependencies]
Finished [Pinned-Dependencies]

RESULTS
-------
|---------|------------------------|--------------------------------|--------------------------------|---------------------------------------------------------------------------|
|  SCORE  |          NAME          |             REASON             |            DETAILS             |                         DOCUMENTATION/REMEDIATION                         |
|---------|------------------------|--------------------------------|--------------------------------|---------------------------------------------------------------------------|
| 9 / 10  | Branch-Protection      | branch protection is not       | Info: 'force pushes' disabled  | github.com/ossf/scorecard/blob/main/docs/checks.md#branch-protection      |
|         |                        | maximal on development and all | on branch 'main' Info: 'allow  |                                                                           |
|         |                        | release branches               | deletion' disabled on branch   |                                                                           |
|         |                        |                                | 'main' Info: linear history    |                                                                           |
|         |                        |                                | enabled on branch 'main' Info: |                                                                           |
|         |                        |                                | strict status check enabled    |                                                                           |
|         |                        |                                | on branch 'main' Warn: status  |                                                                           |
|         |                        |                                | checks for merging have no     |                                                                           |
|         |                        |                                | specific status to check on    |                                                                           |
|         |                        |                                | branch 'main' Info: number     |                                                                           |
|         |                        |                                | of required reviewers is 2     |                                                                           |
|         |                        |                                | on branch 'main' Info: Stale   |                                                                           |
|         |                        |                                | review dismissal enabled on    |                                                                           |
|         |                        |                                | branch 'main' Info: Owner      |                                                                           |
|         |                        |                                | review required on branch      |                                                                           |
|         |                        |                                | 'main' Info: 'admininistrator' |                                                                           |
|         |                        |                                | PRs need reviews before being  |                                                                           |
|         |                        |                                | merged on branch 'main'        |                                                                           |
|---------|------------------------|--------------------------------|--------------------------------|---------------------------------------------------------------------------|
```

##### Using a GitLab Repository

To run Scorecard on a GitLab repository, you must create a [GitLab Access Token](https://gitlab.com/-/profile/personal_access_tokens) with the following permissions:

- `read_api`
- `read_user`
- `read_repository`

You can run Scorecard on a GitLab repository by setting the `GITLAB_AUTH_TOKEN` environment variable:

```bash
export GITLAB_AUTH_TOKEN=glpat-xxxx

scorecard --repo gitlab.com/<org>/<project>/<subproject>
```

For an example of using Scorecard in GitLab CI/CD, see [here](https://gitlab.com/ossf-test/scorecard-pipeline-example).

##### Using GitHub Enterprise Server (GHES) based Repository

To use a GitHub Enterprise host `github.corp.com`, use the `GH_HOST` environment variable.

```shell
# Set the GitHub Enterprise host without https prefix or slash with relevant authentication token
export GH_HOST=github.corp.com
export GITHUB_AUTH_TOKEN=token

scorecard --repo=github.corp.com/org/repo
# OR without github host url
scorecard --repo=org/repo
```

##### Using a Package manager

For projects in the `--npm`, `--pypi`, `--rubygems`, or `--nuget` ecosystems, you have the
option to run Scorecard using a package manager. Provide the package name to
run the checks on the corresponding GitHub source code.

For example, `--npm=angular`.

##### Running specific checks

To run only specific check(s), add the `--checks` argument with a list of check
names.

For example, `--checks=CI-Tests,Code-Review`.

##### Formatting Results

The currently supported formats are `default` (text) and `json`.

These may be specified with the `--format` flag. For example, `--format=json`.
