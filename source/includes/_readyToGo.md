# Ready to go configuration

```javascript
{
  "api": {
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
  	  "logSlowRequest": true,
  	  "slowRequestThreshold": 2500
  	}
  },
  "bunyan": {
    "name": "traffic-splitter",
    "streams": []
  },
  "browserId": {
    "cookie": "bid",
    "maxAge": 315360000,
    "length": 12
  },
  "pathRegExp": {
    "prefix": "^",
    "sufix": "([/?].*)?$"
  },
  "upstreams": [
    // your upstreams in here
  ]
}
```

Use this configuration sample as you want.

Add some upstreams as seen in [here](#upstreams) to the upstreams array.

After that you're ready to rock and split!
