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

## `minimist`

### `<0.2.1 || >=1.0.0 <1.2.3` - Prototype Pollution

Upgrade versions below `0.2.1` to `0.2.1` and versions between `1.0.0` and `1.2.2` to `1.2.3`.

- **Advisory:** https://snyk.io/vuln/SNYK-JS-KINDOF-537849
- **GitHub PR:** https://github.com/jonschlinkert/kind-of/pull/31

#### Fix

1. Remove entry of `minimist@1.2.2` in `yarn.lock` and run `yarn`: https://github.com/karlhorky/yarn-lock-security-fixes/blob/master/README.md#1a-removing-yarnlock-entry-and-running-yarn
2. Use Yarn Resolutions to force semver-incompatible version `minimist@0.2.1` and override semver range `minimist@~0.0.1` in `optimist`: https://github.com/karlhorky/yarn-security-fixes#fix-b2-forcing-incompatible-version-using-yarn-resolutions

Commits: https://github.com/karlhorky/react-malarkey/commit/0cde1f67d7770c91ffa5d8566e8c45de5f4d2d21
