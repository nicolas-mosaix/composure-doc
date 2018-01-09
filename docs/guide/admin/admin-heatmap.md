---
title: Heatmap
contributors:
- nicolas
---

{% capture overview %}

The Heatmap shows the capacity and load of the physical servers in the cloud. Click on **Tools  Heatmap** to navigate to the tool.

{% endcapture %}


{% capture steps %}

The heatmap is a treemap structure. The top level is the load and capacity of each cloud and if you click on a cloud, it will show the physical server’s capacity and load and then under each server is the VM/Container. You can select which single aspect of the cloud’s capacity and load you are interested in (on the left side): CPU, memory, disk, etc.

A heatmap with four cloud is shown below. The block size shows the capacity and the color shows the load. The darker the green, the more the server is loaded.

<img src="/media/image94.png" width="624" height="380" />

In the following screenshot, we drilled down to a specific docker host on a specific Docker Cloud, showing all the containers running on that host and showing the amount of CPU assigned to each container:

<img src="/media/image95.png" width="624" height="382" />

{% endcapture %}


{% include templates/guide.md %}
