# Schema Comparison between Structured Logging Libraries

The goal of this document is to summarize the differences between various
structured logging libraries, in order to encourage a discussion so that a
consensus can be reached for a common format.

Please use the GitHub issues for discussion, and submit GitHub Pull requests
to add/fix the information in the tables.


## Message and Payload

Behold the following logging command that you might find in an application.
There is a "message" string, and a "payload" object:

    logger.info("Hello World", {"animal": "cat", "numLegs": 4})
                 ^^^^^^^^^^^   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                 Message       Payload object

Some libraries merge the payload object with the root object, so the event
will look like this:

```json
{
    "timestamp": "2018-06-18T23:16:45.000Z",
    "message": "Hello World",
    "animal": "cat",
    "numLegs": 4
}
```

While other libraries nest the payload object beneath some key, with a result like this:

```json
{
    "timestamp": "2018-06-18T23:16:45.000Z",
    "message": "Hello World",
    "data": {
        "animal": "cat",
        "numLegs": 4
    }
}
```

| Library | Message JSON Key | Payload Object JSON Key |
|---------|--------------------|------------------|
| [bunyan](https://github.com/trentm/node-bunyan) (JavaScript) |  `msg` | *Merged with root* |
| [katip](https://github.com/Soostone/katip) (Haskell) | `msg` | `data` |
| [logrus](https://github.com/sirupsen/logrus) (Go) | `msg` | *Merged with root* |
| [logstash](https://github.com/elastic/logstash) | `message` | *Merged with root* |
| [ougai](https://github.com/tilfin/ougai) (Ruby) | `msg` | *Merged with root* |
| [pygogo](https://github.com/reubano/pygogo) (Python) | `message` | *Merged with root* |
| [roarr](https://github.com/gajus/roarr) (JavaScript) | `message` | `context` |
| [semantic logger](https://github.com/rocketjob/semantic_logger) (Ruby) | `message` | payload |
| [serilog](https://github.com/serilog/serilog) (C#) | `@m` / `@mt` | *Merged with root* |
| [slog](https://github.com/slog-rs/slog) (Rust) | `msg` | *Merged with root* |
| [structlog](https://github.com/hynek/structlog) (Python) | `msg` | **HELP NEEDED** |


## Timestamp JSON Key and format

All libraries automatically add a timestamp to all events.

| Library | Timestamp JSON Key | Timestamp format |
|---------|--------------------|------------------|
| [bunyan](https://github.com/trentm/node-bunyan) (JavaScript) | `time` | ISO 8601 |
| [katip](https://github.com/Soostone/katip) (Haskell) | `at` | ISO 8601 |
| [logrus](https://github.com/sirupsen/logrus) (Go) | `time` | ISO 8601 |
| [logstash](https://github.com/elastic/logstash) | `timestamp` / `@timestamp` | ISO 8601 / varies |
| [ougai](https://github.com/tilfin/ougai) (Ruby) | `time` | ISO 8601 |
| [pygogo](https://github.com/reubano/pygogo) (Python) | `time` | ISO 8601 |
| [roarr](https://github.com/gajus/roarr) (JavaScript) | `time` | Epoch milliseconds |
| [semantic logger](https://github.com/rocketjob/semantic_logger) (Ruby) | `timestamp` | ISO 8601 |
| [serilog](https://github.com/serilog/serilog) (C#) | `Timestamp` / `@t` | ISO 8601 |
| [slog](https://github.com/slog-rs/slog) (Rust) | `ts` | ISO 8601 |
| [structlog](https://github.com/hynek/structlog) (Python) | `timestamp` | ISO 8601 |


## Log Level

All libraries require that that all events are tagged with a level, chosen
from a pre-defined list of available levels.

### Log Level JSON Key

| Library | Log Level JSON Key |
|---------|--------------------|
| [bunyan](https://github.com/trentm/node-bunyan) (JavaScript) | `level` |
| [katip](https://github.com/Soostone/katip) (Haskell) | `sev` |
| [logrus](https://github.com/sirupsen/logrus) (Go) | `level` |
| [logstash](https://github.com/elastic/logstash) | **HELP NEEDED** |
| [ougai](https://github.com/tilfin/ougai) (Ruby) | `level` |
| [pygogo](https://github.com/reubano/pygogo) (Python) | `level` |
| [roarr](https://github.com/gajus/roarr) (JavaScript) | `context.logLevel` |
| [semantic logger](https://github.com/rocketjob/semantic_logger) (Ruby) | `level` |
| [serilog](https://github.com/serilog/serilog) (C#) | `Level` / `@l` |
| [slog](https://github.com/slog-rs/slog) (Rust) | `level` |
| [structlog](https://github.com/hynek/structlog) (Python) | `level` |

### Available Log Levels

| Library | VERBOSE | TRACE | DEBUG | INFO | NOTICE | WARNING | ERROR | CRITICAL | ALERT | FATAL | PANIC | EMERGENCY |
|---------|-----------|---------|----|----|----|----|----|----|----|----|----|----|
| [bunyan](https://github.com/trentm/node-bunyan) (JavaScript) | *N/A* | `10` | `20` | `30` | *N/A* | `40` | `50` | *N/A* | *N/A* | `60` | *N/A* | *N/A* |
| [katip](https://github.com/Soostone/katip) (Haskell) | *N/A* | *N/A* | `Debug` | `Info` | `Notice` | `Warning` | `Error` | `Critical` | `Alert` | *N/A* | *N/A* | `Emergency` |
| [logrus](https://github.com/sirupsen/logrus) (Go) | *N/A* | *N/A* | `debug` | `info` | *N/A* | `warning` | `error` | *N/A* | *N/A* | `fatal` | `panic` | *N/A* |
| [logstash](https://github.com/elastic/logstash) | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** |
| [ougai](https://github.com/tilfin/ougai) (Ruby) | *N/A* | `10` | `20` | `30` | *N/A* | `40` | `50` | *N/A* | *N/A* | `60` | *N/A* | *N/A* |
| [pygogo](https://github.com/reubano/pygogo) (Python) | *N/A* | *N/A* | `DEBUG` | `INFO` | *N/A* | `WARNING` | `ERROR` | `CRITICAL` | *N/A* | *N/A* | *N/A* | *N/A* |
| [roarr](https://github.com/gajus/roarr) (JavaScript) | *N/A* | `10` | `20` | `30` | *N/A* | `40` | `50` | *N/A* | *N/A* | `60` | *N/A* | *N/A* |
| [semantic logger](https://github.com/rocketjob/semantic_logger) (Ruby) | *N/A* | `trace` | `debug` | `info` | *N/A* | `warn` | `error` | *N/A* | *N/A* | `fatal` | *N/A* | *N/A* |
| [serilog](https://github.com/serilog/serilog) (C#) | `Verbose` | *N/A* | `Debug` | `Information` | *N/A* | `Warning` | `Error` | *N/A* | *N/A* | `Fatal` | *N/A* | *N/A* |
| [slog](https://github.com/slog-rs/slog) (Rust) | *N/A* | `TRACE` | `DEBUG` | `INFO` | *N/A* | `WARN` | `ERROR` | `CRITICAL` | *N/A* | *N/A* | *N/A* | *N/A* |
| [structlog](https://github.com/hynek/structlog) (Python) | *N/A* | *N/A* | `debug` | `info` | *N/A* | `warning` | `error` | `critical` | *N/A* | *N/A* | *N/A* | *N/A* |


## System Context Data

Some libraries automatically add additional fields to all events (such as
process ID and thread ID) and/or allow you to configure a few pre-determined
global fields suchs the current environment and application.

| Library | Environment | Application | Namespace | Package | Machine Hostname | Process ID | Thread ID |
|---------|-----------------|----|----|----|----|----|----|
| [bunyan](https://github.com/trentm/node-bunyan) (JavaScript) | *N/A* | *N/A* | *N/A* | *N/A* | `hostname` | `pid` | *N/A* |
| [katip](https://github.com/Soostone/katip) (Haskell) | `env` | `app` (Array of strings) | `ns` (Array of strings) | *N/A* | `host` | `pid` |`thread` |
| [logrus](https://github.com/sirupsen/logrus) (Go) | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* |
| [logstash](https://github.com/elastic/logstash) | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** | **HELP NEEDED** |
| [ougai](https://github.com/tilfin/ougai) (Ruby) | *N/A* | `app` | *N/A* | *N/A* | `hostname` | `pid` | *N/A* |
| [pygogo](https://github.com/reubano/pygogo) (Python) | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* |
| [roarr](https://github.com/gajus/roarr) (JavaScript) | *N/A* | `context.application` | `context.namespace` | `context.package` | `context.hostname` | *N/A* | *N/A* |
| [semantic logger](https://github.com/rocketjob/semantic_logger) (Ruby) | `environment` | `application` | *N/A* | *N/A* | `host` | `pid` | `thread` |
| [serilog](https://github.com/serilog/serilog) (C#) | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* |
| [slog](https://github.com/slog-rs/slog) (Rust) | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* |
| [structlog](https://github.com/hynek/structlog) (Python) | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* | *N/A* |


## HTTP Request Context Data

Logging of HTTP requests is very common. Let's figure out a common format for
HTTP method, host, url, headers, remote IP, remote IP geo-lookup, response
status code, response size, etc...

!!! TODO !!!
