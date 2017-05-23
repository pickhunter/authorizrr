Authorizrr
===================
Expressive Authorization middlewares for Express apps

> **Note:** This is for *Authorization* **NOT Authentication**

## Installation ##
```sh
npm install authorizrr --save
```
## Quick Start ##
```js
var express = require('express');
var app = express();
var authorizrr = require('authorizrr');

// Let Authorizrr know about current user
authorizrr.configure.userBy((function(req) {
  // Return user per request
  return { role: req.role };
});

// Decide current Users abilities
// Pass a function which receives current user and a 'can' function
// use the 'can' function to attach abilities to current user
authorizrr.configure.abilitiesBy(function(user, can) {
  if(user.role == 'beardsmen') {
    can('manage', 'beardsmen');
  }
});

app.get('/', function (req, res) {
  res.json({ public: true });
});
  
app.get('/beardsmen', authorizrr.authorize('manage', 'beardsmen'), function(req, res, next) {
  res.json({ authorized: true })
});

app.listen(3000);
```
