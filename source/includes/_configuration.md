# Configuration
## api
```javascript
{
  "serverName": "Traffic Splitter",
  "port": 80,
  "maxConnections": 1024,
  "upstreamKeepAlive": {
    "maxSockets": 1024,
    "maxFreeSockets": 64,
    "timeout": 60000,
    "keepAliveTimeout": 30000
  },
  "emitMetricsInterval": {
    "http": 10000,
    "https": 10000
  },
  "performance": {
    "logSlowRequests": true,
    "slowRequestThreshold": 1000
  }
}
```

## bunyan
```javascript
{
  "name": "traffic-splitter",
  "streams": []
}
```

## browserId
```javascript
{
  "cookie": "bid",
  "maxAge": 315360000,
  "length": 12
}
```

## domains
```javascript
[
  "www.trafficsplitter.io",
  "www.mindera.com"
]
```

## cors
```javascript
{
  "headers": [
      "ORIGIN",
      "X_REQUESTED_WITH",
      "X-Requested-with",
      "Content-Type",
      "Accept"
  ],
  "credentials": "true",
  "methods": [
      "GET",
      "POST",
      "DELETE",
      "OPTIONS",
      "PUT"
  ],
  "max-age": "600"
}
```

## pathRegExp
```javascript
{
  "prefix": "^",
  "sufix": "([/?].*)?$"
}
```

## rulesets
```javascript
{
  "first": {
    "cookie": [
      {
        "name": "step",
        "value": "in"
      }
    ]
  },
  "second": {
    "cookie": [
      {
        "name": "step",
        "value": "out"
      }
    ]
  }
}
```

## upstreams
```javascript
[
  {
    "name": "paths",
    "enabled": true,
    "criteria": {
      "in": {
        "path": [
          "/teste"
        ]
      }
    },
    "upstream": {
      "type": "serveSecure",
      "options": {
        "host": "www.torn.com",
        "port": 443,
        "headers": {
          "host": "www.torn.com"
        }
      }
    }
  }
]
```
