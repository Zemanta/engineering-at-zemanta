#Monitoring

At zemanta we monitor all web application in real time. We do so, to be able to react to failiures in producion fast. 

In this section we will cover the tools we use for monitoring and alerting and their purpose alongiside use cases.

## InfluxDB + Grafana

One can read about our history of migrating from librato to [influxdb + grafana](http://zemanta.github.io/2016/05/10/from-librato-to-influxdb/) stack to better understand why are we using this metrics stack. 

Every application is emitting lots of real time metrics via the influx client that is embedded into the application.

**Python example**

```python
influx.incr('etl.refresh_k1.refresh_k1_reports', 1, mytag='value')
```

**Go example**

```go
handler.Influx.Counter("bidder.bidHandler.histogram", 1).Tag("bucket", "plus").Submit()
```

For emitting metrics we use:

* [`Zemanta/django-statsd-influx`](https://github.com/Zemanta/django-statsd-influx)
* Internal Go lib, soon to be opensourced

Both libraries above, send metircs to Telegraf via statsd compatibility layer that Telegraf supports. Telegraf then ingests these metrics to InfluxDB. Grafana queries InfluxDB directly for data that's shown in it's dashboards. 


### InfluxDB + Grafana Purpose

* Every important high level feature in our apps should be emitting metric(s) to monitor feature's internal state.
* Every app should also be sending high level performance metrics to InfluxDB.
* Grafana is also the main source of truth for application state and performance the entire team always goes to as the **first point of contact upon state or performance inquiry**. 


