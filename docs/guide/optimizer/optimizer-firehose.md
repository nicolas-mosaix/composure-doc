---
title: Metric Firehose
contributors:
- nicolas
---

{% capture overview %}

With the Metric Firehose, Composure can use metrics from external monitoring systems to drive autoscaling.

This section describes :
  - How to connect a third-party monitoring system to a cloud.
  - How to leverage default monitoring for Docker Clouds.
  - How to connect to specific default metrics to drive autoscaling decisions.
  - How to connect to new custom metrics

For Docker Clouds, if no external monitoring system is provided, Composure automatically deploys Prometheus for Docker Clouds. Otherwise users can use any of the monitoring systems listed below.

| Cloud     | Monitoring System     |
|-----------|-----------------------|
| Docker    | Prometheus, Sensu     |
| OpenStack | Ceilometer            |
| AWS       | CloudWatch (future)   |
| Azure     | Future                |


{% endcapture %}


{% capture steps %}

### How does it work?

Composure offloads the process of collecting monitoring statistics to external monitoring systems. The Metrics firehose continuously polls these for monitoring statistics.

However, Composure collects some metrics directly from the cloud provider by considering multiple variables on the resource, in particular if those metrics are not collected by a 3rd party monitoring system.

Composure retrieves metrics in the following order:
1.  Retrieve the metrics from the cloud provider directly.
1.  Retrieve the metrics from the external monitoring system.
In the event of a conflict, the metric firehose chooses the metric reported by the cloud provider instead of the same metric from the external monitoring system.

### Connecting a monitoring system to a Cloud

The Metrics Firehose feeds Composure with monitoring statistics from external monitoring systems via a REST endpoint. Once the cloud is mounted and connected to a monitoring system, default metrics are available. Those metrics are used to trigger scale-up and scale-down events based on the specified auto-scaling policy.

To associate a cloud with an existing monitoring system, the details of the monitoring system need to be provided in the cloud mount file used to mount the cloud to Composure.

The following cloud mount file shows how to connect an external monitoring system to Docker (provider: docker). The cloud **Docker\_Cloud\_2** is associated with a Prometheus based monitoring system that is running at the REST endpoint [http://192.168.122.23:9300](http://192.168.122.23:9300).

```yaml
cloud:
    name : Docker_Cloud_2
    provider : docker
    monitoringSystemParams : [\http://192.168.122.23:9300\]
    cloudosServerAddress: 192.168.122.1
    monitoringSystemType : PROMETHEUS
    <padmin.username : mosaix
    Endpoints : http://54.201.137.193:4243/, http://54.201.137.201:4243/
```

#### Keys

| Key                    | Type   | Description                                                                          |
| -----------------------|--------|--------------------------------------------------------------------------------------|
| monitoringSystemParams | string | Endpoint of the monitoring system. Format: \["\\"http://ip\_address:port\\""\]       |
| cloudosServerAddress   | string | IP address of the Composure server which will connect to the monitoring system. It should be set to public IP address in most cases. If not provided, the IP address of one of the Composure network interfaces (eth\*) is selected. |
| monitoringSystemType   | string | Select either PROMETHEUS, SENSU or CEILOMETER.                                       |

### Default monitoring system deployed for Docker Clouds

If you do not connect a monitoring system to the Cloud when mounting it to Composure, Composure will deploy Prometheus when the cloud is mounted.

At this time, we only support automatic deployment of a monitoring system for a Docker cloud. For Openstack clouds, Composure uses the Ceilometer REST endpoint.

Prometheus is the monitoring system started (as a container) locally on Composure. Additionally, a container is deployed on each Docker host that comprises the Docker cloud. These containers run as privileged containers on the host, so that they can retrieve metrics which require privileged access.

Each container runs two standard services called **nodeExporter** and **cAdvisor** to export host and container metrics.

### Connecting Autoscaler to a specific monitoring metric

Metric are used to take autoscaling actions: scale-out and scale-in by setting a threshold when you want to scale out or scale in.

The following autoscaler policy which is a subset of the AUTOSCALER service type which can be added to your YAML based appspec file.

```yaml
policy:

 - name: node-policy

     min: 2 # Minimum number of nodes to autoscale
     max: 6 # Maximum number of nodes to autoscale

     scheduler_policy: fast-spread # Scheduling policy monitor:

 - name: cpu_used # Metric that triggers autoscaling

     scale_in_threshold: 0
     scale_out_threshold: 50

     scale_in_unit: 1
     scale_out_unit: 1

     reaction_time: 30
```

The field definitions are:

| Metric                                       | Definition                                                                                                                                                                      |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name                                         | Name defined in JSON file for Composure metric. Can be any value.                                                                                                               |
| Scale_in_threshold, scale_out_threshold:     | This is normal operating range. So 0 to 50 is within thresholds for this example. When range is exceeded then the Autoscaler scales up another instance in scale\_out\_unit(s). |
| scale_in_unit                                | Quantity of metrics under observation. So it could be 1 cpu meaning the metric is per 1 cpu.                                                                                    |
| scale_out_unit: 1                            | Quantity of objects to expand. 1 means 1 of whatever item in being monitored. So here it might be enough to grab another vCPU for this instance.                                |
| reaction_time                                | Time in seconds that metric can exceed threshold before autoscale kicks in.                                                                                                     |

The metrics and thresholds are then visible in the following two Autoscaler screens:

  <img src="/media/image71.png" width="472" height="454" />

**Note:** If you are interested in connecting with other monitoring products, contact Composure.ai for more information.

**Default metrics**

The table below describes the list of default metrics available in Composure depending on the type of 3rd party monitoring system it is connected to.


Found under the following directory: /mnt/opt/mosaix/bin/kpa/conf.d/
```code
  $ cat prometheus-docker-metrics-map.json

  {
    eval(sum(container_fs_usage_bytes{image!=\\}/(1024*1024*1024)) by (id)) : disk_used,
    eval(sum(container_memory_usage_bytes{image!=\\}/(1024*1024)) by (id)) : mem_used,
    eval(sum(irate(container_cpu_usage_seconds_total{image!=\\}[1m])) by (id)) : cpu_used,
    eval(sum(container_memory_working_set_bytes{image!=\\}/(1024*1024)) by (id)) : mem_total,
    eval(sum(container_fs_limit_bytes{image!=\\}/(1024*1024*1024)) by (id))</em> : disk_total
  }
```
Consult the Prometheus documentation for the metrics and functions available in that product.

<table>
<tbody>
    <tr class="odd">
    <td><strong>Monitoring System</strong></td>
    <td><strong>Some of the available metrics</strong></td>
    </tr>
    <tr class="even">
    <td>Prometheus</td>
    <td><ul>
    <li><p>Rpc_durations_seconds_count>
    Labels (production, dev, QA, etc.)>
    Http_requests_total</p></li>
    <li><p>Results from Prometheus functions</p></li>
    <li><p>Data which cannot be scraped using the Prometheus Pushgateway</p></li>
    </ul></td>
    </tr>
    <tr class="odd">
    <td>Sensu</td>
    <td><ul>
    <li><p>Check_disk_usage</p></li>
    <li><p>Check_arista_eth0</p></li>
    <li><p>Check_mysql_replication (sample custom attribute)</p></li>
    </ul></td>
    </tr>
    <tr class="even">
    <td>Ceilometer</td>
    <td><ul>
    <li><p>Memory</p></li>
    <li><p>Memory.usage</p></li>
    <li><p>Memory.resident</p></li>
    <li><p>Cpu cumulative</p></li>
    <li><p>Cpu.delta</p></li>
    <li><p>Cpu utilization</p></li>
    <li><p>Vcpus</p></li>
    <li><p>Disk.read.requests</p></li>
    <li><p>Disk.read.requests.rate</p></li>
    <li><p>Disk.write.requests</p></li>
    <li><p>Disk.write.requests.rate</p></li>
    <li><p>Disk.device.latency</p></li>
    <li><p>disk.device.iops</p></li>
    </ul>
    <p>More listed <a href="https://docs.openstack.org/ceilometer/latest/admin/telemetry-measurements.html"><em>here</em></a>.</p>
    </td></tr>
</body>
</table>



### Adding new custom metrics

The Metric firehose can be configured with new metrics from external monitoring systems.

The mapping between the name used in Composure and the third-party product is specified in a product-specific JSON file.

They can point-in-time, cumulative, or alerts.

Note from the author: More details to be provided later.



{% endcapture %}


{% include templates/guide.md %}
