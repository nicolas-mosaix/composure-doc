---
title: Scheduler
contributors:
- nicolas
---

{% capture overview %}

The scheduler determines where workloads are allocated within Composure, where **workload** means at least one container or VM. Here we explain how you can control that.

{% endcapture %}


{% capture steps %}

### Scheduling policies, priorities, and constraints

Three main elements to set are:

1.  **Cloud Priority**—define the clouds you want to use in a prioritized list. If no cloud-priority is defined then a specific cloud must be set.

2.  **Scheduling Policies**—determines how Composure will allocate workloads across hosts. Defaults to **Basic**.

3.  **Constraints**—define workloads that you want allocated on the same host or whether to always allocate those on different hosts.

The scheduling policies, priorities, and constraints are defined in the table below.

<table>
<tbody>
    <tr class="odd">
    <td><strong>Type</strong></td>
    <td><strong>Description</strong></td>
    <td><strong>Illustration</strong></td>
    </tr>
    <tr class="even">
    <td>Cloud-priority</td>
    <td>The scheduler tries to allocate all of the workloads in such a way that the first cloud (in a prioritized list) is selected that meets CPU, memory and disk capacity constraints.</td>
    <td></td>
    </tr>
    <tr class="odd">
    <td colspan="3"><strong>Scheduling Policies</strong></td>
    </tr>
    <tr class="even">
    <td>Basic</td>
    <td>The scheduler tries to allocate all of the workloads such that allocation satisfies CPU, memory and disk capacity constraints.</td>
    <td></td>
    </tr>
    <tr class="odd">
    <td><p>Workload-</p>
    <p>optimize</p></td>
    <td>The scheduler tries to allocate as many workloads as possible subject to CPU, memory and disk capacity constraints.</td>
    <td></td>
    </tr>
    <tr class="even">
    <td><p>Minimize-</p>
    <p>compute</p></td>
    <td>The scheduler tries to use as few hosts as possible to allocate all the workloads subject to CPU, memory, disk capacity and affinity constraints.</td>
    <td></td>
    </tr>
    <tr class="odd">
    <td>Spread</td>
    <td>The scheduler tries to distribute all workloads among hosts as evenly as possible subject to CPU, memory, disk capacity and affinity constraints.</td>
    <td></td>
    </tr>
    <tr class="even">
    <td><p>Maximize-</p>
    <p>network-</p>
    <p>availability</p></td>
    <td>The scheduler tries to allocate all of the workloads to hosts with high network availability subject to CPU, memory, disk capacity and affinity constraints.</td>
    <td></td>
    </tr>
    <tr class="odd">
    <td><p>Cost-</p>
    <p>optimize</p></td>
    <td>The scheduler tries to allocate all of the workloads in such a way that the overall cost is minimized subject to CPU, memory, disk capacity and affinity constraints.</td>
    <td></td>
    </tr>
    <tr class="even">
    <td colspan="3"><strong>Constraints</strong></td>
    </tr>
    <tr class="odd">
    <td>Affinity</td>
    <td>The scheduler tries to allocate all of the workloads to one host subject to CPU, memory, disk capacity and affinity constraints.</td>
    <td></td>
    </tr>
    <tr class="even">
    <td>Anti-affinity</td>
    <td>The scheduler distributes all workloads among hosts such that no two workloads reside on the same host, subject to CPU, memory, disk capacity and affinity constraints.</td>
    <td></td>
    </tr>
</tbody></table>

  Note: illustrations to be added soon.

### Applying scheduling priorities, policies, and constraints

You can use scheduling priorities, policies and constraints individually or in combination with one another.

Scheduling policies and constraints can be applied in two ways:
- Globally to all **CONTAINER** or **VM** services defined within a **COMPOSITE** service under the **WSCHED** service type
- Local to a specific **CONTAINER** or **VM** service under the **scheduling_policy** key.
- Both; In this case, policies defined at the **CONTAINER** level take precedence over the global policies.

### Examples

**Example 1: Wordpress deployed with MYSQL in the same Docker cloud**

The same subnet is used for each container and already exists in the cloud.

{% include code.html language="yaml" file="/yamls/wordpress-simple.yaml" ghlink="/docs/guide/optimizer/yamls/wordpress-simple.yaml" %}

**Example 2: Wordpress: Different Docker cloud for each container**

The same subnet is used for each container. The subnet must already exist in both clouds and be setup to span across two Docker clouds (e.g. using Weaveworks), allowing both containers to reach each other.

{% include code.html language="yaml" file="/yamls/wordpress-multi-cloud.yaml" ghlink="/docs/guide/optimizer/yamls/wordpress-multi-cloud.yaml" %}

**Example 3: Wordpress: Cloud priority set**

A prioritized list of Docker clouds is provided. The same subnet is used for each container. The subnet must already exist in both clouds and be setup to span across the two Docker clouds (e.g. using Weaveworks), allowing both containers to reach each other.

{% include code.html language="yaml" file="/yamls/wordpress-cloud-priority.yaml" ghlink="/docs/guide/optimizer/yamls/wordpress-cloud-priority.yaml" %}


{% endcapture %}


{% include templates/guide.md %}
