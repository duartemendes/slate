# Debugging

```javascript
Request:
  curl
    --user-agent "Mozilla/5.0 (iPhone; CPU iPhone OS 10_0 like Mac OS X) AppleWebKit/602.1.38 (KHTML, like Gecko) Version/10.0 Mobile/14A5297c Safari/602.1"
    -G localhost
    -d splitterIP=195.245.151.210
    -d splitterDebug=true
    -v

Response debugging headers:
  x-splitter-upstream: "debugging"
  x-splitter-geo: "PT.17.Porto"
  x-splitter-device: {"ua":"Mozilla/5.0 (iPhone; CPU iPhone OS 10_0 like Mac OS X) AppleWebKit/602.1.38 (KHTML, like Gecko) Version/10.0 Mobile/14A5297c Safari/602.1","_cache":{"phone":"iPhone","mobile":"iPhone","tablet":null},"maxPhoneWidth":600}
  x-splitter-bucket: 66
```

Pass the ?splitterDebug=true parameter to obtain debug information in the response headers about the criteria selection.

### Provided headers
**x-splitter-upstream** - selected upstream name

**x-splitter-geo**\* - geoip information

**x-splitter-device**\* - device information

**x-splitter-bucket**\* - assigned bucket

\* - only available if selected upstream has the respective rule
