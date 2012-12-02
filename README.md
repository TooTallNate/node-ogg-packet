node-ogg-packet
===============
### Manually construct `ogg_packet` struct instances

This module lets you construct your own `ogg_packet` struct instances using
JavaScript and Buffers. You'll most likely not need to use this module for any
practical purposes, but it is useful for testing purposes.

The more common way to get _proper_ `ogg_packet` structs is via a decoded OGG file
and node-ogg's `ogg.Decoder` class, or one of the codec's encoder classes like
node-vorbis' `vorbis.Encoder` class.


Installation
------------

``` bash
$ npm install ogg-packet
```


Example
-------

``` javascript
var ogg_packet = require('ogg-packet');

var decoder = new ogg.Decoder();
decoder.on('stream', function (stream) {
  console.log('new "stream":', stream.serialno);

  // emitted upon the first packet of the stream
  stream.on('bof', function () {
    console.log('got "bof":', stream.serialno);
  });

  // emitted for each `ogg_packet` instance in the stream.
  // note that this is an *asynchronous* event!
  stream.on('packet', function (packet, done) {
    console.log('got "packet":', packet.packetno);
    done();
  });

  // emitted after the last packet of the stream
  stream.on('eof', function () {
    console.log('got "eof":', stream.serialno);
  });
});

// pipe the ogg file to the Decoder
fs.createReadStream(file).pipe(decoder);
```

See the `examples` directory for some more example code.


API
---

### ogg_packet class

A `ref-struct` class that mirrors the `ogg_packet` fields in the `ogg.h` file.

``` c
typedef struct {
  unsigned char *packet;
  long  bytes;
  long  b_o_s;
  long  e_o_s;
  ogg_int64_t  granulepos;
  ogg_int64_t  packetno;
} ogg_packet;
 ```
