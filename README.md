# semantic-release-github-pr
[![Build Status](https://travis-ci.org/Updater/semantic-release-github-pr.svg?branch=master)](https://travis-ci.org/Updater/semantic-release-github-pr) [![npm](https://img.shields.io/npm/v/semantic-release-github-pr.svg)](https://www.npmjs.com/package/semantic-release-github-pr) [![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)

Preview the semantic release notes that would result from merging a Github PR.

![image](https://user-images.githubusercontent.com/356320/33625928-257bc906-d9c7-11e7-9adb-de85726952eb.png)

Instead of publishing a new release, this [`semantic-release`](https://github.com/semantic-release/semantic-release) plugin will post a comment to any open Github PRs with a preview of the generated release notes.

## Install
```bash
npm install -D semantic-release-github-pr
```

## Configuration
This plugin is a complement to `semantic-release`. It is assumed the user is already fully familiar with that package and its workflow.

### Github
Github authentication must be configured for `semantic-relase`. 

* The [`semantic-release` setup instructions](https://github.com/semantic-release/semantic-release#setup)
* [README for `semantic-release`'s Github plugin](https://github.com/semantic-release/github/#github-repository-authentication)

## Usage
The easiest way to use the plugin is through the CLI. If more flexibility is needed, the plugin can also be set through the `semantic-release` config in `package.json`.

### CLI
Post a comment to any open Github PRs with a matching base branch and git head (previous comments generated by this plugin will be removed).
```bash
npx semantic-release-github-pr
```

The `semantic-release-github-pr` command is provided as a convenience. Internally, it simply wraps the `semantic-release` command, applying the necessary `publish` plugin (see config below). Otherwise, it works just the same and accepts [the same options](https://github.com/semantic-release/semantic-release#cli) .

### `package.json` config
If more flexibility is needed, this package provides a [`publish` plugin](https://github.com/semantic-release/semantic-release#publish).

Default configuration (as encapsulated in [the `semantic-release-github-pr` command](https://github.com/Updater/semantic-release-github-pr/blob/master/bin/semantic-release-github-pr.js)):
```json
{
  "release": {
    "publish": "semantic-release-github-pr",
    "verifyConditions": "@semantic-release/github"
  }
}
```

## CI
This plugin is best used with a CI build process to fully automate the posting of comments in Github PRs. Ideally, it should run whenever a relevant PR is opened.

### Travis
By default, Travis will trigger a build whenever a PR is opened (pr) or updated (push) in the corresponding Github repo. 

```yaml
after_success:
  - "npx semantic-release-github-pr"
```

### CircleCI
Unfortunately, CircleCI only supports building on push, [not when a PR is created](https://discuss.circleci.com/t/trigger-new-build-on-pr/4219). This limits the usefulness of the plugin somewhat, as a build will have to be triggered manually after a PR is opened.

```yaml
test:
  post:
    - "npx semantic-release-github-pr"
```