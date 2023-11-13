
![WhatsApp Image 2023-11-13 at 11 23 20 AM](https://github.com/sanurb/vagrant-prometheus-grafana/assets/80982045/8e75a35f-23ca-47fc-a5af-61a4fb55b58b)

# Prometheus and Grafana Configuration Guide

This README provides an overview of the key configuration paths and settings for Prometheus and Grafana, based on the installation on an Ubuntu system. For detailed installation instructions, please refer to the [Linode Guide](https://www.linode.com/docs/guides/how-to-install-prometheus-and-grafana-on-ubuntu/).

## Prometheus Configuration

### Key Configuration Paths

- **Prometheus Configuration File**: `/etc/prometheus/prometheus.yml`
  - This YAML file contains the main configuration for Prometheus.
- **Prometheus Data Directory**: `/var/lib/prometheus`
  - This directory stores Prometheus application data.
- **Prometheus Executable**: `/usr/local/bin/prometheus`
  - The location of the Prometheus executable file.
- **Prometheus Service File**: `/etc/systemd/system/prometheus.service`
  - This file is used to configure Prometheus as a service.

### Overview

Prometheus is an open-source system monitoring application that collects server metrics at regular intervals. It is configured to monitor itself by default, and additional client nodes can be added for monitoring.

## Grafana Configuration

### Key Configuration Paths

- **Grafana Web Interface**: Accessed on port `3000` of the host server.
- **Grafana Dashboards**: Integrated within the Grafana UI.
  - Dashboards can be imported or created for various data visualizations.

### Overview

Grafana is a visualization tool that displays metrics collected by Prometheus in a user-friendly dashboard format. It does not collect or store data itself but provides an intuitive interface for data presentation.

## Node Exporter

- **Node Exporter Installation**: Installed on each client node.
- **Default Port**: `9100`
  - Node Exporter listens on this port for Prometheus to collect metrics.

### Overview

Node Exporter is a Prometheus component that collects hardware and OS metrics from the client nodes. It is essential for Prometheus to gather detailed statistics about each node.

## Additional Notes

- Ensure both Prometheus and Grafana are running on the same server for optimal performance.
- Customize the Prometheus and Grafana configurations as per your monitoring requirements.
- Refer to the official documentation for advanced configurations and troubleshooting.

## Metric exploration

### 1. CPU usage per Core

This query provides CPU usage broken down by core, which is useful for identifying load imbalances between cores.

```promql
100 - (avg by (cpu) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

This query uses irate to calculate the rate of change per second of the metrics over a 5-minute interval, giving insight into the percentage of time each CPU core spends in idle mode.

### 2. Memory Usage
This query gives us a view of RAM usage, excluding the areas used by buffers and cache, thus providing a more realistic view of memory usage by process.

```promql
(node_memory_MemTotal_bytes - node_memory_MemFree_bytes - node_memory_Buffers_bytes - node_memory_Cached_bytes) / node_memory_MemTotal_bytes * 100
```

### 3. Disk Usage per Device
Monitoring disk usage is critical, especially on systems with high disk read/write. This query shows disk usage by device.

```promql
100 - (node_filesystem_free_bytes{fstype!="tmpfs"} * 100) / node_filesystem_size_bytes{fstype!="tmpfs"}
```

### 4. Network Traffic

This query is useful for monitoring network traffic, both incoming and outgoing, on network interfaces.

```promql
rate(node_network_receive_bytes_total[5m])
rate(node_network_transmit_bytes_total[5m])
```

These two queries provide the rate of bytes received and transmitted in a 5-minute interval for each network interface.

### 5. System Load
System load is a critical indicator of overall performance. This query provides the average system load over the last 1, 5 and 15 minutes.

```promql
node_load1
node_load5
node_load15
```
### Fuentes: 

https://prometheus.io/docs/introduction/overview/
https://grafana.com/docs/grafana/latest/
