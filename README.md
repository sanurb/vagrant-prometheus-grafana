# vagrant-prometheus-grafana


## Metric exploration

### 1. CPU usage per Core

Esta consulta proporciona el uso de la CPU desglosado por core, lo que es útil para identificar desequilibrios en la carga entre cores.

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