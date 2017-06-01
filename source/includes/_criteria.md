# Criteria

As shown in the [example](#example), criteria has two properties: "in" and "out".

Those properties are objects that contains rules that will be evaluated against the request made.

```javascript
{
  "path": ["/home", "/about", "/blog"]
}
```

There are quite a few default rules at your disposal, check below. These rules are arrays that may contain n elements and only one of them needs to be true for the condition to be true as well. Check the given example: the request can only have one path, so for the condition to be valid that path needs to match one of the given paths.

<br>

Oh, and you can also add your custom rules in a very easy and smooth way! (wait... what?) Yeah, it's [true](#addrule)!

## agent
```javascript
"agent": ["Chrome", "Safari", "Opera"]
```

Evaluates against the user-agent header.

If no such header is provided the condition wont ever be met.

## bucket
```javascript
"bucket": [
  {
    "min": 50,
    "max": 100
  }
]
```

Bucket selection is one of the most basic forms of traffic splitting. It allows the splitter to send percentages of traffic to different upstreams.

The bucketing system works by issuing a browser identifier cookie (name and lifespan [configurable](#browserid)) with random sequence of characters (12 by default). The cookie is only issued once, which the splitter then uses to calculate the bucket (from 0 to 100). Meaning that, once a user was assigned a bucket, it will always be "inside" that bucket (unless cookies are deleted).

## cookie
```javascript
"cookie": [
  {
    "name": "amIallowed",
    "value": "yes"
  }
]
```

Matches against the request cookies.

[Remember](#criteria) that only one cookie needs to be matched for the condition to be valid!

## device

Device detection is achieved using the [mobile-detect](https://www.npmjs.com/package/mobile-detect) package, which is based on the user-agent header.

```javascript
"device": [
  {
    "device": "tablet",
    "type": "ipad"
  },
  {
    "device": "desktop",
    "browser": "Firefox",
    "version": {
      "from": 35,
      "to": 37
    }
  }
]
```

### Options

**device** - "desktop", "phone", "tablet", "mobile". "mobile" will match both tablets and phones.

**type** - only relevant when device is set to either phone, tablet or mobile. It provides the type of device.

**browser** - any browser

**version** - version of browser with the ability to limit the version by specifying a "from" and "to". Either from or to can be omitted to leave the lower/upper boundary open.

Check the package [documentation](http://hgoebl.github.io/mobile-detect.js/doc/MobileDetect.html) for the list of device types and browsers.

## geoip
```javascript
"geoip": [
  {
    "country": "US",
    "region": "TX",
    "city": "San Antonio"
  },
  {
    "country": "PT",
    "region": "",
    "city": ""
  }
]
```

[geoip-lite](https://www.npmjs.com/package/geoip-lite) package allows traffic splitter to evaluate the location from where the request was made. This package uses the GeoLite database from [MaxMind](https://www.maxmind.com).

<br>

You can leave any property empty to get a broader area.

<br>

By default, this uses the IP address from the incoming connection but this can be overridden by passing the ?splitterIP=<IP> parameter on the query string.

## host
```javascript
"host": [
  "www.mindera.com",
  "mindera.com",
]
```

Evaluates against the host header.

If no such header is provided the condition wont ever be met.

## path
```javascript
"path": ["/home", "/about", "/blog"]
```

Validates against the request path.

For better use of this rule check [pathRegExp](#pathregexp).

## visitor
```javascript
"visitor": true
```

User is considered a visitor when request has no cookies.

## and
```javascript
"and": [
  // insert n rules in here
]
```

This rule consists of an array of rules and it will only be true if all rules inside it are also true.

Note that this particular rule is slightly redundant as the splitter already matches rules within in and out sections using the AND approach (all rules must match).

## or
```javascript
"or": [
  // insert n rules in here
]
```

By now we bet you already know how this rule works, right? :)

In case you don't, this rule also consists of an array of rules and it will be true if at least one of the rules inside it is true.
