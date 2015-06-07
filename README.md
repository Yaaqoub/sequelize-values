# sequelize-values.js

# Easily get raw data from Sequelize instances

## What it does

A few utility functions for getting raw data from Sequelize instances. Includes ability to remove duplicate data.

## Current status

[![NPM version](https://img.shields.io/npm/v/sequelize-values.svg)](https://www.npmjs.com/package/sequelize-values)
[![Build Status](https://img.shields.io/travis/overlookmotel/sequelize-values/master.svg)](http://travis-ci.org/overlookmotel/sequelize-values)
[![Dependency Status](https://img.shields.io/david/overlookmotel/sequelize-values.svg)](https://david-dm.org/overlookmotel/sequelize-values)
[![Dev dependency Status](https://img.shields.io/david/dev/overlookmotel/sequelize-values.svg)](https://david-dm.org/overlookmotel/sequelize-values)
[![Coverage Status](https://img.shields.io/coveralls/overlookmotel/sequelize-values/master.svg)](https://coveralls.io/r/overlookmotel/sequelize-values)

## Usage

### Loading module

To load module:

```js
var Sequelize = require('sequelize-values')();
// NB Sequelize must also be present in `node_modules`
```

or, a more verbose form useful if chaining multiple Sequelize plugins:

```js
var Sequelize = require('sequelize');
require('sequelize-values')(Sequelize);
```

### Methods

#### Instance#getValues()

Like Sequelize's native `Instance#values`, except that it recursively calls `getValues()` on any nested values. So if you get an instance from `Model#find()` with eager-loaded associations, the associated Instances returned are also converted to values.

```js
Task.find( { include: [ User ] } )
.then(function(task) {
	return task.getValues();
	// returns the attributes of the Task,
	// with the attributes of the User also converted to values rather than a DAO.
});
```

#### Sequelize.getValues(input)

Same as `Instance#getValues()` except can be called with an `Instance` or array of `Instance`s.

```js
Task.findAll( { include: [ User ] } )
.then(function(tasks) {
	return Sequelize.getValues(tasks);
	// returns the array of Tasks, with each Task converted to plain attributes,
	// with the attributes of the Users also converted to values rather than DAOs.
});
```

#### Instance#getValuesDedup()

Same as `Instance#getValues()`, except removes duplicated data, e.g. removes `id` fields which are returned twice. Rather than:

```js
{ name: 'foo', UserId: 1, User: { id: 1, name: 'Bar' } }
```

you get:

```js
{ name: 'foo', User: { id: 1, name: 'Bar' } }
```

Useful if you want to e.g. send the values of an Instance to the browser without sending unnecessary duplicated data.

#### Sequelize.getValuesDedup(input)

Same as `Sequelize.getValues(input)`, but with data de-duplication.

## Tests

Use `npm test` to run the tests. Use `npm run cover` to check coverage.
Requires a database called 'sequelize_test' and a db user 'sequelize_test' with no password.

## Changelog

See changelog.md

## Issues

If you discover a bug, please raise an issue on Github. https://github.com/overlookmotel/sequelize-values/issues

## Contribution

Pull requests are very welcome. Please:

* ensure all tests pass before submitting PR
* add an entry to changelog
* add tests for new features
* document new functionality/API additions in README
