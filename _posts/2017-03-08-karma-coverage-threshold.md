---
layout: post
title: Keep it Covered
desc: How to use karma to measure & enforce your code coverage
date: 2017-03-08
author: ronapelbaum
comments: true
publish: true
tags: [javascript, karma]
---

## Where you *are*

So you've set up your [karma](https://karma-runner.github.io), and you have some tests.
<br/>
How do you measure your tests *'effectiveness'*?

> *Code Coverage* is how much of your code us covered by your tests.   

## karma-coverage

Meet [karma-coverage](https://github.com/karma-runner/karma-coverage).
it's a plugin for karma, that helps you measure the *code coverage*.
<br/>
How does it work?
It *instruments* your code before test run, and then it measures all the code that was executed during the test run.

<br/>
First, install it:

```
npm install karma-coverage --save-dev
```

Then, you need to update your `karma.conf.js` file with the following:

```javascript
config.set({
    
    ...
    
    preprocessors: {
          'src/**/*.js': ['coverage']
        },
    
    reporters: ['coverage'],  

    // optionally, configure the reporter
    coverageReporter: {
      type : 'html',
      dir : 'coverage/'
    }
    
    ...
    
})
```

1. Add `'coverage'` to your `preprocessors` array - *instrumentation*.
2. Add `'coverage'` to your `reporters` array.
3. Configure `coverageReporter` entry - how do you want your report.
<br/>

Personally, I also like the `text-summary` report:

```javascript
coverageReporter: {
    reporters: [
            {
                type: 'html',
                dir: 'coverage/'
            },
            {
                type: 'text-summary'
            }
        ]
    }
```

This gives you a nice summary in your console:

```
====================== Coverage summary =======================
Statements   : 68.41% ( 13267/19394 )
Branches     : 50.21% ( 4262/8488 )
Functions    : 62.07% ( 2540/4092 )
Lines        : 68.46% ( 13249/19352 )
================================================================
```

## Where you *want to be*

Now, le'ts say that your code coverage is 67%.
<br/>
What you really want is to keep it that way. You want to *fail* test run if the code coverage get's below your threshold.
<br/>
Why?
1. Someone might have added new code without proper testing - *bad*
2. Someone might commit `fdescribe()` by mistake - *terrible!*

Well, since [2015](https://github.com/karma-runner/karma/issues/673#issuecomment-137896304) you can simply add `check`:

```javascript
coverageReporter: {
  check: {
    global: {
       statements: 82,
       lines: 67,
       functions: 100,
       branches: 85
    }}
}
```

## Summary

Check out my [learn jasmine project](https://github.com/ronapelbaum/mangal), to see full settings.
