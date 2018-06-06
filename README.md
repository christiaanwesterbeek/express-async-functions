# ExpressJS Async Functions

A dead simple ~~ES6 generators and~~ ES7 async/await support hack for [ExpressJS](http://expressjs.com)

Forked from [express-yields](https://github.com/MadRabbit/express-yields) so that
it has just ES7 async/await support and none for ES6 generators. The latter depends on [co](https://github.com/tj/co) which is a relatively heavy install.

## Usage

```
npm install express-async-functions --save
```

Then require this script somewhere __before__ you start using it:

```js
const express = require('express');
require('express-async-functions');
const User = require('./models/user');
const app = express();

app.get('/users', async (req, res) => {
  const users = await User.findAll(); // <- some Promise
  res.send(users);
});
```

## A Notice About Calling `next`

As we all know express sends a function called `next` into the middleware, which
then needs to be called with or without error to make it move the request handling
to the next middleware. If you want to pass an error, just throw a normal exception:

```js
app.use(async (req, res) => {
  const user = await User.findByToken(req.get('authorization'));

  if (!user) throw Error("access denied");
});
```

## How Does This Work?

This is a minimalistic and unobtrusive hack. Instead of patching all methods
on an express `Router`, it wraps the `Layer#handle` property in one place, leaving
all the rest of the express guts intact.

The idea is that you require the patch once and then use the `'express'` lib the
usual way in the rest of your application.

## Copyright & License

What? I forked this from [express-yields](https://github.com/MadRabbit/express-yields). And removed the generators stuff. Dunno.
