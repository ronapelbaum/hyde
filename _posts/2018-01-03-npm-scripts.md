---
layout: post
title: Unleash the true pwoer of NPM
desc: How to manage your project with npm
date: 2018-01-03
author: ronapelbaum
comments: true
publish: true
tags: [npm]
---

## `package.json`

We are used to think of `package.json` as a _dependency manager_ for our project. But in fact it can (and should) be way more than that. We should use it as single source of truth for our project management.

## npm scripts

We can use the `"scripts"` section in `package.json` to define all our executable options of our project:
```json
{
   "scripts": {
       "lint": "eslint src/** --quiet --fix"
   }
}
```
Now, when we want to run:
```
npm run lint
```

> We use `npm run ...` as short for `npm run-script ...`

What's the benefit here?
- We don't have to remember all the cli options
- Usually, when you run `eslint` from (or any other executable npm package) you run it from _global node_modules_. but what if you want to run a tool from _current node_modules_? you'd have to run `./node_modules/.bin/eslint`. When you run from npm scripts, you're running the local node_modules first.

### Lifecycle scripts

Some scripts are considered "essential part of every node project". This way we run them without the `run-script` command.

#### `npm install`
Yes, it's basically a lifecycle script. If we'd like to add some logic to that phase, we can add:
```json
    "install": "node install.js"
```
#### `npm start`
Has a default implementation that runs `node server.js` but can be overriden.

#### `npm test`
We need to provide this, otherwise we'd get `Error: no test specified`.

> You can also use a shorter version. `npm i` for install, `npm t` for test.

## scripts hooks

Now let's say that we've defined our `test`, but we want to make sure that somthing always happes _before_:
```json
{
   "scripts": {
       "clean": "rm -rf coverage/",
       "test": "karma start karma.conf.js",
       "pretest": "npm run clean"
   }
}
```

When we run `npm test`, npm will discover the `pre` of `pretest` and make sure that `npm run clean` will be executed _before_ `npm test`.

The log will look like this:
```
ron$ npm test
> my-module@1.2.3 pretest /projects/my-module
> npm run clean

> my-module@1.2.3 clean /projects/my-module
> rm -rf coverage/

> my-module@1.2.3 test /projects/my-module
> karma start karma.conf.js

PhantomJS 2.1.1 (Mac OS X 0.0.0): Executed 100 of 100 SUCCESS (0 secs / 0 secs)

=============================== Coverage summary ===============================
Statements   : 100% ( 100/100 )
Branches     : 100% ( 80/80 )
Functions    : 100% ( 90/90 )
Lines        : 100% ( 1234/1234 )
================================================================================
ron$
```

## Embrace npm as your project manager

So if we can do these ellaborate steps with npm scripts, why do we need task mangers like _grunt_ or _gulp_?

We don't.

When using _grant_, we are dependent on grunt's _plugins_, whlie when using npmscripts, we are using only the tool that we need, using it's CLI.
