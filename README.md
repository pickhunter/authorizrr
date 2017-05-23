Authorizr
===================
Expressive Authorization middlewares for NodeJS/ExpressJS/ConnectJS apps

> **Note:** This is for *Authorization* **NOT Authentication**

## Installation ##
```sh
npm install authorizr --save
```
## Quick Start ##
```js
var express = require('express');
var app = express();
var authorizr = require('authorizr');

// Let Authorizr know about current user
authorizr.configure.userBy((function(req) {
  // Return user per request
  return { role: req.role };
});

// Decide current Users abilities
// Pass a function which receives current user and a 'can' function
// use the 'can' function to attach abilities to current user
authorizr.configure.abilitiesBy(function(user, can) {
  if(user.role == 'beardsmen') {
    can('manage', 'beardsmen');
  }
});

app.get('/', function (req, res) {
  res.json({ public: true });
});
  
app.get('/beardsmen', authorizr.authorize('manage', 'beardsmen'), function(req, res, next) {
  res.json({ authorized: true })
});

app.listen(3000);
```
