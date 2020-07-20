# Case Studies

Example case studies of how recent npm / Yarn security vulnerabilities from GitHub security advisories were fixed with Yarn (v1), either by editing lockfiles or by using Yarn Resolutions.

## `decompress`

### `<4.2.1` - Arbitrary File Write Fix

Upgrade versions below `4.2.0` to `4.2.1`.

- **Advisory:** https://www.npmjs.com/advisories/1217/versions
- **GitHub PR:** https://github.com/kevva/decompress/pull/73#issuecomment-607268177

#### Fix

1. Remove entry in `yarn.lock` and run `yarn`: https://github.com/karlhorky/yarn-lock-security-fixes/blob/master/README.md#1a-removing-yarnlock-entry-and-running-yarn

Commits: https://github.com/karlhorky/gatsby-serverside-auth0/commit/fa44fb1b7a278ec066f9b8a9eb9f41b997b1287b

## `https-proxy-agent`

### `<2.2.3` - Machine-In-The-Middle

Upgrade versions before `2.2.3` to at least `2.2.3`.

- **Advisory:** https://www.npmjs.com/advisories/1184
- **GitHub PR:** https://github.com/TooTallNate/node-https-proxy-agent/commit/36d8cf509f877fa44f4404fce57ebaf9410fe51b

#### Fix

1. Remove entry in `yarn.lock` and run `yarn`: https://github.com/karlhorky/yarn-lock-security-fixes/blob/master/README.md#1a-removing-yarnlock-entry-and-running-yarn

Commits: https://github.com/upleveled/course-material/commit/5a2344a41fccd45ee46d1cf9197e0cd4e14bdfca

## `kind-of`

### `>=6.0.0 <6.0.3` - Information Exposure

Upgrade versions between `6.0.0` and `6.0.2` to `6.0.3`.

- **Advisory:** https://snyk.io/vuln/SNYK-JS-KINDOF-537849
- **GitHub PR:** https://github.com/jonschlinkert/kind-of/pull/31

#### Fix

1. Remove entry in `yarn.lock` and run `yarn`: https://github.com/karlhorky/yarn-lock-security-fixes/blob/master/README.md#1a-removing-yarnlock-entry-and-running-yarn

Commits: https://github.com/karlhorky/next-offline-example/commit/66f2154680c45ff23af9628f33cefee1e4be3ad8

## `lodash`

### `<4.17.19` - Prototype Pollution

Upgrade versions below `4.17.19` to `4.17.19`.

- **Advisory:** https://www.npmjs.com/advisories/1523
- **GitHub Commit:** https://github.com/lodash/lodash/commit/c84fe82760fb2d3e03a63379b297a1cc1a2fce12

#### Fix

1. Remove entry of `lodash` in `yarn.lock` and run `yarn`: https://github.com/karlhorky/yarn-lock-security-fixes/blob/master/README.md#1a-removing-yarnlock-entry-and-running-yarn

Commits: https://github.com/upleveled/upleveled-vscode-eslint-base-config/commit/77d83e8b7f4dbffce9a39bb9c41e10ab61584759

## `minimist`

### `<0.2.1 || >=1.0.0 <1.2.3` - Prototype Pollution

Upgrade versions below `0.2.1` to `0.2.1` and versions between `1.0.0` and `1.2.2` to `1.2.3`.

- **Advisory:** https://snyk.io/vuln/SNYK-JS-KINDOF-537849
- **GitHub PR:** https://github.com/jonschlinkert/kind-of/pull/31

#### Fix

1. Remove entry of `minimist@1.2.2` in `yarn.lock` and run `yarn`: https://github.com/karlhorky/yarn-lock-security-fixes/blob/master/README.md#1a-removing-yarnlock-entry-and-running-yarn
2. Use Yarn Resolutions to force semver-incompatible version `minimist@0.2.1` and override semver range `minimist@~0.0.1` in `optimist`: https://github.com/karlhorky/yarn-security-fixes#fix-b2-forcing-incompatible-version-using-yarn-resolutions

Commits: https://github.com/karlhorky/react-malarkey/commit/0cde1f67d7770c91ffa5d8566e8c45de5f4d2d21

## `websocket-extensions`

### `<0.1.4` - Regular Expression Denial of Service

Upgrade versions below `0.1.4` to `0.1.4`.

- **Advisory:** https://github.com/advisories/GHSA-g78m-2chm-r7qv
- **GitHub Commit:** https://github.com/faye/websocket-extensions-node/commit/29496f6838bfadfe5a2f85dff33ed0ba33873237

#### Fix

1. Remove entry of `websocket-extensions@0.1.4` in `yarn.lock` and run `yarn`: https://github.com/karlhorky/yarn-lock-security-fixes/blob/master/README.md#1a-removing-yarnlock-entry-and-running-yarn

Commits: https://github.com/karlhorky/shopify-landing-clone/commit/11dc1c30a60fac5671dfb70cada13b7ad69f81ce
