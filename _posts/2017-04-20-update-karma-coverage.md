---
layout: post
title: Update karma coverage threshold automatically
desc: How to continuously raise you coverage threshold 
date: 2017-04-20
author: ronapelbaum
comments: true
publish: true
tags: [javascript, karma]
---

In [previous post](/2017/03/08/karma-coverage-threshold) we saw how we can define the *karma coverage threshold*, using the magic `check` command: 


```javascript
coverageReporter: {
  check: {
    global: {
       statements: 82,
       lines: 67,
       functions: 100,
       branches: 85
    }
  }
}
```

This way the test run will *fail* if the code coverage get’s below your threshold. 

> **Note!** This way you can also prevernt silly unfortunate mistakes, such as committing the **f** word (`fdescrib` or `fit`)...

## Show Must Go On...
Well, life happens, and you continue with your project.
<br/>
You've been a very good boy, and your coverage is now better.
<br/>
You want to update you're coverage threshold to the new standard.

## Manual Labor?
No.

## Threshold Outsourcing
First, save your threshold in a separate file: 
```javascript
coverageReporter: {
  check: {
    global: require('./coverage.conf.json') 
  }
}
```
Now, your *coverage.conf.json* will look like this:
```json
{
  "statements": 68,
  "branches": 49,
  "functions": 61,
  "lines": 68
}
```

## Json Coverage Report
It turns out that you also have a [json reporter](https://github.com/gotwarlost/istanbul/blob/master/lib/report/json.js) and a [json-summary reporter](https://github.com/gotwarlost/istanbul/blob/master/lib/report/json-summary.js). Unfortunately, it still doesn't show up in [karma-coverage docs](https://github.com/karma-runner/karma-coverage#advanced-multiple-reporters) yet (I've opened a PR).
 
Try this:

```javascript
reporters: [
         {type: 'json-summary'}
      ]
```
You'll get a beautiful json, with your summary coverage.

## Define New Goals
First, let's get the last run coverage summary:
```javascript
//read the "coverage-summary.json" file
let fileContent = fs.readFileSync('coverage-summary.json');
//parse string to json
let coverageSummary = JSON.parse(fileContent);
```

Then, extract the percentage results.

We'de like to add a `FLEXIBILITY` factor, to reduce the roughness of the latest coverage results (I've set it to `0.5`):
```javascript
//define new thresholdConfig
let thresholdConfig = {};
//extract data from coverageSummary to thresholdConfig
//FLEXIBILITY is a number to have some flexibility 
['lines', 'statements', 'branches', 'functions'].forEach(key => thresholdConfig[key] = coverageSummary.total[key].pct - FLEXIBILITY);

```

Now, you can define you new threshold.
```javascript
//overwrite the thresholdConfig to "coverage.conf.json"
fs.writeFileSync('coverage.conf.json', JSON.stringify(thresholdConfig), 'utf8');
```

## CI/CD
The next stage would be to add a stage to you CI/CD cycle:

- commit
- run test
- **update thresholds**
- **commit "coverage.conf.json"** (but *don't* rerun tests)
