# Getting started

First things first:
<aside class="notice">npm install traffic-splitter --save</aside>

```javascript
const TrafficSplitter = require('traffic-splitter')
const splitter = new TrafficSplitter(/*your configuration*/)
splitter.start()
```

Import the package.

Create an instance with your personalized [configuration](#configuration),

Start it!

<br>

And BOOM, splitter is running!

<aside class="success">localhost:your_port/healthcheck</aside>
