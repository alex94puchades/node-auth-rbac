# auth-rbac

RBAC-based authorization library for [node](http://nodejs.org/)

[![Build Status](https://travis-ci.org/alex94puchades/auth-rbac.svg?branch=master)](https://travis-ci.org/alex94puchades/auth-rbac)

## Installation

```bash
$ npm install auth-rbac
```

## Sample usage

```js
var rbac = require('auth-rbac');
rbac.mongoose = require('auth-rbac-mongoose');
rbac.httpBasic = require('auth-rbac-http-basic');

var User = require('./models/users');
var Group = require('./models/groups');

var auth = rbac.mongoose({
	userModel: User,
	roleModel: Group
});

var express = require('express');
var app = express();

app.use(rbac.httpBasic(auth, 'example'));

app.get('/resources', rbac.requirePrivilege(auth, 'resource-list', function(req, res) {
	res.send('Access granted');
}));
```

**Note:** At this moment, some of the plugins are not finished (yet), so you can use the [raw interface](#raw-interface-for-plugin-developers) instead.

## Tips

For a better experience, you should make use of auth-rbac plugins, such as:

* [auth-rbac-http-basic](https://github.com/alex94puchades/auth-rbac-http-auth)
* [auth-rbac-mongoose](https://github.com/alex94puchades/auth-rbac-mongoose) (not available yet)

You are enticed to contribute with your own plugins. If you do so, make me know so that I can list it here with the others.

## Raw interface (for plugin developers)

```js
var auth = rbac({
	authenticateUser: function(creds, cb) {
		// return user info or null
	},
	
	userGetRole: function(user, cb) {
		// return user role
	},
	
	roleHasPrivilege: function(role, priv, cb) {
		// return whether role has privilege
	}
});
```

```js
app.use(rbac.authenticate({
	credentialsGiven: function(req) {
		// return whether request contains credentials
	},
	
	extractCredentials: function(req) {
		// return credentials in request
	},
	
	askForCredentials: function(res) {
		// ask for credentials, ie: res.sendStatus(401)
	}
}));
```
