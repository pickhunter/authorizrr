Authorizrr
===================
Expressive Authorization middlewares for NodeJS/ExpressJS/ConnectJS apps

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

authorizrr.configure({
  // Let Authorizrr know about current user
  user: function(req) {
    // Return user per request
    return { role: req.role };
  }),

  // Decide current Users abilities
  // Pass a function which receives current user and a 'can' function
  // use the 'can' function to attach abilities to current user
  abilities: function(user, can) {
    if(user.role == 'beardsmen') {
      can('manage', 'beardsmen');
    }
  });

app.get('/', function (req, res) {
  res.json({ unprotected: true });
});
  
app.get('/beardsmen', authorizrr.authorize('manage', 'beardsmen'), function(req, res, next) {
  res.json({ authorized: true })
});

app.listen(3000);
```


----------
## Configuration / Options ##
Authorizrr is configured by calling `configure` with the `options` object on the `authorizrr` singleton. Following options are availble:


| Option | Type |Required / Optional | Description |
| ------ | ---- |------------------- | ----------- |
| user | `function(req)` | Required | function to extract user from the `request` object
| abilities | `function(user, can)` | Required  | function to attach abilities on the current user |
| onAuthFail | `function(req, res, next)` | Optional | Authorizrr sends a status of `403` on authentication failure. You can use this function to override the behavior completely and construct/send your own response |
