# Getting started

There are two ways you can use this package and both need to be provided with a configuration.

Personalize your own [configuration](#configuration) or go [here](#ready-to-go-configuration) for a sample.

### CLI

<aside class="notice">npm i traffic-splitter -g</aside>

```javascript
// in case config is a js file wrap it in:
module.exports = {}
```

<aside class="notice">traffic-splitter -c configuration.js</aside>

Provided configuration can be a json or a js file.

<br>

### API

<aside class="notice">npm i traffic-splitter</aside>

```javascript
const TrafficSplitter = require('traffic-splitter')
const splitter = new TrafficSplitter(/*your configuration*/)
splitter.start()
```

Import the package.

Create an instance providing a configuration object.

Start it!

<br>
<br>
<br>

And BOOM, splitter is running!

<aside class="success">localhost:PORT/healthcheck</aside>
