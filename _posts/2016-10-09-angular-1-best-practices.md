---
layout: post
title: angular 1.x - Best Practices
date: 2016-09-09
author: ronapelbaum
comments: true
tags: []
publish: false
---
# angular 1.x

## Project Structure

## Service

Our main goal is to [think of our angular service as a javascript class](https://ronapelbaum.github.io/2016/08/08/upgrade-your-angular/).

```javascript
(function () {
    function MyService(OtherService) {
        this.foo = function () {
            OtherService.goo();
        }
    }
    MyService.$inject = ['OtherService'];

    angular.module('app').service('MyService', MyService);
})();
```

This code is very easy to migrate to TypeScript:

```typescript
class MyService {
    static $inject = ["OtherService"];
    
    constructor(private OtherService:OtherService) {

    }
    
    public foo () {
        this.OtherService.goo();
    }

}
angular.module('app').service('MyService', MyService);

```

## Controller 
