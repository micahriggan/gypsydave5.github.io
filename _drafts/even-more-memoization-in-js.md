---
layout: post
title: "(Even More) Memoization in JavaScript"
date: 2015-11-17 20:08:44
tags:
    - JavaScript
    - Mergermarket
    - Learnings
    - Functional Programming
published: false
---

A while ago I wrote a piece on [basic lazy evaluation and memoization in
JavaScript][firstBlog]. I'd like to continue some of the thoughts there on
memoization, and also show how to do the same thing but without all the
implementation details using some popular functional libraries.

### Memoization refresher

So the first time we saw memoization it was in the context of [lazy
evaluation][firstBlog], where the memoized function had already been 'prepped'
to be lazy. The lazily evaluated function was executed within our memoizer, and
the result was retained within the scope of the memoized function, to be
returned each subsequent time the function was called. As a refresher:

```javascript
function lazyEvalMemo (fn) {
  const args = arguments;
  let result;
  const lazyEval = fn.bind.apply(fn, args);
  return function () {
    if (result) { return result }
    result = lazyEval()
    return result;
  }
}
```

Which is great if want the function to be memoized with a specific set of
arguments. But that isn't going to work all of the time - probably not even most
of the time. It's rare that there's just one set of parameters we want to be
eternally bound to our function. A bit of freedom would be nice.

### Keeping track of your arguments

If we want to be able to memoize a function which can take a variety of
arguments, we need a way to keep track of them. Let's take one of the simplest
cases:

```javascript
function addOne (x) {
  return x + 1;
};
```

That shouldn't take too much explanation. Now, in order to have a memoized
version of it, we need to keep track of all the different values of `x` passed
to it, and to associate them with the return value of `addOne`.

A JavaScript object should suffice, taking `x` as a key, and the result as the
value. Let's give it a go:

```javascript

const memo = {};

function memoAddOne (x) {
  if (memo[x]) { return memo[x]; }
  return memo[x] = x + 1;
};
```

This takes advantage of the fact that the return value of an assignment is the
value assigned (in this case `x + 1`), saving a line or so. The only issue here
is that the vaue of `memo` is floating around in public, just waiting for
unscrupulous other functions to overwrite and mutate. We need a way to hide it.

Well, we could place `memo` on the function itself - it is an object after all
- and just put an underscore in front of its name to let the world know that
it's private (even though it isn't really private):

```javascript
function memoAddOne (x) {
  memoAddOne._memo = memoAddOne._memo || {};

  if (memoAddOne._memo[x]) { return memoAddOne._memo[x]; }
  return memoAddOne._memo[x] = x + 1;
};
```

This isn't beautiful, but it works. `_memo` gets defined as an empty object on
initialization and gets filled up with results on each application of the
function - throw some `console.log()`s in there to prove it to yourself. That's
exciting - although we're still a little exposed with the `_memo` property being
available on the function.

That said, we've got what we came for - a memoized version of our function that
works for many different arguments.

### Fun with strings

Problem is, we're reliant on `x` being used as the property for our `_memo`
object. As all good school children are taught, JavaScript, like the universe,
is a collection of strings held together by poorly-understood forces. When `x`
is used in `_memo[x]`, JavaScript handily casts it to a string to be used as the
property name.

I say handily - but try this...

```javascript
memoAddOne([55]) // => '551'
memoAddOne(55) // => '551'
memoAddOne(66) // => 67
memoAddOne([66]) // => 67
```

Because...

```javascript
55.toString() // => '55'
[55].toString() // => '55'
```

Ah, JavaScript: thou givest with one hand... `toString()`, which is what
JavaScript is using under the hood to cast the non-string property identifier to
a string, does not uniquely identify that argument. So our function behaves
inconsistently depending on whether it was given the array or the number that
`toString()` converts to the same string.

We need a more predictable way of parsing our argument. Happily, we can borrow
one of the built-in libraries of NodeJS to do this, `JSON.stringify()`.

```javascript
JSON.stringify(55) // => '55'
JSON.stringify('55') // => '"55"'
JSON.stringify([55]) // => '[55]'
JSON.stringify(['55']) // => '["55"]'
```

Pretty good - let's give it a whirl:

```javascript
function memoAddOne (x) {
  memoAddOne._memo = memoAddOne._memo || {};
  const jsonX = JSON.stringify(x);

  if (memoAddOne._memo[jsonX]) { return memoAddOne._memo[jsonX]; }
  return memoAddOne._memo[jsonX] = x + 1;
};

memoAddOne([55]) // => '551'
memoAddOne(55) // => 56
```

Sorted.

### The General Case

Now let's put together a function that can memoize _any_ function - and as
a bonus, we can also hide that nasty `_memo` property behing a closure:

```javascript
function memoize (fn) {
  const memo = {};

  return function () {
    const args = Array.prototype.slice.call(arguments);
    const jsonArgs = JSON.stringify(args);

    if (memo[jsonArgs]) { return memo[jsonArgs]; }
    return memo[jsonArgs] = fn.apply(null, args);
  };
};
```

Much of this should now be familiar, but let's dig in. We take a single
argument, hopefully a function, and bind it to the variable `fn`. We now get to
declare `memo` inside our function - and hooray it's now unavailable to anyone
but the function we're returning. Now that's what I call private.

The function we give back, well we don't know how many arguments it's going to
be given so why bind them to any variables. Which is a long winded way of saying
we'll leave its arguments empty. Any arguments we do get we'll instantly turn
into an array by the funky `Array.prototype.slice.call(arguments)` dance. And
that array we can `stringify()` neatly on the next line.

Then we do much the same as we have been above, only instead of giving `x + 1`
to as the value of `_memo[jsonX]`, we set `memo[jsonArgs]` to the result of
applying the `args` array to the original function we were given to memoize. Job
done.

Again, throw some `console.log`s in there to see what's really going on.

### Here's One I <del>Made Earlier</del> Bought from the Supermarket

[firstBlog]: {% post_url 2015-03-21-lazy-eval-and-memo.md %}