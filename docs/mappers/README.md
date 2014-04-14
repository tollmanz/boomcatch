# Mappers

Mappers are the third type of extension point,
called after validators and filters
but before forwarders.
Their purpose is to transform beacon data
into an appropriate format
to be consumed by some other process.

The choice of mapper
can be specified at the command line,
using the `-m` [option].

Two mappers are availble out-of-the-box,
[statsd] and [har].

Defining custom mappers is simple.
The [source code for the statsd mapper][src]
should be easy to follow,
but the basic pattern
is to export an interface
that looks like this:

```javscript
{
    initialise: function (options) {
    }
}
```

Where `initialise` is a function
that takes an options object
and returns the mapper function,
bound to any pertinent options.
The signature for
the returned mapper function
should look like this:

```javascript
function (/* bound options, ... */ data, referer, userAgent, remoteAddress) {
}
```

The remaining/unbound arguments
passed to the mapper
are, in order:

1. `data`:
   Beacon data,
   in the boomcatch [normalised format][format].

2. `referer`:
   URL of the page
   that sent the beacon request.

3. `userAgent`:
   User agent string of the client
   that sent the beacon request.

4. `remoteAddress`:
   IP address of the client
   that sent the beacon request.

[option]: ../../README.md#from-the-command-line
[statsd]: statsd.md
[har]: har.md
[src]: ../../src/mappers/statsd.js
[format]: ../data.md
