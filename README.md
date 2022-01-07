node-telnet
===========
### Telnet implementation for Node.js
[![Build Status](https://secure.travis-ci.org/TooTallNate/node-telnet.png)](http://travis-ci.org/TooTallNate/node-telnet)


This module offers an implementation of the [Telnet Protocol (RFC854)][rfc],
making it possible to write a telnet server that can interact with the various
telnet features.

### Implemented Options:

| **Name**            | **Event**             |**Specification**
|:--------------------|:----------------------|:-------------------------
| Binary transmission | `'transmit binary'`   | [RFC856](http://tools.ietf.org/html/rfc856)
| Echo                | `'echo'`              | [RFC857](http://tools.ietf.org/html/rfc857)
| Suppress Go Ahead   | `'suppress go ahead'` | [RFC858](http://tools.ietf.org/html/rfc858)
| Window Size         | `'window size'`       | [RFC1073](http://tools.ietf.org/html/rfc1073)


Installation
------------

Install with `npm`:

``` bash
$ npm install telnet
```


Examples
--------

``` js
var telnet = require('telnet')

telnet.createServer(function (client) {

  // make unicode characters work properly
  client.do.transmit_binary()

  // make the client emit 'window size' events
  client.do.window_size()

  // listen for the window size events from the client
  client.on('window size', function (e) {
    if (e.command === 'sb') {
      console.log('telnet window resized to %d x %d', e.width, e.height)
    }
  })

  // listen for the actual data from the client
  client.on('data', function (b) {
    client.write(b)
  })

  client.write('\nConnected to Telnet server!\n')

}).listen(23)
```

And then you can connect to your server using `telnet(1)`

``` bash
$ telnet localhost
Trying ::1...
telnet: connect to address ::1: Connection refused
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.

Connected to Telnet server!
```
