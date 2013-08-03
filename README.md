
# Why Care?

Web applications, and single page applications in particular, make it
hard to see errors that are happening in client side JavaScript.

# What It Does

Flinger flings logs from your browser clients back to your node servers
so you can see what your users are up to.

# What It Is

Flinger is node _middleware_ for [express](https://github.com/visionmedia/express) or [connect](https://github.com/senchalabs/connect) that does two things:

* For the client, serves a client library that monkey patches
    * `console.log`
    * `console.error`
    * `Error`
* For the server, provides a receiver that catches and logs client logs

# How To Use It

**Flinger uses jQuery for HTTP back to the server**. Make sure it is in
your page.

Here is the most basic installation possible:

```javascript
var express = require('connect');
var app = connect()
    .use(connect.cookieParser())
    .use(connect.static(path.join(__dirname, 'client')))
    .use(flinger())
    .listen(9999);
```

Flinger serves its client library automatically as a convenience, so on
the client:

```html
<script type="text/javascript" src="/flinger.js"></script>
```

This redirects, by default:

* client `console.log(...)` to server `console.log(...)`
* client `console.error(...)` to server `console.error(...)`
* client `new Error(...)` to server `console.error(...)`

## Fancy Server Use

Flinger lets you hook to reformat or log as you see fit, flinger really
is:

`flinger(onConsoleLog, onConsoleError, onException)`

Each of the `onXXX` functions is:

`function handler(logEvent){}`

Each `logEvent` is:

* `request`, flinger logs over HTTP, so you can get at cookies etc to
  identify users and make custom logs
* `arguments`, the javascript `arguments` captured on the client
  function

