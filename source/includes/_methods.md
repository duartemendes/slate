# Methods

```javascript
const TrafficSplitter = require('traffic-splitter')
const splitter = new TrafficSplitter(config)
```

Splitter provides some methods to help you take more advantage of its features.

Besides the [isConfigurationValid](#isConfigurationValid) all the methods are at instance level.

## isConfigurationValid

```javascript
if (!TrafficSplitter.isConfigurationValid(myConfig)) {
  throw new Error('My configuration is invalid!')
}
```

This method returns a boolean and tells you if your configuration has the necessary objets and properties to start.

## getLogger

```javascript
const log = splitter.getLogger()
log.info('Gotta love this!')

// {"name":"traffic-splitter","hostname":"unknown","pid":13360,"level":30,"msg":"Gotta love this!","time":"2017-06-01T15:56:05.897Z","v":0}
```

This method is at instance level and it returns its bunyan instance so you can use it.

[Bunyan](https://www.npmjs.com/package/bunyan) is a simple and fast JSON logging library.

## bootstrap

```javascript
splitter.bootstrap((server, config) => {
  // this code will be executed before the server starts
  // do whatever you want/need in here
})
```

This method allows you to run some code before the server starts.

It takes a callback with two parameters:

**server** - restify instance

**config** - optimized configuration

## use

```javascript
splitter.use((server, config) => (req, res, next) => {
  log.info(`Request URL = ${req.url}`)
  return next()
})
```

This method allows you to add middlewares to the server. It will be executed once per request.

It takes the same parameter as the previous method but this callback should return another callback that it will be called by the server.

Inside the callback returned always do "return next()" (or similar) or the request will get stuck in it.

## addRule


```javascript
// somewhere in your upstreams criteria configuration:
"in": { // or "out"
  "myCustomRule1": {
    "even": false
  },
  "host": [
    "myHost.com"
  ]
}

// evaluating custom rule
splitter.addRule('myCustomRule1', (criteria, req) => {
  const currentMinute = new Date().getMinutes()
  const isEven = currentMinute % 2 === 0
  return criteria ? isEven : !isEven
})

// overriding splitter rule
splitter.addRule('host', (criteria, req) => {
   return criteria === 'myHost.com'
})
```

This is maybe the coolest feature of the splitter, it allows you to evaluate your custom rules in a very simplistic way.

<br>

And it also gives you the chance to override any splitter default rule evaluation in case you need to adjust it for your needs.

<br>

It takes two parameters. First is the name of your rule (in the configuration) and second is a callback that takes two parameters as well (first will be the rule object and second the current request). This callback must return a boolean. 

<br>

Note: missing methods to evaluate custom rules or those who won't return a boolean will be ignored.
