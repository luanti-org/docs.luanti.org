---
title: Server Metrics using Prometheus
aliases:
  - /Server_Metrics_using_Prometheus
  - /server-metrics-using-prometheus
  - /server/prometheus-metrics
---

Prometheus is an "open-source monitoring system with a dimensional data model, flexible query language, efficient time series database and modern alerting approach".

Luanti has built-in support for Prometheus, allowing you to see metrics about your server - such as player count and performance information.

Here are some examples of Grafana dashboards created using Prometheus:

- [monitor.rubenwardy.com](https://monitor.rubenwardy.com/d/9TgIegyGk/ctf)
- https://monitoring.luanti.ch/ (note: this contains heavy modifications)

## Using Prometheus in Luanti

### Get or build the prometheus-cpp library

You may be able to find prometheus-cpp in your distro's package manager. Otherwise, you'll need to build it and make it available to Luanti.

Here's how you do that on Linux:

```bash
cd /tmp
git clone --recursive https://github.com/jupp0r/prometheus-cpp
mkdir prometheus-cpp/build
cd prometheus-cpp/build
cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local -DCMAKE_BUILD_TYPE=Release -DENABLE_TESTING=0
make -j$(nproc)
sudo make install
```

### Build Luanti

Now, you need build Luanti with Prometheus enabled

```bash
cmake . -DENABLE_PROMETHEUS=1
make -j$(nproc)
```

### Configure prometheus

Prometheus will make HTTP requests to Luanti's prometheus-cpp library to get the metrics.

You can set the prometheus listen address by assigning an IP:PORT in the minetest.conf:

```
prometheus_listener_address = 127.0.0.1:30000
```

## Grafana

You can link up Prometheus to Grafana to make nice web graphs. For example: [monitor.rubenwardy.com](https://monitor.rubenwardy.com/d/r6cCl68mk/minetest-servers)

To do: example layouts
