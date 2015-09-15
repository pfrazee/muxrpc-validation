# muxrpc validation

Validation library for [muxrpc](https://npm.im/muxrpc) apis.

```js
var valid = require('muxrpc-validation')

var manifest = {
  usage: 'sync',
  get:   'async',
  add:   'async',
  list:  'source'
}

// wrap the functions in the validators
var api = {
  usage:   valid.sync(usage, 'string|boolean'), // multiple types
  get:     valid.async(get, 'string'),
  add:     valid.async(add, ['string'], ['string', 'object']), // multiple signatures
  list:    valid.source(list, 'queryOpts?') // optional param
}

// register special validators
valid.set('queryOpts', function (v) {
  if (v.reverse && typeof v.reverse != 'boolean')
    return new TypeError('opts.reverse must be a bool')
})

// function defs:
function usage (cmd) {
  // ...
}
function get (key, cb) {
  // ...
}
function add (key, value, cb) {
  if (typeof value == 'function') {
    // handle (value, cb)
    value = key
    key = value.key
    cb = value
  }
  // ...
}
function list (opts) {
  opts = opts || {}
  // ...
}
```