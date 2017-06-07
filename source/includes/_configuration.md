# Configuration

```javascript
// load from json file:
const config = JSON.parse(require('fs').readFileSync('./conf/config.json'))

// or declare it normally
const config = {
  // ...
}
```

Configuration is an object with several properties (listed below) that will define the splitter behaviour.

Before the server starts, **splitter optimizes the configuration by changing some properties and values** so that the conditions in runtime are faster, mainly when evaluating rules.

You are able to see how your configuration will be with the [getConfiguration](#getconfiguration) method.

Look at a minimum configuration example in [here](#ready-to-go-configuration).

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

This property is related to the restify instance and agentkeepalive configuration, as well as other values for manipulation of logs and events.

### Properties

**serverName** - restify instance name

**port** - port for the server: localhost:port

**maxConnections** - server maximum connections. Default is 256

**upstreamKeepAlive** - configuration for the [agentkeepalive](#https://www.npmjs.com/package/agentkeepalive) instance

**emitMetricsInterval** - interval in milliseconds to emit the [httpSocketMetrics](#httpsocketmetrics) and [httpsSocketMetrics](#httpssocketmetrics) events. Default is 5000

**performance** - "logSlowRequests" is a flag to log (or not) slow requests and "slowRequestThreshold" its the boundary in which a request is considered slow. Example: given the configuration at right, if a request takes 900 milliseconds then is not a slow request, but if it takes 1000 or more milliseconds it is now considered a slow request

<br>

This is a required property.

## bunyan
```javascript
{
  "name": "traffic-splitter",
  "streams": []
}
```

Splitter uses the [bunyan](https://www.npmjs.com/package/bunyan) library for logging.

Provide a name for it and 0 or more streams.

In case no streams are provided the splitter will add the stream: { level: 'info', stream: process.stdout }

<br>

This is a required property.

## browserId
```javascript
{
  "cookie": "bid",
  "maxAge": 315360000,
  "length": 12
}
```

This property defines the configuration for the browserId cookie emitted by the splitter for [bucketing](#bucket) purposes.

<br>

This is a required property.

## domains
```javascript
[
  "www.trafficsplitter.io",
  "www.mindera.com"
]
```

Provide a list for the domains that you want to enable splitter to add the [CORS](#cors) headers.

<br>

This is an optional property.

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

Configuration for the CORS headers in case the host belongs to the [domains](#domains) list.

<br>

This is an optional property.

## pathRegExp
```javascript
{
  "prefix": "^",
  "sufix": "([/?].*)?$"
}
```

This property is a plus for the [path](#path) criteria. The example at right is the **recommend** definition for this property.

The [path](#path) criteria is turned into a regex when splitter optimizes the configuration and this object allows you to set a prefix and sufix to that regex for every path. Let's see why this is useful.

Let's make an example and assume that the path criteria is ["/banana"].

```javascript
const expression1 = new RegExp("/banana", "i")
const condition1 = request.url.match(expression1)

const expression2 = new RegExp("^" + "/banana" + "([/?].*)?$", "i")
const condition2 = request.url.match(expression2)
```

Also assume the defined variables at right.

| request.url | condition1 (without pathRegExp)  | condition2 (with pathRegExp) |
|:----------- |:-------------------:|:---------------:|
| /apple | ![fail](images/fail.png) | ![fail](images/fail.png) |
| /banana | ![success](images/success.png) | ![success](images/success.png) |
| /bananas | ![success](images/success.png) | ![fail](images/fail.png) |
| /bananas/1 | ![success](images/success.png) | ![fail](images/fail.png) |
| /bananas?id=1 | ![success](images/success.png) | ![fail](images/fail.png) |
| /banana/ | ![success](images/success.png) | ![success](images/success.png) |
| /banana/1 | ![success](images/success.png) | ![success](images/success.png) |
| /banana?id=1 | ![success](images/success.png) | ![success](images/success.png) |
([run this test](https://jsfiddle.net/uqyo484v/1/))

<br>

We believe that the correct behaviour is achieved with the condition2 (with pathRegExp), but do what best suits your needs, really :)

<br>

This is an optional property.

## rulesets
```javascript
{
  "myRuleset1": {
    "cookie": [{ "name": "step", "value": "in" }],
    "path": ["/blog", "/people-and-culture", "/case-studie"],
    "agent": ["Chrome", "Safari"]
  }
}

// inside some upstream
in: {
  ruleset: ["myRuleset1"],
  bucket: [{ "min": 0, "max": 50 }]
}

// inside some other upstream
in: {
  ruleset: ["myRuleset1"],
  bucket: [{ "min": 51, "max": 100 }]
}
```

This is one hell of a property! Its very useful to avoid repetition of criterias accross the upstreams.

Picture yourself having two upstreams with 10 criterias each (for the 'in' part). And in those 10, only one of them is different. Quite boring and repetitive right? Well, just create a ruleset with whatever name you want containing all those 9 equal criteria. And then, back in each upstream delete the criteria that is repeated and add the criteria [ruleset](#ruleset).

Just check the example at right.

<br>

### Notes:
* You can have multiple rulesets for each upstream

* If an upstream has a criteria that is also defined in a ruleset used by the same upstream, the one that's taking effect is the one that was last declared

<br>

This is an optional property.

## upstreams
```javascript
[
  {
    "name": "mindera <3",
    "enabled": true,
    "criteria": {
      "in": {
        // your criteria to allow access in here
      },
      "out": {
        // your criteria to deny access in here
      }
    },
    "upstream": {
      "type": "serveSecure",
      "options": {
        "host": "www.mindera.com",
        "port": 443,
        "headers": {
          "host": "www.mindera.com"
        }
      }
    }
  }
]
```

This is the most interesting part :D

It's this property that contains the array with all the upstreams that you want your splitter to use, just like in the [example](#example).

<br>

This is a required property.