---
title: JavaScript Promises
date: "2020-04-13"
template: "post"
draft: false
slug: "js-promises"
category: "Code"
tags:
  - "Code"
description: "Promises have been a part of JavaScript I've used but have never fully understood, here's my attempt to understand them more."
---
```javascript
const promiseA = new Promise( (resolutionFunc,rejectionFunc) => {
    resolutionFunc(777);
});
// At this point, "promiseA" is already settled.
promiseA.then( (val) => console.log("asynchronous logging has val:",val) );
console.log("immediate logging");
  ```