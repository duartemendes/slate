# Release change log

### 1.2.8 - 08/08/2018
* [events parameter fixed](#servingfile) - fixed servingFile and servingFileError events upstream parameter.
* [handle file read fail](#serve-file) - send 500 http status code when it fails to read the file.

### 1.2.7 - 02/08/2018
* [emitting browserid](#browserid) - emit browserid cookie only when a [bucket](#bucket) criteria is present.

### 1.2.6 - 01/08/2018
* [events created and updated](#servingFile) - new events servingFile and servingFileError were created. serving and servingError events now have access to upstream request and response.

### 1.2.4 - 26/06/2018
* [referencing upstreams](#upstreamsreferences) - configuration property to increase config legibility.

### 1.2.0 - 03/10/2017
* [serve file upstream type](#serve-file) - there is a new upstream type that allows you to serve files!
* [custom executors](#addexecutor) - you can now add your own custom executors.

### 1.1.0 - 16/08/2017
* rulesets updated - each ruleset is now only evaluated once per request, improving the time to determine the appropriate upstream.
