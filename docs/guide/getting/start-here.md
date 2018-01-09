---
title: Start Here
author: nicolas
---

{% capture overview %}

This page shows how to connect to your first cloud and deploy your first service.

{% endcapture %}

{% capture steps %}

## Login

Access the Composure web console from your web browser via HTTPS (port 443):

    https://URL_TO_YOUR_COMPOSURE_SERVER

Enter the username and password. The initial admin login for Composure is:

> **Username:** admin
>
> **Password:** admin

Click through the warning to add the root CA certificate if this is the first time you logged in.

<img src="/media/image3.png" alt="screenshot-01.png" width="269" height="295" />

Usernames are **not** case-sensitive.

Default user account when the software is first installed:

| Username               | Password     | Role         |
|------------------------|--------------|--------------|
| admin                  | admin        | admin        |


**Change passwords**:

1. Log in as the user (e.g. **admin/admin**)

1. Click the **Profile  Change Password** menu

    <img src="/media/image4.png" width="280" height="313" />

1. Enter the existing password and the new password in the appropriate fields.

    <img src="/media/image5.png" width="512" height="271" />

1.  Click the **Submit** button to update the password.


## Adding a cloud

### Overview

The Composure treats clouds as entities that can be mounted and unmounted at runtime, similar to mounting filesystems in Unix.

#### Prerequisites

Composure supports the following clouds:

<table markdown="0">

  <thead>
  <tr>
      <th>
        Cloud
      </th>
      <th>
        Prerequisite
      </th>
  </tr>
  </thead>

  <tbody>
  <tr>
      <td>
        <b><a href="#amazon-web-services-(aws)">AWS</a></b>
      </td>
      <td>
        One or more AWS accounts. Each mounted AWS Cloud corresponds to a different AWS account.<br>
        The Secret Access Key and Access Key ID.<br>
        The IAM policy required for the AWS user used to mount your AWS cloud to Composure must have at least the following IAM policies:<br>
        <ul>
          <li>AmazonEC2FullAccess</li>
          <li>AmazonVPCFullAccess</li>
        </ul>
      </td>
  </tr>

  <tr>
    <td>
        <b><a href="#microsoft-azure">Azure</a></b>
    </td>
    <td>
      One or more Azure accounts. Each mounted Azure Cloud correspond to an Azure resource group (must exist) in a specific Azure region.<br>
      Ask your Azure cloud administrator to create an application via the Azure portal to obtain those credentials.<br>
      For more information: How to get an Application ID and key:<br>
      https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal.
    </td>
  </tr>

  <tr>
    <td>
      <b><a href="#docker">Docker</a></b>
    </td>
    <td>
      One or more Docker hosts (VMs or bare-metal). The Docker superuser password.
    </td>
  </tr>

  <tr>
    <td>
      <b><a href="#openstack">OpenStack</a></b>
    </td>
    <td>
      One or more OpenStack clusters.
    </td>
  </tr>
  </tbody>
</table>

### Amazon Web Services (AWS)

In order to set up your **AWS** cloud with Composure, you need to create an cloud mount file, save it in Composure and then mount your Azure cloud. Cloud mount files define clouds to be mounted and discover while AppSpec files are YAML based configuration files services to create and run.

1. Click **Tools \> Composer**.

1. Click **New** at the top of the screen.

    <img src="/media/image6.png" width="624" height="385" />

1. Copy the following cloud mount template

    ```yaml
    cloud :
        provider : aws                      # Connecting to an AWS account &amp; specific AWS region>
        name : cloud_name                   # Specify a unique name for this cloud>
        region : region_id                  # Specify the AWS region (i.e. us-west-2)>
        username : IAM_user_name            # IAM user name for this AWS account>
        endpoint : access_key_id            # AWS access key ID. (e.g. AKAIXXXXXXXXXXXX)>
        password : aws_access_secret_key    # Optional: AWS secret access key>
                                            # If not provided here, user will be
                                            # prompted when mounting the cloud via>
                                            # the Composure CLI
    ```
    **Keys**

| Key      | Description                                                                                                                                                      |
|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name     | Name you want assigned to this cloud.                                                                                                                            |
| provider | Type of cloud (aws).                                                                                                                                             |
| region   | AWS region (e.g. us-west-2).                                                                                                                                     |
| username | AWS IAM username.                                                                                                                                                |
| endpoint | AWS access key ID.                                                                                                                                               |
| password | AWS secret access key (optional) associated to the Access Key ID. If omitted, the password (secret access key) is requested when mounting the cloud via the CLI. |

1. Paste this cloud mount configuration into the text area of the editor and set the values specific for your cloud.

    <img src="/media/image7.png" width="624" height="488" />

1. Click **Save Options \> Save As New** (or CTRL-S or CMD-S).

    Note: **DO** **NOT** press submit. You only submit services and mount clouds.

1. Enter a **file name** when prompted and click the **OK** button. The extension \*.yaml will be automatically added.

    <img src="/media/image8.png" width="214" height="124" />

1. Go to **Tools \> CLI.**

1. Type the **ls** command to list all the AppSpec files in your local Composure library

1. You should see the file you just created.

1. Type the following command to add your cloud to Composure:

        Composure> mount your-cloud-file-name

1. Where **\<your-cloud-file-name\>** is the name you set in the AppSpec file.

1. Enter the Composure host’s superuser password and then the AWS user’s password:

        Composure> mount _<your-cloud-file-name>_
        sudo.password (Superuser password to mount a cloud):******
        _<cloud_name>_.password (if any):********
        Re-enter _<cloud_name>_.password:********
        cloud_name was successfully installed.
        Cloud.properties successfully modified with the 1st cloud information.
        Discovery started for cloud _<cloud_name>_

Give Composure a few minutes to mount and discover your AWS cloud and discover all existing resources already deployed if any (virtual machine, networks, etc). Then go to **Tools \> Graph** to [view your new cloud](#cloud-graph).



### Microsoft Azure

In order to set up your **Azure** cloud with Composure, you need to create a cloud mount file (YAML based), save it in Composure and then mount your Azure cloud. An Azure Cloud in Composure maps to an Azure resource group defined in a specific Azure region (e.g eastus).

You will need Azure credentials related to your Azure account and obtain an Application ID, Application Key, Azure AD Directory ID/URL, Subscription ID for Composure to access and create resources in your Azure account.

Ask your Azure cloud administrator to create an application via the Azure portal to obtain those credentials. For more information: How to get an Application ID and key: [https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal).

1. Click **Tools \> Composer**.

1. Click **New** at the top of the screen.

    <img src="/media/image6.png" width="624" height="385" />

1. Copy the following cloud mount template:

    ```yaml
    cloud:
        name : <your_azure_cloud_name>            # Unique cloud name in Composure
        provider : azure                          # Composure cloud type
        username : <azure_application_ID>         # Application ID
        password : <azure_application_key>        # Application Key
                                                  # Azure AD directory ID (URL)
        endpoint : https://login.windows.net/xxxxxxxxxxxxxxxxxx/oauth2/token
        resourcegroup : <azure_resource_group>    # Resources group name existing
                                                  # in this Azure account
        subscriptionid : <azure_subscription_id>  # Subscription ID
        region : <azure_region>                   # Azure region (e.g eastus) - lowercase
                                                  # https://azure.microsoft.com/en-us/regions/
    ```
    **Keys**

| Key            | Description                                                    |
|----------------|----------------------------------------------------------------|
| name           | Name you want assigned to this cloud, unique in Composure      |
| provider       | Type of cloud (must be “azure” for mounting to an Azure Cloud. |
| region         | Azure Region (e.g. eastus) - Lowercase                         |
| resourcegroup  | Azure resources group name                                     |
| username       | Azure Application ID                                           |
| password       | Azure Application Key                                          |
| endpoint       | Azure AD directory ID (URL)                                    |
| subscriptionid | Azure Subscription ID                                          |

1. Paste this cloud mount configuration into the text area of the editor and set the values specific for your cloud.

    <img src="/media/image9.png" width="636" height="481" />

1. Click **Save Options \> Save As New** (or CTRL-S or CMD-S).

    Note: **DO** **NOT** press submit. You only submit services and mount clouds.

1. Enter a **file name** when prompted and click the **OK** button. The extension \*.yaml will be automatically added.

    <img src="/media/image10.png" width="249" height="150" />

1. Go to **Tools \> CLI**.

1. Type the **ls** command to list all the AppSpec files in your local Composure library

1. You should see the file you just created.

1. Type the following command to add your cloud to Composure:

        Composure> mount your-cloud-file-name

1. Where **your-cloud-file-name** is the name you set in the AppSpec file.

1. Enter the Composure host’s superuser password and then the AWS user’s password:

        Composure> mount _<your-cloud-file-name>_
        sudo.password (Superuser password to mount a cloud):******
        _<cloud_name>_.password (if any):********
        Re-enter _<cloud_name>_.password:********
        cloud_name was successfully installed.
        Cloud.properties successfully modified with the 1st cloud information.
        Discovery started for cloud _<cloud_name>_

Give Composure a few minutes to mount and discover your Azure cloud and discover all existing resources already deployed if any (virtual machine, networks, etc). Then go to **Tools \> Graph** to [view your new cloud](#cloud-graph).


### Docker

In order to setup your Docker cloud with Composure, you’ll need to create a Cloud mount configuration file, save it in Composure and then mount your Docker cloud based on that cloud mount file. At least one or more Docker hosts running a Docker engine with Docker version 1.10 or higher must already be deployed and accessible from Composure over your IP network.

1. Click **Tools \> Composer**.

1. Click **New** at the top of the screen.

    <img src="/media/image6.png" width="552" height="340" />

1. Copy the following and set the values specific to your cloud.

    **Minimum cloud mount file example:**

    ```yaml
    cloud:
        name : <cloud_name>
        provider : docker
        admin.username : mosaix
        endpoints : http://dkr_host_ip1:4243/, http://dkr_host_ip2:4243
    ```

    **Complete cloud mount file example:**

    ```yaml
    cloud:
        name: <cloud_name>
        provider: docker
        endpoints: http://dkr_host_ip1:dkr_port, http://dkr_host_ip2:dkr_port

        # Secret keys for Composure to secure access docker hosts
        tlsverify: true # if true, the following three fields are required
        tlscacert: /path/to/cacert
        tlscert: /path/to/cert
        tlskey: /path/to/key

        # Overcommit ratios:
        overcommit.cpu: 10
        overcommit.memory: 1

        admin.username: user
        enable.monitoring: true

        # Proxy for Composure to reach docker hosts:
        proxy.username: proxyuser
        proxy.endpoint: http://x.x.x.x:2340/
        sensing.enabled: true

        # Connecting the cloud to Docker registries including Docker Hub accounts:
        registries:
          index.docker.io:
            username: mike
            password: s3cret
          somewhere.com:
            username: bob
            password: s3cret
          somewhere-else.org:
            username: john
            password: s3cret

        # Chargeback settings:
        chargeback:
            # chargeback for CONTAINER/VM
            service:
                cpu: 0.032 # 1 cpu cost per time unit
                memory: 0.012 # memory cost per time unit
                disk: 0.01 # disk cost per time unit
                flavor: #flavors' cost for the containers created in this cloud
                t1.small: 0.352
                f2.large: 0.212
            # chargeback for docker hosts
            operation:
                cpu: 0.32
                memory: 0.012
                disk: 0.01
                flavor: # flavors' cost for physical servers of the docker cloud
                f1: 0.352
                f2: 0.212
    ```
    **Keys**

| Key               | Description                                    |
|-------------------|------------------------------------------------|
| name              | Name you want use for this cloud.              |
| provider          | Type of cloud (docker)                         |
| admin.username    | Admin username for the Docker hosts            |
| admin.password    | Admin password for the Docker hosts (optional) |
| endpoints         | At least one Docker host endpoint. Make sure the IP address of the Docker hosts is reachable by Composure over your IP network. Multiple endpoints are separated by a comma. The port of the Docker REST API must specified. The default factory port used by the Docker engine is 4243.                      |
| overcommit.cpu    | Ratio that defines the maximum number of virtual CPUs. This is the actual CPU count of each host multiplied by the overcommit.cpu ratio. (optional, Default = 1)                |
| overcommit.memory | Ratio that defines the maximum amount of virtual memory. This is the actual amount of memory of each host multiplied by the overcommit.memory ratio. (optional, default = 1).   |


1. Click **Settings  Save**.

1. Enter a **template name** when prompted and press **OK**.

    <img src="/media/image11.png" alt="cloud-dkr-yml-file-name.png" width="189" height="112" />

1.  Click **Tools \> CLI**.

1.  Type the **ls** command to list all the AppSpec files.

1.  You should see the file you created.

1.  Type the following command to add your cloud to Composure:

        Composure> mount your-cloud-file-name

1.  Where **your-cloud-file-name** is the name you set in the AppSpec file.

1.  Enter the Composure host’s superuser password and then the AWS user’s password:

        Composure> mount _<your-cloud-file-name>_
        sudo.password (Superuser password to mount a cloud):******
        _<cloud_name>_.password (if any):********
        Re-enter _<cloud_name>_.password:********
        cloud_name was successfully installed.
        Cloud.properties successfully modified with the 1st cloud information.
        Discovery started for cloud _<cloud_name>_

Give Composure a few minutes to mount and discover your AWS cloud and discover all existing resources already deployed if any (virtual machine, networks, etc). Then go to **Tools \> Graph** to [*view your new cloud*](#cloud-graph).


### OpenStack

The instructions for OpenStack are basically the same as Azure and the others. The only difference is the OpenStack AppSpec, like the sample shown below.

1. Click **Tools \> Composer**.

1. Click **New** at the top of the screen.

    <img src="/media/image6.png" width="498" height="308" />

1. Copy the following and set the values specific to your cloud.

    ```yaml
    cloud:
        name : OpenStack_Cloud
        provider : openstack
        username : tenant_name:user_name
        password : tenant_password
        region : RegionOne
        endpoint : http://198.178.162.130:5000/v2.0/
        overcommit.cpu : 8
        overcommit.memory : 1
        admin.username : tenant_name:admin_user_name
        admin.password : admin_password
        external_accesspoint : xxx.xxx.xxx.xxx
    ```
    **Keys**

| Key                   | Description                                                                                                                                                                   |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name                  | Name to assign to this cloud.                                                                                                                                                 |
| provider              | Type of cloud (openstack).                                                                                                                                                    |
| username              | Username of a tenant formed as tenant\_name:user\_name.                                                                                                                       |
| password              | Password of the tenant.                                                                                                                                                       |
| region                | Name of the region to be connected to.                                                                                                                                        |
| admin.username        | Admin username for the OpenStack cluster. Required to connect to Ceilometer and infrastructure details (scheduler) on OpenStack.                                              |
| admin.password        | Admin password for the OpenStack cluster (optional).                                                                                                                          |
| endpoint              | URL (https://<IP_address>) of the OpenStack cluster (REST API endpoint). The port to connect to OpenStack must specified. The default factory port used is 5000. Make sure this port is not blocked by a firewall. |
| overcommit.cpu        | Ratio that defines the maximum number of virtual CPUs. This is the actual CPU count of each host multiplied by the overcommit.cpu ratio. (optional, Default = 1).             |
| overcommit.memory     | Ratio that defines the maximum amount of virtual memory. This is the actual amount of memory of each host multiplied by the overcommit.memory ratio. (optional, default = 1). |
| external_accesspoint | IP address used to reach user applications running on OpenStack from outside of the OpenStack cluster.                                                                         |

1. Click **Settings \> Save**.

1. Enter a **template name** when prompted and press **OK**.

    <img src="/media/image12.png" width="199" height="121" />

1. Click **Tools \> CLI**.

1. Type the **ls** command to list all the AppSpec files.

1. You should see the file you created.

1. Type the following command to add your cloud to Composure:

        Composure> mount your-cloud-file-name

1. Where **your-cloud-file-name** is the name you set in the AppSpec file.

1. Enter the Composure host’s superuser password and then the AWS user’s password:

        Composure> mount _<your-cloud-file-name>_
        sudo.password (Superuser password to mount a cloud):******
        _<cloud_name>_.password (if any):********
        Re-enter _<cloud_name>_.password:********
        cloud_name was successfully installed.
        Cloud.properties successfully modified with the 1st cloud information.
        Discovery started for cloud _<cloud_name>_

Give Composure a few minutes to mount and discover your AWS cloud and discover all existing resources already deployed if any (virtual machine, networks, etc). Then go to **Tools \> Graph** to [view your new cloud](#cloud-graph).



## Viewing your cloud

The Cloud Graph displays the clouds, showing which nodes are connected to which and providing details about each. Click **Tools \> Graph** to view.

<img src="/media/image13.png" width="624" height="473" />

Click on the boxes on the left to display nodes in the graph. Click on a node in the graph to display its attributes in a box on the right. (We use the term **graph** in the mathematical sense, meaning a set of nodes and edges \[lines showing how they connect to each other\].)

See the [Cloud Graph](#cloud-graph) section for more details on how to use the Cloud Graph.



## Search your cloud

Cloud Search provides a robust search engine to find resources in your cloud. Click **Home** (Composure.ai logo on the top left) to search your cloud. Click on any of the predefined search tags (Cloud, Container, vm, etc.) or in the search box, type **all** and press **Enter** to search for all resources in your cloud.

<img src="/media/image14.png" width="624" height="532" />




## Add a new service

Assuming an AWS cloud was set up and mounted as described in the previous section (see how to add an [AWS Cloud](#amazon-web-services-(aws)) to Composure), we will now create and stop a VM service (an Ubuntu EC2 instance) on our AWS cloud using the Cloud Composer. The example below serves as a quick way to see how Composure services are created. See the Cloud Composer section for more details.

### Deploy a VM in AWS

We are going to create an YAML based AppSpec file that will describe the new VM.

1. Click **Tools \> Composer**.

1. Click the **New** button to open a new template.

    <img src="/media/image6.png" width="585" height="361" />

1. Copy and paste the following AppSpec file in the editor.

    ```yaml
    service:
        type: VM                  # VM as a Service - Reserved key for Composure
        alias: VM_service_name    # Name of the service in Composure

        cloud: AWS_cloud_name     # Replace with Cloud name existing in Composure
        subnet: subnet_CIDR_name  # Replace with Subnet name existing selected Cloud

        prefix: aws_name_tag      # Sets the 'Name' tag for the EC2 instance
                                  # If 'prefix' used instead of 'name', Composure
                                  # extends the name with a unique and random string.

        imageid: AWS-AMI          # Choose an AWS image ID (aka AMI) must exist
                                  # (i.e. ami-xxxxxxxxx) in the region of the

        flavor: t2.micro          # Any AWS Instance type (e.g.: c3.xlarge)
                                  # supported in the region of the selected Cloud.
    ```

1. Select **Save Options \> Save as new** to save your new Appspec file

    <img src="/media/image15.png" width="562" height="341" />

1. Replace the settings in your new Appspec with the appropriate desired value in the editor and save the change (**CTRL+S** or **Save Options  Save**)

| Key     | Description                                                                                                                                   |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| vm      | Service type must be **VM**.                                                                                                                  |
| alias   | The name of the service created in Composure.                                                                                                 |
| cloud   | The name of a cloud already mounted in Composure.                                                                                             |
| subnet  | The name of the subnet which already exists in the selected AWS cloud and discovered by Composure.                                            |
| prefix  | The name of the instance in AWS.                                                                                                              |
| imageid | AWS image id in your AWS account (does not need to be discovered by Composure initially).                                                     |
| flavor  | AWS Instance type in AWS (e.g.: t2.small, c4.4xlarge, etc.). Must be supported in the AWS region (e.g. us-west-1) for the selected AWS cloud. |

  **Tips:**

  - You can find the subnet name via [*Cloud Search*](#cloud-search) by searching for: **type=subnet.**

  - You can find an AWS AMI available in the AWS region that you mounted as a cloud in Composure from your AWS web console:
    [https://us-west-2.console.aws.amazon.com/ec2/v2/home?region=us-west-2\#Images:visibility=public-images;sort=name](https://us-west-2.console.aws.amazon.com/ec2/v2/home?region=us-west-2#Images:visibility=public-images;sort=name)

  - For an Ubuntu VM, you can also find the AWS AMI supported in the selected region from this link:
    [https://cloud-images.ubuntu.com/locator/ec2/](https://cloud-images.ubuntu.com/locator/ec2/)

**Follow these steps:**

1. Click the **Submit** button to submit the service. Submit means to create the service. You start it under the **actions** screen shown below.

    <img src="/media/image16.png" width="517" height="366" />

1. An information alert appears on the top right below the main menu, showing your service got submitted successfully with a unique service id assigned to it:

    <img src="/media/image17.png" width="576" height="104" />

1.  Go to the **Service** pane to see your submitted service in the list of services.

    <img src="/media/image18.png" width="586" height="342" />

1.  Select the service in the service list, then click on the “**Actions**” button to select “**Running**” to run your service and deploy the requested resources:

    <img src="/media/image19.png" width="502" height="262" />

    The following alert appears to confirm your request to run the service was received:

    <img src="/media/image20.png" width="519" height="67" />

1.  Click on the<img src="/media/image21.png" width="43" height="39" />button to refresh the view (upper right) until you see the transition to the desired state for the service set to “Running”:

    <img src="/media/image22.png" width="624" height="273" />

    The **Desired state** of the service changes from **REQUEST** to **RUNNING**.

1.  Click on the service and at the bottom of the page select **Inspect** for more details. This is a log of what is happening with the service now.

    <img src="/media/image23.png" width="624" height="320" />

    **Note**: You can use [Cloud Search](#cloud-search) to find your newly created VM service by entering **name=name-of-your-vm** into the search box and pressing **Enter** on your keyboard.


### Terminate a service

1.  Click on the service you want to terminate in the **Service** pane.

1.  Click the service so to highlight the line item.

1.  Select **Actions \> Terminate** from the pull down menu under the **Service Editor.**

1.  Click on the <img src="/media/image21.png" width="43" height="39" /> **refresh** icon to refresh the status of the service(s) in the **Service Pane**. It will not update by itself.


### Delete a service

**Recommendation!**: Terminate your service before deleting it, to safely decommission all cloud ressources the service created.

1.  Click on the service you want to delete in the **Service** panel.

1.  Right-click on the service you want to remove and select **Delete** .

1.  Click **Delete** to confirm the selection.

    <img src="/media/image24.png" width="423" height="157" />


## Removing your cloud

A mounted cloud named cloud-name can be unmounted by running the unmount command from the Composure web CLI (Go to **Tools \> CLI**), as illustrated below:

  ```shell
  Composure> unmount <cloud-name>
  sudo.password (Superuser password to mount a cloud):
  Cloud <cloud-name> has been successfully unmounted.

  Removal process started for clouds/storage managers <cloud-name>.
  File cloud.properties successfully modified.

  Composure> list-clouds
  Clouds available in the cloud.properties file:
    -none-
  ```

Enter Composure host’s superuser password. After unmounting a cloud, the discovered nodes detected during the mount process will be removed from Composure. Enter the command ‘list-clouds’ to verify that the cloud has been removed.

{% endcapture %}

{% include templates/guide.md %}
