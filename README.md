# Promised based multipart form parser


[![NPM version][npm-image]][npm-url]
[![build status][travis-image]][travis-url]
[![Test coverage][codecov-image]][codecov-url]
[![npm download][download-image]][download-url]

[npm-image]: https://img.shields.io/npm/v/async-busboy.svg?style=flat-square
[npm-url]: https://npmjs.org/package/async-busboy
[travis-image]: https://img.shields.io/travis/m4nuC/async-busboy.svg?style=flat-square
[travis-url]: https://travis-ci.org/m4nuC/async-busboy
[codecov-image]: https://codecov.io/github/m4nuC/async-busboy/coverage.svg?branch=master
[codecov-url]: https://codecov.io/github/m4nuC/async-busboy?branch=master
[download-image]: https://img.shields.io/npm/dm/async-busboy.svg?style=flat-square
[download-url]: https://npmjs.org/package/async-busboy


Promised based multipart form parser. Parsing logic relies on [busboy](http://github.com/mscdex/busboy), mainly inspired by [co-busboy](http://github.com/cojs/busboy). Ideal for async/await and koa2.

As of today there is no support for direct piping of the request stream. The files are first written to disk (using os.tmpDir()). Not when the consumer stream drained the request stream, file will be automatically removed.


## Examples

### Async/Await
```js
import asyncBusboy from 'async-busboy';

// Koa 2 middleware
async function(ctx, next) {
  const {files, fields} = await asyncBusboy(ctx.req);

  // Make some validation on the fields before upload to S3
  if ( checkFiles(fields) ) {
    files.map(uploadFilesToS3)
  } else {
    return 'error';
  }
}
```

### ES5
```js
var asyncBusboy = require('async-busboy');

function(someHTTPRequest) {
  multiparter(someHTTPRequest).then(function(formData) {
    // do something with formData.files
    // do someting with formData.fields
  });
}
```

### Use cases:

- Form sending only octet-stream (files)

- Form sending file octet-stream (files) and input fields.
  a. File and fields are processed has they arrive. Their order do not matter.
  b. Fields must be processed (for example validated) before processing the files.