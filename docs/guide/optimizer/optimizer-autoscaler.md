---
title: Autoscaler
contributors:
- nicolas
---

{% capture overview %}

Auto-scaling as a Service provides a way to start a user application and then automatically autoscale resources (VMs or containers) based on metrics fed into Composure from external monitoring systems. To allow Composure to receive metrics (CPU usage, network throughput, response times, latency, etc) from external monitoring systems read the [*Metric Firehose*](#metric-firehose) section.

{% endcapture %}


{% capture steps %}

The Autoscaler service can work within and across clouds. For example, in an autoscaling container setup with multiple Docker clouds, before instantiating containers in a Docker host, the autoscaling service consults the Composure Scheduler to determine where the new containers should be placed to meet the requested quality of service (performance, security, etc) and other constraints (affinity or anti-affinity to workloads). You set the scheduling policy based on the kind of application you want to autoscale and set that in the AppSpec file.

The Cloud Compose AppSpec file is submitted through Cloud Composer, directly from the Composer or from the Hub. The Autoscaler tool shows submitted applications and their corresponding autoscaling policy, which can be modified from there as well.

### Using the Autoscaler tool

1. From the menu bar, click on the **Tools  Autoscaler** icon.

    <img src="/media/image68.png" width="226" height="347" />

1. The Autoscaler tool displays the list of active services with at least one AUTOSCALER service:

    <img src="/media/image69.png" width="624" height="178" />

1. Click on the <img src="/media/image70.png" width="108" height="32" />button for a selected autoscaled service to see the autoscale policy set for this service:

    <img src="/media/image71.png" alt="screen31.png" width="491" height="462" />

1. Click “View Application” button for real-time monitoring.

    There are 3 main sections:

| Section              | Description                                                                                                                                                                      |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Autoscaler Group** | **Expandable** cloud graph view of the autoscaled application, with autoscaler-groups and container. **View Logs** to show a history of all scale-in and scale-out events.  |
| **Policy Editor**    | Select the autoscale-group from the graph view to view and then edit the policy, while the app is still running.                                                                 |
| **Metrics**          | Real-time monitoring load and other data for each metrics used to trigger scale-in and scale-out events.                                                                         |

  <img src="/media/image72.png" width="647" height="590" />

1. Click Edit from the Policy Editor to modify any of the settings:

  <img src="/media/image73.png" width="331" height="354" />

1. Click **Add a metric** to add a new metric to drive more precisely how autoscale should be triggered:

  <img src="/media/image74.png" width="310" height="260" />

{% endcapture %}


{% include templates/guide.md %}
