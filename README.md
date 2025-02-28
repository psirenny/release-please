<img src="https://avatars2.githubusercontent.com/u/2810941?v=3&s=96" alt="Google Cloud Platform logo" title="Google Cloud Platform" align="right" height="96" width="96"/>

# [Release Please](https://github.com/googleapis/release-please)

[![npm version](https://img.shields.io/npm/v/release-please.svg)](https://www.npmjs.org/package/release-please)
[![codecov](https://img.shields.io/codecov/c/github/googleapis/release-please/main.svg?style=flat)](https://codecov.io/gh/googleapis/release-please)

Release Please automates CHANGELOG generation, the creation of GitHub releases,
and version bumps for your projects.

It does so by parsing your
git history, looking for [Conventional Commit messages](https://www.conventionalcommits.org/),
and creating release PRs.

## What's a Release PR?

Rather than continuously releasing what's landed to your default branch,
release-please maintains Release PRs:

<img width="400" src="/screen.png">

These Release PRs are kept up-to-date as additional work is merged. When you're
ready to tag a release, simply merge the release PR. Both squash-merge and
merge commits work with Release PRs.

When the Release PR is merged, release-please takes the following steps:

1. Updates your changelog file (for example `CHANGELOG.md`), along with other language specific files (for example `package.json`).
2. Tags the commit with the version number
3. Creates a GitHub Release based on the tag

You can tell where the Release PR is its lifecycle by the status label on the
PR itself:

- `autorelease:pending` is the initial state of the Release PR before it is merged
- `autorelease:tagged` means that the Release PR has been merged and the release has been tagged in GitHub
- `autorelease:published` means that a GitHub release has been published based on the Release PR (_release-please does not automatically add this tag, but we recommend it as a convention for publication tooling_).

## How should I write my commits?

Release Please assumes you are using [Conventional Commit messages](https://www.conventionalcommits.org/).

The most important prefixes you should have in mind are:

* `fix:` which represents bug fixes, and correlates to a [SemVer](https://semver.org/)
  patch.
* `feat:` which represents a new feature, and correlates to a SemVer minor.
* `feat!:`,  or `fix!:`, `refactor!:`, etc., which represent a breaking change
  (indicated by the `!`) and will result in a SemVer major.

### What if my PR contains multiple fixes or features?

Release Please allows you to represent multiple changes in a single commit,
using footers:

```txt
feat: adds v4 UUID to crypto

This adds support for v4 UUIDs to the library.

fix(utils): unicode no longer throws exception
  PiperOrigin-RevId: 345559154
  BREAKING-CHANGE: encode method no longer throws.
  Source-Link: googleapis/googleapis@5e0dcb2

feat(utils): update encode to support unicode
  PiperOrigin-RevId: 345559182
  Source-Link: googleapis/googleapis@e5eef86
```

The above commit message will contain:

1. an entry for the **"adds v4 UUID to crypto"** feature.
2. an entry for the fix **"unicode no longer throws exception"**, along with a note
  that it's a breaking change.
3. an entry for the feature **"update encode to support unicode"**.

> :warning: **Important:** The additional messages must be added to the bottom of the commit.

## How do I change the version number?

When a commit to the main branch has `Release-As: x.x.x`(case insensitive) in the **commit body**, Release Please will open a new pull request for the specified version.

**Empty commit example:**

`git commit --allow-empty -m "chore: release 2.0.0" -m "Release-As: 2.0.0"` results in the following commit message:

```txt
chore: release 2.0.0

Release-As: 2.0.0
```

## How can I fix release notes?

If you have merged a pull request and you would like to amend the commit message
used to generate the release notes for that commit, you can edit the body of
the merged pull requests and add a section like:

```
BEGIN_COMMIT_OVERRIDE
feat: add ability to override merged commit message

fix: another message
chore: a third message
END_COMMIT_OVERRIDE
```

The next time release please runs, it will use that override section as the
commit message instead of the merged commit message.

## Strategy (Language) types supported

Release Please automates releases for the following flavors of repositories:

| release type            | description
|-------------------|---------------------------------------------------------|
| `dart`              | A repository with a pubspec.yaml and a CHANGELOG.md |
| `elixir`            | A repository with a mix.exs and a CHANGELOG.md |
| `go`                | A repository with a CHANGELOG.md |
| `helm`              | A repository with a Chart.yaml and a CHANGELOG.md |
| `krm-blueprint`     | [A kpt package, with 1 or more KRM files and a CHANGELOG.md](https://github.com/GoogleCloudPlatform/blueprints/tree/main/catalog/project) |
| `node`              | [A Node.js repository, with a package.json and CHANGELOG.md](https://github.com/yargs/yargs) |
| `ocaml`             | [An OCaml repository, containing 1 or more opam or esy files and a CHANGELOG.md](https://github.com/grain-lang/binaryen.ml) |
| `php`               | A repository with a composer.json and a CHANGELOG.md |
| `python`            | [A Python repository, with a setup.py, setup.cfg, CHANGELOG.md](https://github.com/googleapis/python-storage) and optionally a pyproject.toml and a &lt;project&gt;/\_\_init\_\_.py |
| `ruby`              | A repository with a version.rb and a CHANGELOG.md |
| `rust`              | A Rust repository, with a Cargo.toml (either as a crate or workspace) and a CHANGELOG.md |
| `simple`            | [A repository with a version.txt and a CHANGELOG.md](https://github.com/googleapis/gapic-generator) |
| `terraform-module`  | [A terraform module, with a version in the README.md, and a CHANGELOG.md](https://github.com/terraform-google-modules/terraform-google-project-factory) |

## Setting up Release Please

There are a variety of ways you can deploy release-please: 

### GitHub Action (recommended)

The easiest way to run release please is as a GitHub action. Please see [google-github-actions/release-please-action](https://github.com/google-github-actions/release-please-action) for installation and configuration instructions.

### Running as CLI

Please see [Running release-please CLI](docs/cli.md) for all the configuration options.

### Install the GitHub App

There is a probot application available, which allows you to deploy Release
Please as a GitHub App. Please see 
[github.com/googleapis/repo-automation-bots](https://github.com/googleapis/repo-automation-bots/tree/main/packages/release-please)
for installation and configuration instructions.

## Bootstrapping your Repository

Release Please looks at commits since your last release tag. It may or may not be able to find
your previous releases. The easiest way to on-board your repository is to
[bootstrap a manifest config](/docs/cli.md#bootstrapping).

## Customizing Release Please

Release Please provides several configuration options to allow customizing
your release process. Please see [customizing.md](docs/customizing.md) for more details.

## Supporting Monorepos via Manifest Configuration

Release Please also supports releasing multiple artifacts from the same repository.
See more at [manifest-releaser.md](docs/manifest-releaser.md).

## Supported Node.js Versions

Our client libraries follow the [Node.js release schedule](https://nodejs.org/en/about/releases/).
Libraries are compatible with all current _active_ and _maintenance_ versions of
Node.js.

Client libraries targeting some end-of-life versions of Node.js are available, and
can be installed via npm [dist-tags](https://docs.npmjs.com/cli/dist-tag).
The dist-tags follow the naming convention `legacy-(version)`.

_Legacy Node.js versions are supported as a best effort:_

* Legacy versions will not be tested in continuous integration.
* Some security patches may not be able to be backported.
* Dependencies will not be kept up-to-date, and features will not be backported.

#### Legacy tags available

* `legacy-8`: install client libraries from this dist-tag for versions
  compatible with Node.js 8.

## Versioning

This library follows [Semantic Versioning](http://semver.org/).

## Contributing

Contributions welcome! See the [Contributing Guide](https://github.com/googleapis/release-please/blob/main/CONTRIBUTING.md).

Please note that this `README.md`, the `samples/README.md`,
and a variety of configuration files in this repository (including `.nycrc` and `tsconfig.json`)
are generated from a central template. To edit one of these files, make an edit
to its template in this
[directory](https://github.com/googleapis/synthtool/tree/main/synthtool/gcp/templates/node_library).

## License

Apache Version 2.0

See [LICENSE](https://github.com/googleapis/release-please/blob/main/LICENSE)
