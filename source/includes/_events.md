# Events

```javascript
const TrafficSplitter = require('traffic-splitter')
const splitter = new TrafficSplitter(config)
const log = splitter.getLogger()
```

During its life splitter emits several events which you can listen to.


## applicationStart

```javascript
splitter.events.on('applicationStart', () => {
  log.info('Application has started')
})
```

Emitted when splitter.start() is called, which is after optimizing the given configuration.

## serverStart

```javascript
splitter.events.on('serverStart', () => {
  log.info('Server has started')
})
```

Triggered after server is configured and started, and also after executing [bootstrap functions](#bootstrap).

## rulesProcessing

```javascript
splitter.events.on('rulesProcessing', (duration, selectedUpstream) => {
  log.info(`Rules processed in ${duration} milliseconds`)
  log.info(`Selected upstream =  ${selectedUpstream}`)
})
```

Called each time a request is made.

It tells you the time it took to calculate the upstream (in milliseconds) and also the selected upstream name.

## noUpstreamFound

```javascript
splitter.events.on('noUpstreamFound', (req) => {
  log.info(`No upstream found for request ${req}`)
})
```

Only emitted when no upstream matched the given request.

## resFinish

```javascript
splitter.events.on('resFinish', (req, res, duration) => {
  log.info(`Response finished in ${duration} milliseconds`)
})
```

Triggered every time a response is sent to the final user.

## serving

```javascript
splitter.events.on('serving', (statusCode, upstream, duration, host) => {
  log.info(`${host} took ${duration} milliseconds to respond with ${statusCode} HTTP code`)
})
```

Called each time an upstream response arrives (only for serve and serveSecure types).

## servingError

```javascript
splitter.events.on('servingError', (err, upstream, duration) => {
  log.info(`Error while serving upstream '${upstream.name}':`, err)
})
```

Triggered when an upstream responds with an error.

## upstreamException

```javascript
splitter.events.on('upstreamException', (exception, upstream) => {
  log.info(`Upstream (${upstream.name}) exception: ${exception}`)
})
```

Emitted when an error is catched while executing an upstream.

## httpSocketMetrics

```javascript
splitter.events.on('httpSocketMetrics', (agentStatus) => {
  log.info('HTTP socket metrics: ', agentStatus)
})

// {"name":"traffic-splitter","hostname":"unknown","pid":14328,"level":30,"msg":"HTTP socket metrics:  { createSocketCount: 0,\n  createSocketErrorCount: 0,\n  closeSocketCount: 0,\n  errorSocketCount: 0,\n  timeoutSocketCount: 0,\n  requestCount: 0,\n  freeSockets: {},\n  sockets: {},\n  requests: {} }","time":"2017-06-03T19:50:33.799Z","v":0}
```

Triggered each [emitMetricsInterval.http](#api) milliseconds when at least one serve upstream type is present in the configuration.

It gives you the [agentkeepalive](https://www.npmjs.com/package/agentkeepalive) instance current status.

## httpsSocketMetrics

```javascript
splitter.events.on('httpsSocketMetrics', (agentStatus) => {
  log.info('HTTPS socket metrics: ', agentStatus)
})
```

Works the same way [httpSocketMetrics](#httpsocketmetrics) does but for the serveSecure upstream type.

## redirecting

```javascript
splitter.events.on('redirecting', (statusCode, upstream, duration) => {
  log.info(`Redirecting to upstream '${upstream.name}' took ${duration} milliseconds with the HTTP code ${statusCode}`)
})
```

Emitted every time a request is redirected (consequence of the [redirect](#redirect) upstream type).
