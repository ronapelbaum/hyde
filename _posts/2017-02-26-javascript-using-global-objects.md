---
layout: post
title: The Global Warning
desc: You are using global properties. You really should stop doing that...
date: 2017-02-26
author: ronapelbaum
comments: true
publish: true
tags: [javascript]
---
## Your Code
Your code must have something like:
```javascript
function foo(){
    if(document.hidden){
        //do something
    } else {
        //do something else
    }
}
```
Well, what's the problem with that?

## The Problem
You are using a *global property*.
How are you planning to test this?
Try this:
```javascript
it("should not work", function(){
    document.hidden = mockValue;
    foo();
    expect(...).toBe(...);
});
```
First, this doesn't work for browser's global properties (on `window` or `document`),
But even for your own global properties (which you really should try to avoid...), it is really bad practise. 
What if you fail to remember to return the  correct value?
<br/>
<br/>
Let's say that your'e using global *functions*.
This is less bad:
```javascript
function foo(){
    window.blur();
}
```
Why? because you can *spy* on functions:
```javascript
it("should call window.blur()", function(){
    spyOn(window, 'blur');
    foo();
    expect(window.blur).toHaveBeenCalled();
});
```
So, what to do?

## Better Code
If you're using a framework such as *angularjs*, you should use the provided *wrapper*. In angular it is `$window`. 
Now, since angular is based on *dependency injection*, you can easily *inject* your mocked object.
<br/>
But what to do for vanilla?
```javascript
function getDocument(){
    return window.document;
}
///
function foo(){
    if(getDocument().hidden){
        //do something
    } else {
        //do something else
    }
}
```
Now, you can test your BL:
```javascript
it("should work now", function() {
    spyOn(window, 'getDocument').and.returnValue(mockDucument);
    mockDucument.hidden = mockValue;
    foo();
    expect(...).toBe(...);
});
```
Basically, you've changed your code use DI (dependency injection), and now your code is testable.

## Wait, Should I change my code just for a test?
Yes.
<br/>
Your code is bad. You're not changing it, you're fixing it.
<br/>
Testable code is better code.
