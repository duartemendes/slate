# Upstream types

## Serve

```javascript
{
  "type": "serve",
  "options": {
    "host": "www.sapo.pt",
    "port": 80,
    "timeout": 6000,
    "headers": {
      "host": "www.sapo.pt"
    },
    "rewrite": {
      "expression": "(.*)",
      "to": "/noticias/"
    },
    "cookies": {
      "flavour": { "maxAge": 60000, "domain": "localhost", "path": "/", "value": "chocolate" }
    }
  }
}
```

It proxies the traffic to a non-certified host.

Host and port must be defined.

### Optional configuration

**timeout** - sets the proxy timeout

**headers** - adds and overrides existing ones to be sent upstream, meaning that these headers will be sent to the server where the request is being proxied to

**rewrite** - rewrites the path of the request by providing a regular expression "expression" and a replacement string "to". In the given example the url will be what the user has requested but it will always respond for the "/noticias/" path

**cookies** - adds and overrides existing ones to be sent both for upstream and downstream, meaning that these cookies will be sent to the server where the request is being proxied to and it will also be included in the response (potentially overriding a cookie being set by the upstream server)

## Serve secure

```javascript
{
  "type": "serveSecure",
  "options": {
    "host": "www.mindera.com",
    "port": 443,
  }
}
```

It proxies the traffic to a certified host.

Host and port must also be defined.

<br>

**Optional configuration** is extended from the [serve](#serve) type.

## Redirect

```javascript
{
  "type": "redirect",
  "options": {
    "location": "https://mindera.com/blog",
    "statusCode": 302
  }
}
```

It responds to the original request with a redirect response containing the given location and status code.

In this case, when a request to localhost is made, the user will be redirected to "https://mindera.com/blog".

## Serve file

```javascript
{
  "type": "serveFile",
  "options": {
    "path": "etc",
    "file": "serve_me.jpg",
    "download": false,
    "encoding": ""
  }
}
```

It serves a file, that easy.

### Options

**path** - path of file

**file** - file name and extension

**download** - flag indicating if the file will be showed or downloaded - default is false

**encoding** - indicates the encoding used to read the file when download is set to true (example, set "utf8" when file is txt) - default is empty

<br>
Only path and file properties are required.
