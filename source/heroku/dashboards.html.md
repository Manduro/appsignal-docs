---
title: "Heroku dashboards"
description: "Learn how to set up Heroku Dashboards and what each dashboard means."
---

-> **Tip:** Make sure a working [Heroku Logplex Logdrain](/heroku/setup-logdrain.html) is setup before continuing with this section.

These dashboards will be generated automatically as "Magic dashboards", when a Logdrain is setup. No further action is required. Note that in some cases you need a

- [Status code dashboard](#status-code-dashboard)
- [Redis dashboard](#redis-dashboard)
- [PostgreSQL dashboard](#postgresql-dashboard)

## Status code dashboard

The status dashboard shows an aggregation of the status codes, returned by Heroku. These a grouped in the main status code categories (2xx, 3xx, 4xx and 5xx).


## Redis dashboard

Redis metrics provided by the Heroku Log Drain. These metrics are exposed by Heroku for Redis premium instances. This dashboard shows active connections, hit rate, evicted keys and redis host metrics.

**Active Connections**: The number of connections established on the Redis instance.

**Redis hit rate**: The ratio of successful reads out of all read operations, rounded to five decimal points.

**Redis evicted keys**: The number of evicted keys due to reaching your instance maxmemory limit.

**Memory**: Approximate amount of memory used by your Redis processes in kB. This includes shared buffer cache as well as memory for each connection.

**Redis server Load average**: Average load on the system over a period of 1 minute.

**Redis server I/O**: Number of read/write operations in I/O sizes of 16KB blocks


## PostgreSQL dashboard

Postgres metrics provided by the Heroku Log Drain. These metrics are exposed by Heroku for Postgres standard and premium instances. This dashboard shows database size, connections and index hit rates.

**Waiting connections**: Connections waiting for a lock. If there are too many connections waiting, it could point to over-use of connections by not using a connection pool, for example.

**Index/Table cache hit rate**: Ratio of index lookups served from a buffer cache. Ideally, this value is always 0.99 or higher. If it drops below 0.99 consistently, you may need to upgrade your database plan or add more RAM. Cache hit rate is a great metric to set an [anomaly detection trigger](/application/anomaly-detection/) on.

**Memory usage**: We track both memory used by Postgres and the system itself. Postgres memory includes buffer cache and memory per connection. For multi-tenant plans, system metrics may include other databases and might be misleading.

**Load average**: Average system load of the Heroku database server, for more information about how to read load averages, we've written a blog post about it a while ago called: ["Understanding system load
and load averages"](https://blog.appsignal.com/2018/03/28/understanding-system-load-and-load-averages.html).

**I/O read/write operations**: Number of read/write operations in sizes of 16KB Blocks. Each Postgres plan has a limit on IOPS it can perform ([see the Heroku docs here](https://devcenter.heroku.com/articles/heroku-postgres-production-tier-technical-characterization)), this would be an excellent candidate to set an [anomaly detection trigger](/anomaly-detection/) on.

For more information about these metrics, you can read more on [the Heroku documentation page](https://devcenter.heroku.com/articles/heroku-postgres-metrics-logs).
