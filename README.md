travis-npm-publish
==================

This script makes Travis publish your Node modules on npm for you.

Source:

* https://raw.githubusercontent.com/rsp/travis-npm-publish/master/travis-npm-publish

Shortcut:

* https://git.io/travis-npm-publish

[npm-url]: https://www.npmjs.com/package/travis-npm-publish
[github-url]: https://github.com/rsp/travis-npm-publish
[readme-url]: https://github.com/rsp/travis-npm-publish#readme
[issues-url]: https://github.com/rsp/travis-npm-publish/issues
[license-url]: https://github.com/rsp/travis-npm-publish/blob/master/LICENSE.md
[travis-url]: https://travis-ci.org/rsp/travis-npm-publish
[travis-img]: https://travis-ci.org/rsp/travis-npm-publish.svg?branch=master
[snyk-url]: https://snyk.io/test/github/rsp/travis-npm-publish
[snyk-img]: https://snyk.io/test/github/rsp/travis-npm-publish/badge.svg
[david-url]: https://david-dm.org/rsp/travis-npm-publish
[david-img]: https://david-dm.org/rsp/travis-npm-publish/status.svg
[github-follow-url]: https://github.com/rsp
[github-follow-img]: https://img.shields.io/github/followers/rsp.svg?style=social&label=Follow
[twitter-follow-url]: https://twitter.com/intent/follow?screen_name=pocztarski
[twitter-follow-img]: https://img.shields.io/twitter/follow/pocztarski.svg?style=social&label=Follow
[stackoverflow-url]: https://stackoverflow.com/users/613198/rsp
[stackexchange-url]: https://stackexchange.com/users/303952/rsp
[stackexchange-img]: https://stackexchange.com/users/flair/303952.png

Right now Travis has a built in way of publishing your modules on `npm` - see:

* https://docs.travis-ci.com/user/deployment/npm/

But this script may be more convenient to use and easier to customize. You don't need to have the `travis` CLI tool installed and you don't need to rely on how Travis handles the deployment to `npm` by default.

You can specify for which GitHub user the publishing is run (though even without it no one anauthorized would not be able to publish your module - it's just to avoid errors in the logs), for which branch or for tagged releases, and the script makes sure that the given tag matches the version from `package.json` and checks whether this version has already been published before trying to do it.

Usage
-----
It needs to be started as `after_success` script in `.travis.yml` - optionally specifying USER and BRANCH:

```sh
./travis-npm-publish
./travis-npm-publish USER
./travis-npm-publish USER BRANCH
./travis-npm-publish USER tags
```

Specifying USER make the script active only for this user's repository.

Specifying BRANCH make it active for all builds on that branch.

Not specifying BRANCH or giving a special value of `tags` makes the script active for tagged builds, but only if the tag matches the version specified in `package.json`.

Authorization
-------------
You need to set an environment variable in Travis called `NPM_AUTH` with a value of the line from your `~/.npmrc` that contains the authToken - this line looks like:

````
//registry.npmjs.org/:_authToken=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
````

Note: `NPM_AUTH` must contain the entire line, not just the token.

You set the variable in Travis in More options / Settings.
Make sure you set "Display value in build log" to off and keep this value secret.

Publishing
----------
When the GitHub repo of your module is set up with Travis and the script is set up properly, then publishing a module is a matter of, for example:

```js
npm version patch
git push origin master
```

The script will not try to publish the module to `npm` if:

* one of the required commands is not present
* the TRAVIS env var is not set (it's set automatically by Travis)
* the build is a pull request
* the build fails the tests
* the NPM_AUTH is not set
* the GitHub user doesn't match USER (if specified)
* the branch doesn't match BRANCH (if specified and is other than "tags")
* the tagged version doesn't match the version in `package.json` (if using tagged releases)
* the version specified in `package.json` has already been published on `npm`

Examples
--------
Example `.travis.yml` - using defaults:

```yaml
language: node_js
node_js: 4
script: npm test
after_success: "./travis-npm-publish"
```

Example `.travis.yml` - publishing only for GitHub repos by USER:

```yaml
language: node_js
node_js: 4
script: npm test
after_success: "./travis-npm-publish USER"
```

Example `.travis.yml` - publishing only for GitHub repos by USER on branch BRANCH:

```yaml
language: node_js
node_js: 4
script: npm test
after_success: "./travis-npm-publish USER BRANCH"
```

Issues
------
For any bug reports or feature requests please
[post an issue on GitHub][issues-url].

Author
------
[**Rafał Pocztarski**](https://pocztarski.com/)
<br/>
[![Follow on GitHub][github-follow-img]][github-follow-url]
[![Follow on Twitter][twitter-follow-img]][twitter-follow-url]
<br/>
[![Follow on Stack Exchange][stackexchange-img]][stackoverflow-url]

License
-------
MIT License (Expat). See [LICENSE.md](LICENSE.md) for details.
