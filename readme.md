# Yarn Security Fixes

> How to fix npm module security vulnerabilities in `yarn.lock` and `package.json`, including [case studies](#case-studies)

Security advisories are becoming more prevalent in the JavaScript / TypeScript ecosystem, with GitHub, npm, Snyk and other companies constantly researching and publishing new security vulnerabilities.

This guide aims to help those with less experience apply manual security vulnerability fixes with Yarn.

## Enable Bots First!

In some cases, these fixes will be applied automatically by bots like:

- [Dependabot](https://dependabot.com/) (built in to GitHub)
- [Renovate Bot](https://renovate.whitesourcesoftware.com/) ([**highly recommended!**](https://twitter.com/karlhorky/status/1245009))
- the [Snyk bot](https://support.snyk.io/hc/en-us/articles/360004032117-GitHub-scan-monitor-and-remediate)

**Recommendation:** Definitely set up the bots first (Renovate, Snyk and Dependabot)! They will fix many issues automatically for you, and keep your top-level dependencies in `package.json` up to date.

## Transitive Dependency Updates

However, these bots are not always able to come up with an automated fix for a fixable security vulnerability, especially with "transitive dependencies" (sub-dependencies of your dependencies - not found in `package.json`):

- [Dependabot failures](https://twitter.com/karlhorky/status/1239183744625446919)
- [Snyk Bot failures](https://twitter.com/karlhorky/status/1244712138511351809)

It can be difficult to know what to do with these security vulnerabilities, especially for those with less experience with package managers and how dependencies are resolved by Yarn and npm.

## Techniques to Fix Transitive Dependencies

Depending on a number of factors in your project, the technique to fix a security vulnerability will be different.

In order to choose a technique below, first open the resources that we will need:

1. Open the security advisory page (the page on GitHub, Snyk, etc)
2. Open the `yarn.lock` file in the project in your editor

Refer to the security advisory page and collect some information:

3. Which version(s) are marked "vulnerable" by the security advisory? Use the advisory in the [Snyk Vulnerability Database](https://snyk.io/vuln/) if there is one - it is often more accurate than the GitHub Security Advisory pages.
4. Which **new version** is required for each affected version?

Switch to the `yarn.lock` file and collect the following information:

5. Which vulnerable version(s) are installed in your `yarn.lock`?
   
   Find the current version by searching for `<package name>@` and referring to the **next line** - the actual installed version is on the next line after `version: `.
6. For each of these vulnerable versions, check the semver version ranges on the line above (the numbers after `<package name>@`). Are all semver version ranges compatible with the corresponding new, fixed version? If you are unsure how to check compatibility, you can try the [npm semver version calculator](https://semver.npmjs.com/).
   
   If all the semver version ranges are compatible with the new version, congratulations, your project is in [State A](#state-a-new-version-compatible-with-existing-semver-ranges).
   
   If any of the version ranges are not compatible, your project is in [State B](#state-b-new-version-incompatible-with-existing-semver-ranges).

### State A. New Version Compatible with Existing Semver Ranges

If the new, fixed version is compatible with all of your semver ranges of that package (check using [npm semver version calculator](https://semver.npmjs.com/)), then this is the simplest case - you can get Yarn to upgrade the single package by deleting the entry in the `yarn.lock` file ([Fix A1](#fix-a1-remove-yarnlock-entry-and-run-yarn)).

#### Fix A1. Remove `yarn.lock` Entry and Run `yarn`

Delete the entry for the package in the `yarn.lock` file and run `yarn` again.

See an example of how to do this in this video:

<a href="https://www.youtube.com/watch?v=iOYTpHRzL0A">
  <img src="upgrade-package.png" alt="Screen capture illustrating steps of upgrading dependency" />
</a>

### State B. New Version Incompatible with Existing Semver Ranges

If the new, fixed version is incompatible with any of your semver ranges of that package (check using [npm semver version calculator](https://semver.npmjs.com/)), then you'll have to do a bit of extra digging:

1. Find all of the incompatible semver ranges for affected versions
2. Search your `yarn.lock` file with `<package name> "<semver range` for every one of these semver ranges (eg. `minimist "0.0.8`). This will show you which **dependents** are depending on this version of this package. The dependent is found at the top of the entry in which the incompatible semver range is specified.
   
   Eg. In the image below, `minimist "^1.2.5` is the semver range that I searched for, and above in the same block, `mkdirp@^0.5.0` and `mkdirp@^0.5.1` are the dependents which depend on it:
   
   <img src="dependent-in-entry.png" width="600" alt="Yarn lockfile showing mkdirp depending on minimist" />
3. For each one of these dependents, make a note of the semver range **for the dependent** so that we can check how far we can upgrade the dependent. View the Git tags on GitHub (or whatever hosting platform the package uses) to see if there are any upgrades compatible with this range (or you can check using [npm semver version calculator](https://semver.npmjs.com/)) and then click on the tag to show the code at this version.
   
   <img src="github-tags.png" width="500" alt="Git tags on GitHub" />

   Check in the `package.json` under `dependencies` or `devDependencies` to see if the dependency you are interested about is at the version that you need. If it is, then you don't need to do any further steps and you can do [Fix B1](#fix-b1-upgrade-dependent-by-removing-yarnlock-entry). If the dependency is not at the version you need, then continue on to the next step.
4. Repeat steps 2 and 3 with the dependent, and then the dependent which depends on the dependent, until you have reached the top level of your dependencies (one of the packages mentioned in your `package.json`).
5. If you have reached the top level and there are no semver-compatible upgrades of any of the dependencies and transitive dependencies all the way up from the vulnerable package, then you can decide whether you want to use Yarn Resolutions to force a version to be installed which is incompatible with one of the semver ranges ([Fix B2](#fix-b2-forcing-incompatible-version-using-yarn-resolutions)) in the name of security.

#### Fix B1. Upgrade Dependent by Removing `yarn.lock` Entry

If you have found a dependent which depends on the vulnerable package, and this dependent has a semver-compatible version that depends on a fixed version of the vulnerable package, you can just remove the **dependent** entry from the `yarn.lock` file and run `yarn` again. This will upgrade both the dependent and also the vulnerable package.

This applies also to dependents further up in the tree - like a grandparent or great grandparent dependent.

See an example of how to do this in this video:

<a href="https://www.youtube.com/watch?v=tIofvKtMT3U">
  <img src="upgrade-dependent.png" alt="Screen capture illustrating steps of upgrading dependent" />
</a>

#### Fix B2. Forcing Incompatible Version Using Yarn Resolutions

If you have exhausted all options of upgrading any packages in a semver-compatible way, then Yarn Resolutions may be used to force a version of any package to be installed, even one incompatible with the semver range specified in a dependency.

For example, if the code below is added to `package.json`, it will force Yarn to use `minimist@0.2.1` for **all** versions (warning: this may lead to incompatibilities, see **Recommendation** below):

```json
  "resolutions": {
    "minimist": "0.2.1"
  }
```

**Recommendation:** Use Yarn Resolutions sparingly and in the most specific way possible. For example, if the problem is that `optimist` is the dependent that depends on `minimist@~0.0.1`, which is incompatible with the fixed version `minimist@0.2.1`, then you can force `minimist@0.2.1` **only** when depended on by `optimist` using the following resolution in `package.json`:

```json
  "resolutions": {
    "**/optimist/minimist": "0.2.1"
  }
```

## Case Studies

### `decompress`

#### `<4.2.1` - Arbitrary File Write Fix

Upgrade versions below `4.2.0` to `4.2.1`.

- **Advisory:** https://www.npmjs.com/advisories/1217/versions
- **GitHub PR:** https://github.com/kevva/decompress/pull/73#issuecomment-607268177

**Fix**

[Remove entry in `yarn.lock` and run `yarn`](https://github.com/karlhorky/yarn-lock-security-fixes/blob/master/README.md#1a-removing-yarnlock-entry-and-running-yarn)

https://github.com/karlhorky/gatsby-serverside-auth0/commit/fa44fb1b7a278ec066f9b8a9eb9f41b997b1287b

### `kind-of`

#### `>=6.0.0 <6.0.3` - Information Exposure

Upgrade versions between `6.0.0` and `6.0.2` to `6.0.3`.

- **Advisory:** https://snyk.io/vuln/SNYK-JS-KINDOF-537849
- **GitHub PR:** https://github.com/jonschlinkert/kind-of/pull/31

**Fix**

[Remove entry in `yarn.lock` and run `yarn`](https://github.com/karlhorky/yarn-lock-security-fixes/blob/master/README.md#1a-removing-yarnlock-entry-and-running-yarn)

https://github.com/karlhorky/next-offline-example/commit/66f2154680c45ff23af9628f33cefee1e4be3ad8
