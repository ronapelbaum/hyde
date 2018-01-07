---
layout: post
title: ESLint as Git Hook
date: 2017-11-30
author: ronapelbaum
comments: true
publish: true
tags: []
---
I came across this [great post](https://internet-israel.com/%D7%A4%D7%99%D7%AA%D7%95%D7%97-%D7%90%D7%99%D7%A0%D7%98%D7%A8%D7%A0%D7%98/%D7%91%D7%A0%D7%99%D7%99%D7%AA-%D7%90%D7%AA%D7%A8%D7%99-%D7%90%D7%99%D7%A0%D7%98%D7%A8%D7%A0%D7%98-%D7%9C%D7%9E%D7%A4%D7%AA%D7%97%D7%99%D7%9D/%D7%A9%D7%99%D7%9E%D7%95%D7%A9-%D7%91-husky-%D7%9C%D7%90%D7%9B%D7%99%D7%A4%D7%AA-git-hooks/) by my friend [Ran Bar-Zik](https://github.com/barzik).
He explains how to easily [set up `eslint`](/2017/04/30/adding-eslint-to-your-project) to run as a [git hook](https://git-scm.com/docs/githooks).

## Use `huskey`
First, you'll nee [huskey](https://www.npmjs.com/package/husky):
```bash
npm i -D huskey
```

Now, just add `precommit` script:
```json
"scripts": {
  "precommit": "eslint ."
}
```

## Save Time
While this is great, the problem is that running `eslint` on your whole project, might take a while...
So, since you obviously use a build process that's running `eslint` on the full code base, on `precommit` you can just run it on _modified_ files!

Get modified `*.js` files:
```bash
git ls-files --modified | grep .js$
```

Now, in `package.json`:
```json
"scripts": {
  "precommit": "eslint `git diff --name-only --staged | grep .js$` --fix"
}
```

#### Note
The backtick ` allows us to run a "nested" command.

#### Note 2
I _love_ the `--fix` flag..
