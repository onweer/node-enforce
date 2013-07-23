## Data Validations [![Build Status](https://secure.travis-ci.org/dresende/node-enforce.png?branch=master)](http://travis-ci.org/dresende/node-enforce)

This is the package responsible for data validations in [ORM](http://dresende.github.io/node-orm2).

### Enforce

You can create a list of validations for several properties of an `Object` and then run the checks to
see if everything is OK.

```js
var enforce = require("enforce");
var checks  = new enforce.Enforce();

checks
	.add("name", enforce.notEmptyString())
	.add("age", enforce.ranges.number(18, undefined, "under-age"));

checks.check({
	name : "John Doe",
	age  : 16
}, function (err) {
	// err should have an error with "msg" = "under-age"
});
```

### Validators

All validators accept a `msg` argument at the end. These argument is the error message returned if the
validation fails. All validators return a `function` that is called by `Enforce` with the value of the property
in question and a `next` callback.

#### `enforce.required([ msg = "required" ])`

Checks if a property is not `null` and not `undefined`. If can be `false`, `0` or `""`.

#### `enforce.notEmptyString([ msg = "empty-string" ])`

Checks if a property length is not zero. It can actually work with `Array`s.

#### `enforce.lists.inside(Array[, msg = "outside-list" ])`

Checks if the property is inside a list of items (the `Array`).

#### `enforce.lists.outside(Array[, msg = "inside-list" ])`

Checks if the property is not inside a list of items (the `Array`).

#### `enforce.ranges.number(min[, max[, msg = "out-of-range-number" ]])`

Checks if a value is inside a specific range of numbers. Either `min` or `max` can be set to `undefined` meaning
that range side is `Infinity`.

#### `enforce.ranges.length(min[, max[, msg = "out-of-range-length" ]])`

Does the same as the above but for the `length` property.