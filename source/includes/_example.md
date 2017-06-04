# Example

```javascript
[
  {
    "name": "mindera.com 0-50",
    "enabled": true,
    "criteria": {
      "in": {
        "bucket": [{ "min": 0,  "max": 50 }]
      },
      "out": {
         "cookie": [{ "name": "keepme", "value": "out" }]
      }
    },
    "upstream": {
      "type": "serveSecure",
      "options": { "host": "www.mindera.com", "port": 443, "headers": { "host": "www.mindera.com" } }
    }
  },
  {
    "name": "npmjs.com 51-100",
    "enabled": true,
    "criteria": {
      "in": {
        "bucket": [{ "min": 51,  "max": 100 }]
      },
      "out": {}
    },
    "upstream": {
      "type": "serveSecure",
      "options": { "host": "www.npmjs.com", "port": 443, "headers": { "host": "www.npmjs.com" } }
    }
  }
]
```

This upstreams example splits the traffic between mindera.com and npmjs.com, each one will handle 50% of the traffic.

<br>

**The object "in" inside the criteria contains all the criteria (don't worry, you'll learn all about this in a few moments) that need to be respected in order for that upstream to be selected. The same goes for the "out" object but instead, it eliminates that upstream from being chosen.**

<br>

For instance, if the user has a cookie named "keepme" with the value "out", he won't ever be served with the mindera's upstream. Why? Because mindera's upstream has a cookie rule (that matches) inside the "out" object.

<br>

Also, upstreams can be enabled or disabled by changing the "enabled" property.

<br>

"Wow.. this is the real thing! What other type of upstreams are available?"

Just scroll down a bit and you'll find out :p
