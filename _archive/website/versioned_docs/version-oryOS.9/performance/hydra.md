---
id: version-oryOS.9-hydra
title: ORY Hydra
original_id: hydra
---

> You are viewing an outdated version of this documentation. Please head over
> to [www.ory.sh/docs](https://www.ory.sh/docs) for a recent version!

In this document you will find benchmark results for different endpoints of ORY
Hydra. All benchmarks are executed using
[rakyll/hey](https://github.com/rakyll/hey). Please note that these benchmarks
run against the in-memory storage adapter of ORY Hydra. These benchmarks
represent what performance you would get with a zero-overhead database
implementation.

We do not include benchmarks against databases (e.g. MySQL or PostgreSQL) as the
performance greatly differs between deployments (e.g. request latency, database
configuration) and tweaking individual things may greatly improve performance.
We believe, for that reason, that benchmark results for these database adapters
are difficult to generalize and potentially deceiving. They are thus not
included.

This file is updated on every push to master. It thus represents the benchmark
data for the latest version.

All benchmarks run 10.000 requests in total, with 100 concurrent requests. All
benchmarks run on Circle-CI with a
["2 CPU cores and 4GB RAM"](https://support.circleci.com/hc/en-us/articles/360000489307-Why-do-my-tests-take-longer-to-run-on-CircleCI-than-locally-)
configuration.

## BCrypt

ORY Hydra uses BCrypt to obfuscate secrets of OAuth 2.0 Clients. When using
flows such as the OAuth 2.0 Client Credentials Grant, ORY Hydra validates the
client credentials using BCrypt which causes (by design) CPU load. CPU load and
performance depend on the BCrypt cost which can be set using the environment
variable `BCRYPT_COST`. For these benchmarks, we have set `BCRYPT_COST=8`.

## OAuth 2.0

This section contains various benchmarks against OAuth 2.0 endpoints

### Token Introspection

```

Summary:
  Total:	0.5401 secs
  Slowest:	0.0240 secs
  Fastest:	0.0001 secs
  Average:	0.0051 secs
  Requests/sec:	18516.3041

  Total data:	1550000 bytes
  Size/request:	155 bytes

Response time histogram:
  0.000 [1]	|
  0.002 [3271]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.005 [1381]	|■■■■■■■■■■■■■■■■■
  0.007 [1796]	|■■■■■■■■■■■■■■■■■■■■■■
  0.010 [2563]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.012 [710]	|■■■■■■■■■
  0.014 [187]	|■■
  0.017 [39]	|
  0.019 [17]	|
  0.022 [18]	|
  0.024 [17]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0003 secs
  50% in 0.0055 secs
  75% in 0.0080 secs
  90% in 0.0097 secs
  95% in 0.0111 secs
  99% in 0.0143 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0240 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0041 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0050 secs
  resp wait:	0.0049 secs, 0.0001 secs, 0.0187 secs
  resp read:	0.0000 secs, 0.0000 secs, 0.0051 secs

Status code distribution:
  [200]	10000 responses



```

### Client Credentials Grant

This endpoint uses [BCrypt](#bcrypt).

```

Summary:
  Total:	18.5476 secs
  Slowest:	0.6890 secs
  Fastest:	0.0166 secs
  Average:	0.1790 secs
  Requests/sec:	539.1521

  Total data:	1570000 bytes
  Size/request:	157 bytes

Response time histogram:
  0.017 [1]	|
  0.084 [1378]	|■■■■■■■■■■■■■■■■■
  0.151 [2812]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.218 [3181]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.286 [1112]	|■■■■■■■■■■■■■■
  0.353 [971]	|■■■■■■■■■■■■
  0.420 [353]	|■■■■
  0.487 [88]	|■
  0.555 [57]	|■
  0.622 [41]	|■
  0.689 [6]	|


Latency distribution:
  10% in 0.0740 secs
  25% in 0.1043 secs
  50% in 0.1788 secs
  75% in 0.2219 secs
  90% in 0.3029 secs
  95% in 0.3641 secs
  99% in 0.4882 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0166 secs, 0.6890 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0070 secs
  req write:	0.0002 secs, 0.0000 secs, 0.1613 secs
  resp wait:	0.1780 secs, 0.0166 secs, 0.6890 secs
  resp read:	0.0006 secs, 0.0000 secs, 0.1565 secs

Status code distribution:
  [200]	10000 responses



```

## OAuth 2.0 Client Management

### Creating OAuth 2.0 Clients

This endpoint uses [BCrypt](#bcrypt) and generates IDs and secrets by reading
from which negatively impacts performance. Performance will be better if IDs and
secrets are set in the request as opposed to generated by ORY Hydra.

```
This test is currently disabled due to issues with /dev/urandom being inaccessible in the CI.
```

### Listing OAuth 2.0 Clients

```

Summary:
  Total:	0.4247 secs
  Slowest:	0.0326 secs
  Fastest:	0.0001 secs
  Average:	0.0039 secs
  Requests/sec:	23546.6970

  Total data:	5090000 bytes
  Size/request:	509 bytes

Response time histogram:
  0.000 [1]	|
  0.003 [5328]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.007 [2049]	|■■■■■■■■■■■■■■■
  0.010 [1478]	|■■■■■■■■■■■
  0.013 [767]	|■■■■■■
  0.016 [257]	|■■
  0.020 [67]	|■
  0.023 [27]	|
  0.026 [20]	|
  0.029 [3]	|
  0.033 [3]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0002 secs
  50% in 0.0020 secs
  75% in 0.0069 secs
  90% in 0.0102 secs
  95% in 0.0123 secs
  99% in 0.0170 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0326 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0069 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0078 secs
  resp wait:	0.0037 secs, 0.0000 secs, 0.0231 secs
  resp read:	0.0001 secs, 0.0000 secs, 0.0079 secs

Status code distribution:
  [200]	10000 responses



```

### Fetching a specific OAuth 2.0 Client

```

Summary:
  Total:	0.4119 secs
  Slowest:	0.0236 secs
  Fastest:	0.0001 secs
  Average:	0.0038 secs
  Requests/sec:	24275.9320

  Total data:	5070000 bytes
  Size/request:	507 bytes

Response time histogram:
  0.000 [1]	|
  0.002 [4947]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.005 [1282]	|■■■■■■■■■■
  0.007 [1593]	|■■■■■■■■■■■■■
  0.009 [1122]	|■■■■■■■■■
  0.012 [642]	|■■■■■
  0.014 [258]	|■■
  0.017 [97]	|■
  0.019 [40]	|
  0.021 [11]	|
  0.024 [7]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0002 secs
  50% in 0.0027 secs
  75% in 0.0067 secs
  90% in 0.0096 secs
  95% in 0.0113 secs
  99% in 0.0154 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0236 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0060 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0056 secs
  resp wait:	0.0035 secs, 0.0000 secs, 0.0236 secs
  resp read:	0.0001 secs, 0.0000 secs, 0.0088 secs

Status code distribution:
  [200]	10000 responses



```
