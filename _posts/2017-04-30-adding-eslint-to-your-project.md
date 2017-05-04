---
layout: post
title: Lint you code
date: 2017-04-30
author: ronapelbaum
comments: true
publish: false
tags: [eslint, javascript]
---
## Welcome to modern javascript

[ESLint](http://eslint.org/docs/user-guide/getting-started) is a mature linter for your javascript.

## Install
If you haven't already, you really should:
```
npm i -g eslint
eslint --init
```
IMO it's better to use recommended style guide, [airbnb](https://github.com/airbnb/javascript) being a very popular one.

![eslint-init]({{ site.url }}/_assets/eslint-init.png)

BTW, if you're still using ES5, just use [airbnb-legacy](https://www.npmjs.com/package/eslint-config-airbnb-base#eslint-config-airbnb-baselegacy).

### Run!
Now just run:
```
eslint **/*.js
```

### Too many errors?

First time you lint your code, you'll probably have million of errors.
Most of them are pretty easy to fix:
```
eslint --fix
```

### Embed in your `package.json`

Add basic scripts:

```json
{
  "scripts": {
    "lint": "eslint **/*.js",
    "lint-fix": "eslint **/*.js --fix --quiet"
  }
}
```

### Too painful? 

You can temporary and explicitly *ignore* linting errors for certain lines, or files by adding
```javascript
/* eslint-disable */
```

A good strategy, will be to add this comment to the header of all you files, and dictate a dev policy that every developer who touches a file, needs to fix the linting issues.

Check out this [script](https://gist.github.com/barzik/d6f43a1ee650643ced19a5094d4cec3d), created by [Ran Bar-Zik](https://internet-israel.com/).

## TypeScript or es6? ESLint!

This is the motivation part...

Long time ago (about 2 yrs) I was very excited about TypeScript. after suffering a *lot* of painful bad-written javascript, I thought that here was our redemption.

But then ESLint came forward, offering good linting to ES6 as well. So now, when you have very good rules for ESLint, although it's not a *real* compiler, it's doing a pretty good job.
Add [node support for ES6](https://nodejs.org/en/docs/es6/) to the equation, and understand why I stopped praising TypeScript... 

## `.editorconfig`?

How many times you've [blamed](https://help.github.com/articles/tracing-changes-in-a-file/) a code line in `git` just to find that all recent changes were indentations?

[EditorConfig](http://editorconfig.org/) is a tool to help you set a common style guide for all project's developers.

But, you don't really need it.

You have ESLint.

## The next level

#### run ESLint with gulp
Install [gulp-eslint](https://www.npmjs.com/package/gulp-eslint)

```javascript
const gulp = require('gulp');
const eslint = require('gulp-eslint');
gulp.task('eslint', () => gulp.src(['**/*.js','!node_modules/**']).pipe(eslint));
```
#### run ESLint with webpack
Install [eslint-loader](https://www.npmjs.com/package/eslint-loader)

```javascript
module.exports = {
  // ... 
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "eslint-loader",
      },
    ],
  },
  // ... 
}
```

#### run ESLint with travis
 ```yaml
script:
  - npm run lint
  - npm run build
  - npm test
```

#### run ESLint with npm
My personal favorite. 

```json
{
  "scripts": {
    "build-all": "npm run lint && npm run build && npm test",
  }
}
```
