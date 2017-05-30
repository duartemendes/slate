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

It proxies the traffic to a defined host (non-https).

Host and port must be defined.

<br>

### Optional configuration

**timeout** - fwefw

**headers** - fwefw

**rewrite** - fwefw

**cookies** - fwefw


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

## Redirect

```javascript
{
  "type": "redirect",
  "options": {
    "location": "http://mindera.com/blog",
    "statusCode": 302
  }
}
```
