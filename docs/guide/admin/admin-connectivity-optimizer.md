---
title: Connectivity Optimizer
contributors:
- nicolas
---

{% capture overview %}

Composure Connectivity allows to visualize and provision connectivity for your clouds.

**Connectivity** means different things depending on the type of resources that are being connected. In **AWS** and **OpenStack**, connectivity means assigning security groups so that one VM can contact another. So it is like a firewall policy. In Docker Clouds, connecting **containers** means to create an overlay network, which is similar to a bridge.

- Visualize connections between resources (a.k.a. endpoints)

- Connect virtual machines to each other and containers.

- Select groups of VMs or containers to connect

- Specify protocol or protocols.

- Set subnets as endpoints

- Work with multiple endpoints at the same time.

{% endcapture %}


{% capture steps %}

### Connectivity per cloud type

A connectivity request is handled by Composure differently depending upon the cloud type:

| Cloud type    | How connectivity works                                 |
|---------------|--------------------------------------------------------|
| **OpenStack** | The endpoints refer to the VM instances in the OpenStack cloud. The connect and disconnect request will modify the security group rules attached to the VMs in OpenStack and the firewall rules in the firewall as a service (FW-aaS) attached to the routers along the routing path between the two VMs. Composure adds and deletes rules that allow traffic from the source VM to the destination VM to the security group and firewall policies. A look at the Security Groups and FWaaS configurations in the OpenStack cloud will reflect this. |
| **AWS**       | The endpoints refer to the EC2 instances in the AWS cloud. The connect and disconnect request will modify the security group rules attached to the instances. Composure adds and deletes rules that allow traffic from the source instance to the destination instance for both of the security groups that are attached to the two instances respectively. |
| **Docker**    | The endpoints refer to the containers in the Docker cluster, and the connect and disconnect request will create or delete an **overlay network** in the docker cluster and attach or detach the two containers to and from the overlay network. A look at the network configuration in Docker container cloud will reflect this.

### Connectivity options

You can specify multiple nodes in the source and destination as well by clicking on the **+** button to add nodes and the **-** button to delete them:

  <img src="/media/image91.png" width="624" height="342" />

Similarly, you can submit a group connectivity request by selecting the **group** tab in the control panel.

  <img src="/media/image92.png" alt="Connectivity.png" width="310" height="310" />

Add nodes by clicking on the nodes and submit the request by clicking on the **connect** or **disconnect** button.

You can specify the protocol for the connection. Supported protocols are: FTP, SSH, SMTP, DNS, HTTP, POP3, NNTP, NTP, IMAP, SNMP, IRC, HTTPS. User can put multiple protocols at a time separated by comma, e.g: HTTP,HTTPS,SSH. Customized protocols are also supported, user can type in e.g: TCP:2379,TCP:3396 in the protocols text box.

  <img src="/media/image93.jpg" alt="cnn_group_multi.jpeg" width="624" height="336" />

### Example

1. Click **Tools ** **Connectivity.** The center panel shows the connectivity graph over the topology context. The light blue solid connectivity edges show the current connectivity between cloud endpoints, which are shown as blue nodes.

    <img src="/media/image84.jpg" alt="cnn_dash.jpg" width="624" height="336" />

1. In the right panel, all the connectivity policy visible to the current user are listed in a sortable policy table. The content of the table can also be filtered by the search function on the top right corner. E.g, user can search policy by source/destination name or specific protocol like ssh/https/http. The remove button in the operation column of each row allows the user to edit each rule. It functions as disconnecting the particular connectivity.

1. In the left panel, select the **Pair** button and **Source** text box and then select the endpoint green node. The screen shows details about the endpoint. For example, in the graph below, if the name of the selected endpoint were **vm-db-1**:

    <img src="/media/image85.jpg" alt="cnn_select_source.jpg" width="624" height="337" />

    Then click on the Destination text box and select another endpoint by clicking on the green node in the graph named **vm-db-2**:

    <img src="/media/image86.jpg" alt="cnn_select_dst.jpg" width="624" height="334" />

1.  The two selected endpoints will appear in the Control Panel. Click **Connect.** A dialogue box will confirm the selection.

    <img src="/media/image87.png" alt="cnn_connect_popup.png" width="378" height="204" />

    Click **Apply Changes**. The dialogue box will show the requested information containing the source and destination and protocol information and the progress of the request:

    <img src="/media/image88.jpg" alt="cnn_change_inProgress.jpg" width="381" height="241" />

    If the request is resolved and successful, the progress bar will show the status **SUCCESS**. Otherwise, the status will show **Fail**.

    <img src="/media/image89.jpg" alt="cnn_change_success.jpg" width="382" height="242" />

Close the dialogue box. The web page will refresh automatically. Also, you can manually click on the refresh button at the bottom of the control panel to enforce refreshing the graph data. A new edge between **vm-db-1** and **vm-db-2** shows the connection between them. You can click on the connectivity edge to see details.

  <img src="/media/image90.jpg" alt="cnn_edge_attr.jpg" width="624" height="330" />


{% endcapture %}


{% include templates/guide.md %}
