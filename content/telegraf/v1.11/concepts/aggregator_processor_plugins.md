---
title: Telegraf aggregator and processor plugins
description: Use Telegraf aggregator and processor plugins to aggregate and process data between the input plugins and output plugins.
menu:
  telegraf_1_11:
    name: Aggregator and processor plugins
    weight: 20
    parent: Concepts
---

Besides the input plugins and output plugins, Telegraf includes aggregator and processor plugins, which are used to aggregate and process metrics as they pass through Telegraf.

{{< diagram >}}
  graph TD
  Process[Process<br/> - transform<br/> - decorate<br/> - filter]
  Aggregate[Aggregate<br/> - transform<br/> - decorate<br/> - filter]

  CPU --> Process
  Memory --> Process
  MySQL --> Process
  SNMP --> Process
  Docker --> Process
  Process --> Aggregate
  Aggregate --> InfluxDB
  Aggregate --> File
  Aggregate --> Kafka

style Process text-align:left
style Aggregate text-align:left
{{< /diagram >}}

**Processor plugins** process metrics as they pass through and immediately emit
results based on the values they process. For example, this could be printing
all metrics or adding a tag to all metrics that pass through.

**Aggregator plugins**, on the other hand, are a bit more complicated. Aggregators
are typically for emitting new _aggregate_ metrics, such as a running mean,
minimum, maximum, quantiles, or standard deviation. For this reason, all _aggregator_
plugins are configured with a `period`. The `period` is the size of the window
of metrics that each _aggregate_ represents. In other words, the emitted
_aggregate_ metric will be the aggregated value of the past `period` seconds.
Since many users will only care about their aggregates and not every single metric
gathered, there is also a `drop_original` argument, which tells Telegraf to only
emit the aggregates and not the original metrics.

**NOTE** Since aggregator plugins only aggregate metrics within their periods,
historical data is not supported. In other words, if your metric timestamp is more
than `now() - period` in the past, it will not be aggregated. If this is a feature
that you need, please comment on this [GitHub issue](https://github.com/influxdata/telegraf/issues/1992).
