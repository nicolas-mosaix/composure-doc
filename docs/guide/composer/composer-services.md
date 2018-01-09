---
title: Services
contributors:
- nicolas
---

{% capture overview %}

This page shows how to list, monitor, search, inspect services through the Services tab.

{% endcapture %}


{% capture steps %}

### List services

In the services tab, each submitted service shows up as a line item. Click the item to bring it into scope to apply actions from the **Action** button and find more information about each service (See Details, Compose, Inspect tabs). Press the **refresh** icon to update the state.

<img src="/media/image41.png">

Information displayed for each service in this tab:

| Field         | Description                                                                                                                                                         |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name          | Alias specified in the appspec file for this service                                                                                                                |
| State         | The current state of the service (e.g.: Request, Pending, Running, Terminated, or Failed)                                                                           |
| Desired state | The desired state of the service (e.g.: Running, Terminate, Delete) based on the desired state selected by the user in the Actions button.                          |
| Type          | The [service type](/docs/guide/composer/composer-service-types) (CONTAINER, VM, COMPOSITE, NETWORK, etc.)                                                                                      |
| ID            | A unique service ID automatically assigned by Composure. Only the last 4 digits are displayed. Select the service and the “Details” tab to see its full service ID. |
| Children      | Number of sub-services in a composite service.                                                                                                                      |
| Creation time | Date and time when the service was submitted.                                                                                                                       |
| Uptime        | Total time since the service was submitted.                                                                                                                         |

### Monitor service states

The service state and desired state may change to any of the following states:

<table>

<thead>
<tr>
<th>State</th>
<th>Description</th>
</tr>
</thead>

<tbody>
<tr>
<td><img src="/media/image42.png" width="200" height="21" /></td>
<td>Service is in Request state, was submitted and ready to be run (<b>Actions \> Running</b>) or deleted (<b>Actions \> Delete</b>).</td>
</tr>

<tr>
<td><img src="/media/image43.png" width="91" height="24" /></td>
<td>Service is running</td>
</tr>

<tr>
<td><img src="/media/image44.png" width="85" height="22" /></td>
<td>Service is moving to a desired state but has not yet reached that state.</td>
</tr>

<tr>
<td><img src="/media/image45.png" width="74" height="22" /></td>
<td>Service failed. If this is a composite service....</td>
</tr>

<tr>
<td><img src="/media/image46.png" width="82" height="21" /></td>
<td>The connection is effectively established (e.g. security rules set in security groups). This state applies only to CONNECTIVITY service types (PAIR\_CONNECTIVITY, GROUP\_CONNECTIVITY). See [*CONNECTIVITY service type*](#connectivity) for more information.</td>
</tr>

<tr>
<td><img src="/media/image47.png" width="98" height="24" /></td>
<td>The connection is effectively removed (e.g. security rules removed from security groups). This state applies only to CONNECTIVITY service types (PAIR\_CONNECTIVITY, GROUP\_CONNECTIVITY). See [*CONNECTIVITY service type*](#connectivity) for more information.</td>
</tr>
</tbody>

</table>


### Filter and search services

Above the service list is a search bar where you can filter services by name (alias):

<img src="/media/image48.png">

Below the search bar are tags used to filter services by their current state (All, Request, Pending, Running, Stop, Terminated, Failed). You can select only one tag at a time.

### Inspecting a service

Select a service to view more detailed information about it in the **Details**, **Compose** (or Appspec), and **Inspect panes** at the bottom:

<table>

<tbody>
  <tr>
    <td><b>Details</b></td>
    <td>
          List all instances (VM or containers) deployed by the service. Shows the total number of instances irrespectively of its status.<br>
          The list displays the following information about each instances:<br>
          <ul>
            <li> Status (UP, DOWN or ERROR)</li>
            <li> Actual name (e.g. container name in docker or name tag)</li>
            <li> Name of the cloud where it i deployed</li>
            <li> Name of the image it is based on</li>
            <li> Type of the instance (VM or CONTAINER)</li>
            <li> IP address of the host it is running on (physical server for a VM, virtual machine for a CONTAINER)</li>
            <li> Public IP address if instance connected to a public network</li>
            <li> Private IP (related to the subnet it is attached to)</li>
          </ul>
          <br>
          If a service failed, error messages may be displayed here instead.<br>
    </td>
  </tr>

  <tr>
    <td><b>Compose</b></td>
    <td>
    Displays the original or current YAML based appspec used to run the service. After a service is submitted from a selected appspec file, it is no longer associated to it. Modifying an appspec file in the Composer editor does not affect already services previously submitted from it.
    </td>
  </tr>

  <tr>
    <td><b>Inspect</b></td>
    <td>
    Displays very detailed information about the service (same concept as the output from a “docker inspect container_id” command)
  </td>
  </tr>
</tbody>
</table>

#### Details

<img src="/media/image49.png">

#### Compose

Original appspec used to submit the service:

<img src="/media/image50.png" width="227" height="239" />

#### Inspect

<img src="/media/image51.png" width="363" height="352" />


{% endcapture %}


{% include templates/guide.md %}
