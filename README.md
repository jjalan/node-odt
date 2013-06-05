
# node-odt

A node js tool to work with OpenDocument text files.

## Install

```
  $ npm install odt
```

## Usage

```js
var fs = require('fs')
  , odt = require('odt')
  , template = odt.template
  , createWriteStream = fs.createWriteStream
var doc = 'mytemplate.ott';
var values = { 'subject': 'My subject value' };

// apply values

template(doc)
  .apply(values)
  .on('error', function(err){
    throw err;
  })
  .on('end', function(doc){

    // write archive to disk.

    doc.pipe(createWriteStream('mydocument.odt'))
    doc.finalize(function(err){
      if (err) throw err;
      console.log('document written!');
    });
  });
```

For a more advanced example see the command line utility in `bin/node-odt`.

## API

### `Template`

The main class to work with templates.  It inherits from `EventEmitter` and
fires the following events:

#### `events`

 * `error` - Fired if an error occurs.
 * `end(document)` - Fired when the document is complete.

#### `.apply(values : Object)`

Applies the values to the template.  `values` is an object of the following
form:

```js
{
  "field-type": {
    "field-name": "field-value"
  }
}
```

e.g.

```js
{
  "string": {
    "subject": "My subject",
    ...
  },
  ...
}
```