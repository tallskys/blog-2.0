---
title: JavaScript Promises
date: "2020-04-13"
template: "post"
draft: false
slug: "js-promises"
category: "Code"
tags:
  - "Code"
description: "Promises have been a part of JavaScript I've used but have never fully understood, here's my attempt to understand them a little more."
---
This is what the [MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) say about promises:

>The Promise object represents the eventual completion (or failure) of an asynchronous operation, and its resulting value.

Which is a lot of big words to say _"a promise is a declaration that something will happen in the future"_. Well, that's not even a concept original to coding, that's just a general life concept. If I promise my sick friend I'm going to make them some delicious chicken noodle soup and drop it off, I'm declaring that an action is going to happen, and now my friend knows to wait for that good good soup. 

Translating this to code, it lets asynchronous methods run like synchronous methods, instead of giving a value back immediately, it _promises_ that it will return some value at some stage in the future. Like my soup, I can't make it appear in my friends home instantaneously, but they've been told it's coming.

Promises exist in 3 states:
  - _Pending_ --neither fulfilled nor rejected
  - _Fulfilled_ --the promise has resolved succesfully
  - _Rejected (error)_ --the operation has failed

This is what the promise constructor syntax looks like:
```javascript
var promiseObj = new Promise(function(resolutionFunc, rejectionFunc){
    // some asynchronous coooooode
} );
```

Let's try to translate this soup metaphor that I am absolutely forcing at this point, and see what we can do. (I desperately want soup now.)

```javascript
const goodFriend = new Promise(function(resolve, reject) {
  let bringsFriendSoup = true;
  if (bringsFriendSoup) {
    resolve(console.log("I'm a damn good friend"));
  } else {
    reject(console.log("put yourself in the toilet, you garbage friend"));
  }
});
```
This returns ` "I'm a damn good friend" ` in the console. I've declared a variable stating that it's true that I bring my friend soup, then since that is true, we call `resolve()` which lets everyone know I am a damn good friend. 

This is a pretty one-dimensional promise though, we don't take into consideration the after-effects of said soup-delivery. For instance, what happens if I have an accident and can't deliver the soup, or how will my friend feel after it's delivered?

We can easily represent this with promises as well, thanks to chaining with `.catch()` and `.then()` which are methods that wil create new promise objects when called, allowing us to easily chain them together.

```javascript
const goodFriend = new Promise(function(resolve, reject) {)
  .then(handleFulfilledA)
  .then(handleFulfilledB)
  .then(handleFulfilledC)
  .catch(handleRejectedAny)
}
```

Back to soup. We can chain a `.then()` method to the end of it to take into consideration other factors, like how my friend will feel after said soup is delivered. Like so:

```javascript
const goodFriend = new Promise(function(resolve, reject) {
  let bringsFriendSoup = true;
  if (bringsFriendSoup) {
    resolve(console.log("I'm a damn good friend"));
  } else {
    reject(console.log("put yourself in the toilet, you garbage friend"));
  }
});
// goodFriend is now setted
goodFriend.then(function(resolve){
  let friendGotSoup = true;
  if (friendGotSoup) {
    resolve(console.log("Friend is happy"));
  }
});
```
So once it's settled that I delivered the soup and am therefore a good friend, `goodFriend.then()` gets called and runs another very simple function that confirms that since my friend got the soup they are happy, and therefore I'm happy. Damn, soup rules.

Also I did not use the reject parameter in this function since I did not create a rejection method.

Let's add another variable, say the road between our houses has a pothole the size of a small horse, and if it hasn't been patched I'm going to hit it and spill soup everywhere. As well as a `.catch()` method, that will get run when the original promise returns with it's rejection. 

```javascript
const goodFriend = new Promise(function(resolve, reject) {
  let bringsFriendSoup = true;
  let potHolePatched = false;
  if (bringsFriendSoup && potHolePatched === true) {
    resolve(console.log("I'm a damn good friend"));
  } else {
    reject(console.log("put yourself in the toilet, you garbage friend"));
  }
});

// goodFriend is settled
goodFriend.then(function(resolve){
  let friendGotSoup = true;
  if (friendGotSoup) {
    resolve(console.log("Friend is happy"));
  }
});

// goodFriend is not settled
goodFriend.catch(function(resolve){
  let friendGotSoup = false;
  if (friendGotSoup === false) {
    resolve(console.log("Friend is still sick and sad"));
  }
});
```
This now returns the fact that I'm a garbage friend since I couldn't come through for my friend after promising them something. It spits out the reject console.log and does _not_ move on to `goodFriend.then()` but instead moves to `goodFriend.catch()` and let's me know that not only do I suck, but my friend is still sick.

That was a super ultra-simplified of Promises, and maybe too soup related, so here are much better references:

-[MDN Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

-[javascript.info](https://javascript.info/promise-basics)

-[Kyle Simpson's You Don't Know JS](https://rileygelwicks.gitbooks.io/you-dont-know-js/content/async%20&%20performance/ch3.html)