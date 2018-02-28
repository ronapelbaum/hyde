---
layout: post
title: Manage your packages with versions
desc: How to manage your packages properly with npm version
date: 2017-12-15
author: ronapelbaum
comments: true
publish: true
tags: [npm, git]
---

## Youe package

So you have your really cool open source project.
Now what?
```
npm publish
```

Yep, now your package is public and anyone can use it.

## Show must go on...

You are continously improving your package, adding features and fix bugs...

Now you need to update your published package, so that you can share the love with the big world.
If you'll try to run `npm publish`, it will fail. You need to bump your package version first.

You can just change the version in your `package.json` and then run `npm publish`.

## Meet [`semver`](http://semver.org/)

[`semver`](http://semver.org/) is a modern approach for managing package versions.

**TL;DR;**

A package version should have a format of `x.y.z`:
- `x`: *MAJOR* version: should increment on *api break*.
- `y`: *MINOR* version: should increment on *api change*.
- `z`: *PATCH* version: should increment on every tiny change (bug fixes)

## `npm version`

Instead of changing the version in `package.json` manually, you can use the [`npm version`](https://docs.npmjs.com/cli/version) command:

i.e.
```
npm version patch
```
*What does this do?*
1. change the version in `package.json`
2. commit the change (`git commit`)
3. create a `git tag`

> Since `npm version` command is using git commands, it's better that your git working diractory will be clean from any unstaged/uncommitted changes

## `git tag`

Git tags are simple pointers, like branches, that point to a commit in the commit tree. 
> You can think of _tags_ as _read-only_ pointers, that once you've set a tag to a commit it stays forever, as apposed to _branches_ which are _read-and-write_ pointers, that are changed to point to your latest commit on your branch.

This allows us to continue working on the project, and still keep "bookmarks" of all our previous versions that were published.

## `git push`

Since we've mode some code changes (to _package.json_), we now need to push the changes back to git origin. We also want to push the new created _tag_:
```
git push && git push --tags 
```
We can also use the [`follow-tags`](https://git-scm.com/docs/git-push#git-push---follow-tags) flag:
```
git push --follow-tags
```

## `npm publish`

Now git origin has all the data about our new release. All that's left is to publish it to npm repository:
``` 
npm publish
```
