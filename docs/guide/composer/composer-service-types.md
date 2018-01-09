---
title: Service Types
contributors:
- nicolas
---

{% capture overview %}

This page shows the list of supported service types.

{% endcapture %}


{% capture steps %}

The table below provides the types of services you can define using Cloud Composer. Those are defined by keys.

| Service Type                                 | Description                                                                                                                                               |
|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| [VM](#vm)                                    | A virtual machine instance on AWS or OpenStack.                                                                                                           |
| [VM\_GROUP](#vm_group)                       | A group of virtual machine instances.                                                                                                                     |
| [CONTAINER](#container)                      | A Docker container.                                                                                                                                       |
| [CONTAINER\_GROUP](#container_group)         | A group of containers, defined as a service on Docker.                                                                                                    |
| [GROUP\_CONNECTIVITY](#group_connectivity)   | Connects grouped services.                                                                                                                                |
| [PAIR\_CONNECTIVITY](#pair_connectivity)     | Connects and disconnects VMs (AWS or OpenStack) or Containers (Docker) with the appropriate network overlays, security and firewall rules.                |
| [LOAD\_BALANCER](#load_balancer)             | Manages load-balancing across specific servers in the underlying clouds.                                                                                  |
| [AUTOSCALER](#autoscaler)                    | Automatically scale the resources needed by the application based on metrics gathered from the Composure monitoring service and thresholds the user sets. |
| [COMPOSITE](#composite)                      | Encapsulates a certain number of other services. Any of the services above can be a subservice.                                                           |
| [DOCKER_CLOUD](#docker_cloud)                | Deploy and mount a Docker Cloud with a specified numnber of Docker hosts in a selected cloud such as AWS, OpenStack or Azure.   |
| [CLONE](#clone)                              | Create a new service from selected containers running in a Docker cloud.   |

The following keys apply to all service types:

| Key   | Description                                        | Required |
|-------|----------------------------------------------------|----------|
| name  | The name of the service within your cloud provider | No       |
| alias | Referenced in the Composure UI                     | No       |


### VM

The **VM** service type is for creating and running VM instances on both AWS and OpenStack clouds.

#### Supported states

When the service is created, it will have current state and target state as REQUEST. The flowchart below shows the service’s allowed state transitions. For example, we can ask the service to change to the target state RUNNING. Later we can ask the service to STOP.

| State         | Description                                  |
|---------------|----------------------------------------------|
| **REQUEST**   | Service created.                             |
| **RUNNING**   | VM/Instance has been created in the cloud.   |
| **STOP**      | VM/Instance has stopped.                     |
| **TERMINATE** | VM/Instance has been deleted.                |

#### Keys


| Key               | Type        | Description                                                                     | Required  |
|-------------------|-------------|---------------------------------------------------------------------------------|-----------|
| type              | VM          | The service type.                                                               | Yes       |
| alias             | string      | Name of the service.                                                            | No        |
| prefix            | string      | A prefix to prepend to the name of the VM to make its name unique in Composure. | No        |
| name              | string      | Name assigned to the VM in Composure.                                           | No        |
| cloud             | string      | Name of a cloud in Composure.                                                   | Yes       |
| imageid           | string      | AWS AMI for AWS images. Must exist in the targeted region/cloud                 | Yes, if image is not provided         |
| image             | string      | Image Name in AWS or OpenStack.                                                 | Yes, if imageid not provided          |
| subnet            | string      | Subnet name in Composure to connect the VM to.                                  | No, use if subnetid not provided      |
| subnetid          | string      | Cloud ID in Cloud of the subnet to connect the VM to.                           | No  |
| flavorid          | string      | Cloud ID in Composure .                                                         | Yes, or provide cpu, memory and disk                            |
| flavor            | string      | Instance type/flavor name.                                                      |           |  
| cpu               | integer     | Number of CPUs to allocate to the VM.                                           | Yes, if flavorid or flavor is not set. Only used for OpenStack VMs. |
| memory            | integer     | Memory size to allocate in MBs to the VM.                                       |           |  
| disk              | integer     | Disk size to allocate in GBs to the VM.                                         |           |
| keypair           | string      | ssh key to attach to the VM.                                                    | No        |
| extaccess         | true/false  | Allow VM to have public access                                                  | No        |
| serverid          | string      | Host ID for the VM.                                                             | No        |
| server            | string      | Host name for the VM.                                                           | No        |
| avoid_scheduler   | true/false  | This option skips using the schedule. But if the user does not specify a specific server, and if the cloud does not provide a default server, the service will fail. | No |
| avoid_monitoring  | true/false  | Avoid monitoring system used by the scheduler.                                  | No        |
| status            | string      | Final status of the VM such as UP or DOWN.                                      | No        |


#### Examples

**Example 1:**

```yaml
service:
    alias: db
    type: VM
    # Cloud name in Composure where to create the VM
    cloud: cloud-name
    imageid: aws_ami_id or openstack_img_id
    # Existing subnet name in the above selected Cloud in Composure
    subnet: subnet1
    # Instance type in AWS or Flavor in OpenStack
    flavor: t2.small
```

**Example 2:**

```yaml
service:
    alias: db
    type: VM
    # Name to be assigned to the VM in Composure
    name: sample-vm
    # Cloud name in Composure where to create the VM
    cloud: cloud-name
    imageid: ami-9abea4fb
    # Existing subnet name in the above selected Cloud in Composure
    subnet: subnet1
    # Instance type in AWS or Flavor in OpenStack
    flavor: t2.small
    keypair: t2key
    # Name of an AWS instance in Composure
    server: aws-instance-1
    # Settings currently required when explicitly setting a server
    avoid_scheduler: true
    avoid_monitoring: true
    status: UP
```

### VM_GROUP

The **VM\_GROUP** service type is for creating and running multiple VM instances on both AWS and OpenStack clouds.

**VM\_GROUP** has all the same keys as the **VM** service type with the addition of one key, which is defined in the table below.

#### Keys
| Key   | Type    | Description                       | Required |
|-------|---------|-----------------------------------|----------|
| count | integer | Number of VM instances to create. | Yes      |

#### Example

The **VM** examples can be used for **VM\_GROUP.** The type key changes to VM\_GROUP and there is one line to add for the count key to the examples.

```yaml
service:
    alias: db
    type: VM_GROUP
    count: 3
    # Cloud name in Composure where to create the VM
    cloud: cloud-name
    imageid: aws_ami_id or openstack_img_id
    # Existing subnet name in the above selected Cloud in Composure
    subnet: subnet1
    # Instance type in AWS or Flavor in OpenStack
    flavor: t2.small
```

### CONTAINER

The **Container** service type is for creating and running containers on Docker clouds.

#### Keys

List of supported key to deploy a CONTAINER type service:

| Key               | Type            | Description                                                                                    | Required                                     |
|-------------------|-----------------|------------------------------------------------------------------------------------------------|----------------------------------------------|
| type              | CONTAINER       | Assign the type of the service.                                                                | Yes                                          |
| alias             | string          | Name of the service.                                                                           | No                                           |
| prefix            | string          | A unique prefix to attach to the name of the container.                                        | No                                           |
| name              | string          | Name to assign to container.                                                                   | No                                           |
| image             | string          | Container image name (name\:tag).                                                              | Yes, one                                     |
| imageid           | string          | Cloud ID in Composure for that image if discovered by Composure.                               |                                              |
| subnet            | string          | Subnet name for the container to use.                                                          | No. If not set, default is “host” network    |
| subnetid          | string          | Cloud ID in Composure for that subnet if discovered by Composure.                              | No. either use subnetid or subnet            |
| command           | string          | Optional command to run in the container, e.g., /usr/bin/top -b.                               | No                                           |
| environment       | list of strings | Environment variables to be set in the container.                                              | No                                           |
| ports             | list of strings | Port to bind to.                                                                               | No                                           |
| Volume            | list of Volume  | List of volumes to be created with the container. See [Volume keys](#volume-keys) below.       | No                                           |
| volume_binds      | list of strings | Volume binding info.                                                                           | No                                           |
| cpu               | integer         | Number of CPUs to allocate.                                                                    | No. If not supplied, the default values are set by the Docker Engine \(1 CPU and 512 MB\). |
| memory            | integer         | Memory to allocate in MBs.                                                                     |                                              |
| disk              | integer         | Disk size to allocate in GBs.                                                                  |                                              |
| cloud             | string          | Name of a cloud in Composure.                                                                  | Yes                                          |
| server            | string          | Name of the docker host.                                                                       | No, one of the two if set                    |
| serverid          | string          | Cloud ID in Composure of the Docker host.                                                      |                                              |
| avoid\_scheduler  | true\/false     | If true, a server or serverid must be specified                                                | No Defaults to false.                        |
| avoid_monitoring  | true\/false     | Do not use the monitoring system used by the scheduler.                                        | No. Defaults to false.                       |
| status            | string          | Final status of the container, such as UP or DOWN.                                             | No                                           |


#### Volume keys

| Key    | Type   | Description        | Required |
|--------|--------|--------------------|----------|
| name   | string | Name of the volume | No       |
| path   | string | Path of the volume | Yes      |
| driver | string | Volume driver      | No       |

#### Examples

Below are minimum and maximum AppSpec file examples for a Container service. The maximum example will have some entries commented out with “\#”, as those are redundant with another entry. If you want to use a commented out entry, remove the “\#” and either comment out the other redundant entry or delete it completely.

**Example 1**

```yaml
service:
    type: CONTAINER
    cloud: docker_cloud
    image: ubuntu:latest
```

**Example 2**

```yaml
service:
    type: CONTAINER
    alias: db
    cloud: docker_cloud
    subnet: my_overlay_ntwk
    image: mysql:latest
```

**Example 3: MySQL**

```yaml
service:
    type : CONTAINER
    cloud : cloudDocker
    imageid : ubuntu:latest
    subnet : 172.17.0.0
    # cpu : 1
    # memory : 100
    # disk : 5
    # prefix : test
    # server : taran-Lenovo-Y50-70
    # serverid : dock:200834399084029390984
    # command : /usr/bin/top -b
    # portbind : 3306:3306
    envar :
        - MYSQL_ROOT_PASSWORD=wordpress
        - MYSQL_DATABASE=wordpress
        - MYSQL_USER=wordpress
        - MYSQL_PASSWORD=wordpress
    avoidscheduler : false
```


### CONTAINER_GROUP

Container groups are used to create, delete or manage multiple containers as a single service.

**CONTAINER\_GROUP** has all the same keys as the **CONTAINER** service type with the addition of two keys, which are defined in the table below.

#### Keys

| Key    | Description                                 | Required               |
|--------|---------------------------------------------|------------------------|
| count  | Number of containers in the group.          | Yes                    |
| policy | Scheduler policy for container allocations. | No. Defaults to basic. |

#### Example

Any **CONTAINER** examples can be used for **CONTAINER\_GROUP.** The type key changes to CONTAINER\_GROUP. There is one line to add for the count key to the examples and an optional line to add for the policy key.

```yaml
[...]
    count: 3
    policy: workload_optimize
[...]
```

### COMPOSITE

The objective of the COMPOSITE service is to encapsulate a number of other service requests, including those with dependencies between them. This service type allows the following capabilities:
  - Managing multiple services in parallel or in given order.
  - Passing information across services.

Composite service takes a list of services and relations. Sub-services under composite service can be any other regular service type such as VM, CONTAINER or even another composite service. Relations under composite service defines the dependency among the services if any. Composite also assign dependencies based on the references in the services.

#### Keys

| Key           | Description                                                                                                              | Required |
|---------------|--------------------------------------------------------------------------------------------------------------------------|----------|
| type          | COMPOSITE                                                                                                                | Yes      |
| decomposition | List of services (any other service types such as CONTAINER, VM, SCHEDULER, AUTOSCALER, NETWORK, ROUTER, RELATION, etc.) | Yes      |

#### Examples

**Composite service with no relation**

The following AppSpec shows an example of a COMPOSITE service with two sub-services. This COMPOSITE service initiate both sub-services in parallel.

```yaml
service:
    type: COMPOSITE
    alias: MyWordpressAppHybrid1
    decomposition:
    - service:
        type: CONTAINER
        alias: db
        prefix: db
        cloud: devdocker1
        image: mysql:latest
        environment:
            - MYSQL_ROOT_PASSWORD=wordpress
            - MYSQL_DATABASE=wordpress
            - MYSQL_USER=wordpress
            - MYSQL_PASSWORD=wordpress

    - service:
        type: CONTAINER
        alias: wordpress
        prefix: wordpress
        cloud: devdocker2
        image: wordpress:latest
        ports:
        - :80
```

**Composite service with relation**

The following AppSpec shows a COMPOSITE service with two sub-services and one relation. ORDER is the type of the relation with before and after services keys. Here the COMPOSITE service will first start the db service before the wordpress service.

```yaml
service:
    type: COMPOSITE
    alias: MyWordpressAppHybrid1
    decomposition:
        - service:
            type: CONTAINER
            alias: db
            name: db1
            cloud: devdocker1
            image: mysql:latest
            environment:
                - MYSQL_ROOT_PASSWORD=wordpress
                - MYSQL_DATABASE=wordpress
                - MYSQL_USER=wordpress
                - MYSQL_PASSWORD=wordpress

        - service:
            type: CONTAINER
            alias: wordpress
            name: wordpress1
            cloud: devdocker2
            image: wordpress:latest
            environment:
                - WORDPRESS_DB_HOST=db1:3306
                - WORDPRESS_DB_PASSWORD=wordpress
            ports:
                - :80

        - relation:
            type: ORDER
            before: db
            after: wordpress
```

**Relation with ORDER service type**

Let say we have three services S1, S2 and S3

**Example #1**: S1  S2  S3 meaning S1 starts before S2, and S2 before S3.

```yaml
service:
    alias: sample1
    decomposition:
    - service:
      alias: S1
    - service:
      alias: S2
    - service:
      alias: S3
    - relation:
      type: ORDER
      before: S1
      after: S2
    - relation:
      type: ORDER
      before: S2
      after: S3
```

**Example #2**: S1  S2, S1  S3 that is S1 before S2, and S3. In this case S2 and S3 will be managed in parallel.

```yaml
service:
    alias: sample1
    decomposition:
    - service:
      alias: S1
    - service:
      alias: S2
    - service:
      alias: S3
    - relation:
      type: ORDER
      before: S1
      after: S2,S3
```

**Example #2**: S1  S3, S2  S3 that is S1 and S2 before S3. In this case S1 and S2 will be managed in parallel.

```yaml
service:
    alias: sample1
    decomposition:
    - service:
      alias: S1
    - service:
      alias: S2
    - service:
      alias: S3
    - relation:
      type: ORDER
      before: S1,S2
      after: S3
```

**Composite service with reference**

The following AppSpec shows a COMPOSITE service with two-services. The wordpress sub-service is setting environment variable as reference to db sub-service. This also suggest that wordpress sub-service is dependent on db and has to be resolved after it.

The COMPOSITE service performs the resolution of references mentioned in the sub-service. It replaces the reference with actual value when initiating the resolution of the sub-service. This reference is defined as {{reference}} where reference is structured as follow:

1.  Service-alias
    {{db}}, will be replace with the id of db service during the resolution.

2.  Service-alias.description.attribute-name
    {{db.description.attribute-name}} will be replaced with the actual value of the attribute-name from db service description.

3.  Service-alias.implementation.attribute-name
    {{db.implementation.attribute-name}} will be replaced with the actual value of the attribute-name from 'db' service implementation.

```yaml
service:
    type: COMPOSITE
    alias: MyWordpressAppHybrid2
    decomposition:

        - service:
            type: CONTAINER
            alias: db
            prefix: db
            cloud: devdocker1
            image: mysql:latest
            avoid_monitoring: true
            environment:
                - MYSQL_ROOT_PASSWORD=wordpress
                - MYSQL_DATABASE=wordpress
                - MYSQL_USER=wordpress
                - MYSQL_PASSWORD=wordpress
            ports:
            - 3306:3306

        - service:
            type: CONTAINER
            alias: wordpress
            prefix: wp
            cloud: devdocker2
            image: wordpress:latest
            avoid_monitoring: true
            environment:
                - WORDPRESS_DB_HOST={{db.implementation.host_ip_address}}:3306
                - WORDPRESS_DB_PASSWORD=wordpress
            ports:
            - :80
```

**Composite service with reference and relations**

```yaml
# -------------------------------------------------------------------------
#
# My App : Wordpress with MySQL database in a Single Host
#
# -------------------------------------------------------------------------
base: &config
cpu: 1
memory: 1024
disk: 0

cloud: &cc devdocker2

service:
    type: COMPOSITE
    alias: MyWordpressAppSingleHost
    decomposition:
        - service:
            type: WSCHED
            alias: wsched
            config:
                - : *config
            count: 2
            name: server
            policy: affinity
            avoid_monitoring: true
            clouds:
                - name: *cc

        - service:
            type: CONTAINER
            alias: db
            prefix: db
            >> : *config
            cloud: *cc
            image: mysql:latest
            serverid: {{wsched.implementation.server_1}}
            avoid_scheduler: true
            environment:
                - MYSQL_ROOT_PASSWORD=wordpress
                - MYSQL_DATABASE=wordpress
                - MYSQL_USER=wordpress
                - MYSQL_PASSWORD=wordpress

        - service:
            type: CONTAINER
            alias: wordpress
            prefix: wordpress
            cloud: *cc
            >> : *config
            image: wordpress:latest
            serverid: {{wsched.implementation.server_2}}
            avoid_scheduler: true
            environment:
                - WORDPRESS_DB_HOST={{db.implementation.private_ip}}:3306
                - WORDPRESS_DB_PASSWORD=wordpress
            ports:
                - :80

        - relation:
            type: order
            before: db
            after: wordpress
```


### AUTOSCALER

Auto-scaling as a Service provides a way to start a user application and automatically autoscale the resources needed by the application based on metrics collected by Composure via its monitoring service (see [Metric Firehose section](#metric-firehose)).

The Autoscaler service works within and across clouds. For example, in an autoscaling container setup with multiple Docker clouds, before instantiating containers in a docker-host, the autoscaling service consults the Composure resource allocator (Scheduler) to determine where the new containers should be located. You can decide the scheduling policy based on the kind of application you want to autoscale and then specify that policy in your AppSpec file.

#### Keys

List of supported keys:

| Key           | Type                               | Description                                                                            | Required                                                       |
|---------------|------------------------------------|----------------------------------------------------------------------------------------|----------------------------------------------------------------|
| type          | AUTOSCALER                         | The service type                                                                       | Yes                                                            |
| load_balance  | string                             | If true it sets up: **Docker**\: Nginx load balancer, **OpenStack**: Load balancer VM  | Allowed values\: true or false                                 |
| template      | List of container or VM properties | Define at least one service type configuration \(e.g. VM, CONTAINER, etc.\) See the specific [Service Types](#service-types) section for the key definitions. | Yes. The required keys for a template will be determined by the service type \(VM, CONTAINER, etc.\). |
| policy        | List of policies                   | One or more autoscaling policies can be defined in order of priority \(top \= first, bottom \= last\).   | Yes. At least one policy is required.        |



#### Policy keys

| Key               | Type             | Description                                                                                                                | Required                                                                     |
|-------------------|------------------|----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|
| name              | string           | The policy name.                                                                                                           | Yes                                                                          |
| min               | int              | The minimum number of instances.                                                                                           | Yes                                                                          |
| max               | int              | The maximum number of instances.                                                                                           | Yes                                                                          |
| scheduler\_policy | string           | Indicates the Scheduling policy to be used to place new instances when scaling out and removing instances when scaling in. | No. If not specified the **basic** policy is used by the autoscaling service |
| monitor           | List of Monitors | List of Monitors. Defines the metrics to use.                                                                              | Yes. At least one monitor must be specified.                                 |

#### Monitor keys

| Key                  | Type    | Description                                                                        | Required                          |
|----------------------|---------|------------------------------------------------------------------------------------|-----------------------------------|
| name                 | string  | The type of metric to use to determine when to scale (e.g. cpu_used, mem_util)     | Yes                               |
| scale_in_threshold   | int     | The percentage of average CPU utilization, memory utilization, etc. that can fall below across all instances in the autoscale group before instances are removed in order to scale in.                                                                                                                   | Yes                               |
| scale_out_threshold  | int     | The percentage of average CPU utilization, memory utilization, etc. that can be reached across all instances in the autoscale group before instances are added in order to scale out.                                                                                                                  | Yes                               |
| scale_out_unit       | int     | The number of instances to remove when scaling out.                                | Yes                               |
| scale_in_unit        | int     | The number of instances to add when scaling in.                                    | Yes                               |
| reaction_time        | int     | The number of seconds to wait before scaling in or out.                            | No (default is set to 30 sec).    |


#### Examples

**Example 1: Basic autoscale**

```yaml
service:
    type: AUTOSCALER
    alias: wordpress
    template:
    - cloud: joyent
      type: CONTAINER
      image: wp-autopilot-wordpress:2
      cpu: 1
      memory: 1024
      policy:
      - name: wordpress-policy
          min: 1
          max: 20
          monitor:
          - name: requests_per_second
          scale_in_threshold: 10
          scale_out_threshold: 30
          - name: cpu_util
          scale_in_threshold: 0
          scale_out_threshold: 60
```

**Example 2: Autoscale with default load-balancer attributes**

```yaml
service:
    type : AUTOSCALER
    load_balance : true
    alias : MyAutoScalerApp
    template:
    - cloud : My_OpenStack_Cloud
      type : VM
      image : web-server
      subnet : subnet1
      prefix : lb
      cpu : 1
      memory : 2048
      disk : 20
      policy:
        - name : policy1
          min : 2
          max : 10
          scheduler_policy : basic
          monitor:
          - name : cpu_util
            scale_in_threshold : 50
            scale_out_threshold : 90
            scale_in_unit : 1
            scale_out_unit : 2
            reaction_time : 60
          - name : mem_util
            scale_in_threshold : 50
            scale_out_threshold : 70
            scale_in_unit : 2
            scale_out_unit : 4
            reaction_time : 60
```

**Example 3: Autoscale with specific load-balancer attributes**

```yaml
service:
    alias: autoscaleLB
    type: AUTOSCALER
    load_balance : true
    template:
        - cloud: dockerCloud
    type : CONTAINER
    image : dzhangmsx/sort
    prefix : lb
    cpu : 1
    memory : 100
    disk : 0
    timeout: 1800
    load_balancer:
        type: LOADBALANCER
        cloud: dockerCloud
        subnet: subnet1
        front_protocol: HTTP
        back_protocol: HTTP
        front_port: 8080
    policy:
        - name : app-policy
          min : 2
          max : 6
          scheduler_policy : basic
          monitor:
              - name : cpu_util
                  scale_in_threshold : 2
                  scale_out_threshold : 5
                  scale_in_unit : 1
                  scale_out_unit : 2
                  reaction_time : 20
              - name : mem_util
                  scale_in_threshold : 50
                  scale_out_threshold : 70
                  scale_in_unit : 2
                  scale_out_unit : 4
                  reaction_time : 60
```

### LOAD_BALANCER

The Composure Load Balancing Service (MLBS) manages load-balancing across specific servers in the underlying clouds. It has three main components: discovery, provisioning, and policy run.

Load balancing as a services creates, updates, and deletes a load balancer in various clouds. It also dynamically updates the weight of load balancing backend servers, based on resource metrics.

Like many other microservices, this depends on the topology microservice to discover the cloud topology. From these it determines the load balancing components (if present). This is shown in the Cloud Graph representation.

#### Keys

| Key              | Type                      | Description                                  | Required                                           |
|------------------|---------------------------|----------------------------------------------|----------------------------------------------------|
| type             | LOADBALANCER              | Service type.                                | Yes                                                |
| prefix           | string                    | Generated lb name with given prefix.         | No                                                 |
| name             | string                    | Name assigned to lb.                         | No                                                 |
| cloud            | string                    | Cloud name.                                  | Yes                                                |
| subnet           | string                    | Name of subnet to connect to.                | Yes: Either define the subnet string or subnet ID. |
| *subnetid*       | string                    | Cloud ID of subnet in Composure.             |                                                    |
| front\_port      | int                       | lb tcp port.                                 | No. Default 80.                                    |
| front\_protocol  | string                    | The lb listener protocol (TCP, HTTP, HTTPs). | No. Default HTTP.                                  |
| back\_protocol   | string                    | Backend protocol (TCP, HTTP, HTTPs).         | No. Default HTTP.                                  |
| member\_services | List of member properties | The lb backend member services.              | No: List of load balancer service IDs (not names). |
| lb_policy_metrics| List of strings           | List of metrics affecting load balancing weights. | No.                                           |
| lb_method        | string                    | Load balancing method.                       | No. least_connection, source_ip or round_robin (default) |

#### Member\_services keys

| Key         | Type | Description                           | Required        |
|-------------|------|---------------------------------------|-----------------|
| service\_id | long | Member service service ID.            | Yes             |
| weight      | int  | The weight assigned to the lb member. | No. Default 1.  |
| port        | int  | tcp port                              | No. Default 80. |

#### Examples

Basic example:

```yaml
service:
    alias: mlb-sample-alias
    type: LOADBALANCER
    # which cloud to create load balancer
    cloud: test-cloud
    # at least one of subnet or subnetid needs to be provided
    # subnet : test-subnet
    subnetid: test-subnetid
```

Comprehensive example:

```yaml
service:
    alias: mlb-sample-alias
    type: LOADBALANCER
    # which cloud to create load balancer
    cloud: test-cloud
    # at least one of subnet or subnetid needs to be provided
    # subnet : test-subnet
    subnetid: test-subnetid
    # default HTTP if not provided
    front_protocol: HTTP
    # default HTTP if not provided
    back_protocol: HTTP
    # default 80 if not provided
    front_port: 8080
    member_services:
        - service_id : 17974800056175334
            # default 1 if not provided
            weight : 3
            # default 80 if not provided
            port : 8080
        - service_id : 17974800056175335
            weight : 5
            port : 8080
        - service_id : 17974800056175336
            weight : 5
            port : 8080
```

### NETWORK

A network in Openstack, AWS and Docker can be created, deleted, and managed as a service. Initially. we only support relatively simple use cases like a flat L2 network containing subnets which provide L3 services. Specifically, we do not support complex networks like L3 VPNs (IPSec or MPLS based) at this time.

#### Supported states

When the service is created, it will have REQUEST current and target state. The flowchart below shows how the service allows state transition. For example, we can ask the start to change to START or RUNNING. Later, we can ask the service to change to STOP.

| State       | Description                                                                                                                   |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| **REQUEST** | Network service has been requested but has not been picked up by the resolver yet for provisioning.                           |
| **START**   | Validated by the network service resolver and provisioning has started but is still in progress.                              |
| **RUNNING** | Successfully provisioned in the cloud. Compute and Storage instances can now be associated with the network service.          |
| **STOP**    | Has been deleted in the cloud.                                                                                                |
| **FAILED**  | Provisioning failed. Look at the Inspect pane for the log.                                                                    |

#### Network keys

| Key                    | Description                                                                                           | Required?                                                                                                          |
|------------------------|-------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| type                   | NETWORK                                                                                               | Yes                                                                                                                |
| name                   | Name of the network.                                                                                  | No                                                                                                                 |
| cloud                  | Cloud ID.                                                                                             | Yes                                                                                                                |
| network\_type          | Type of network (FLAT/VXLAN).                                                                         | Yes                                                                                                                |
| ip\_range              | The private IP range for the network. All subnets for this network will get a subset of this range.   | Yes                                                                                                                |
| subnets                | List of subnets that are part of this network (See subnets keys in table below.).                     | Yes                                                                                                                |
| external\_connectivity | Flag to indicate if the network is required to support external (internet) connectivity.              | Yes                                                                                                                |
| external\_ip           | IP address to be assigned to this network. This IP address will be allocated by the physical network. | Only required field if external-connectivity is set to true and we want to specify static IP for the external port |

#### Subnet keys

| Key         | Description                                                              | Required?                              |
|-------------|--------------------------------------------------------------------------|----------------------------------------|
| cloud       | Cloud name                                                               | Yes                                    |
| network     | The name of the network the subnet will be provisioned in.               | Yes. Either use network or network id  |
| networkid   | The Composure cloud id of the network the subnet will be provisioned in. |                                        |
| ip\_range   | IP address range in CIDR notation for the subnet.                        | Yes                                    |
| gateway\_ip | IP address of the subnet gateway.                                        | No                                     |
| name        | Name of the subnet.                                                      | No                                 |

#### Router keys

| Key     | Description                                                                                                                                                                                                                             | Required? |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------|
| name    | The name of the router. This will be referenced by the subnets that are attached to it.                                                                                                                                                 | Yes       |
| routes  | A list of arbitrary routes in the router’s routing table. By default all subnet routes will be configured in the router and the external gateway will be configured as the default gateway. This list is only to add non-subnet routes. | No        |
| subnets | The name of the subnets the router is associated with.                                                                                                                                                                                  | Yes       |

#### Example

```yaml
service:                        

 type: COMPOSITE                  
 alias: MyNetworkApp              
 decomposition:                   

     - service:                       
         type: NETWORK                    
         alias: my-network-1              
         name: my-network-1               
         cloud: cloud1                    
         ip_range: 192.167.0.0/16        
         external_connectivity: false  

     - service:                       
         type: SUBNET                     
         alias: my-subnet-1               
         name: my-subnet-1                
         cloud: cloud1                    
         network: my-network-1            
         ip_range: 192.167.1.0/24        
         gateway_ip: 192.167.1.1/32      

     - service:                       
         type: SUBNET                     
         alias: my-subnet-2               
         name: my-subnet-2                
         cloud: cloud1                    
         network: my-network-1            
         ip_range: 192.167.2.0/24        
         gateway_ip: 192.167.2.1/32      

     - service:                       
     type: ROUTER                     
     alias: my-router-1               
     cloud: cloud1                    
     name: my-router-1                
     routes:                          
         - destination: 0.0.0.0/0         
         nexthop: ""                      
         - destination: 192.168.0.0/16    
         nexthop: ""                      
         subnets:                         
             - my-subnet-1                    
             - my-subnet-2                    

     - relation:                      
         type: order                      
         before: my-network-1             
         after: my-subnet-1,my-subnet-2   

     - relation:                      
         type: order                      
         before: my-subnet-1,my-subnet-2  
         after: my-router-1
```

### CONNECTIVITY

Connectivity as a service provides a simple and effective way to manage the connectivity between cloud elements. With Connectivity as a Service, you can specify the services you want to connect or disconnect, for one or more specific protocols.

| Cloud type     | Source/Destination types           | Security configuration                                                                                                                          |
|----------------|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| Openstack, AWS | Any to any of the following types: VM, Group of VMs, Subnet, Group of subnets | Provision **security group and firewall rules** in such a way that the rules will **allow or deny traffic** from the source to the destination. |
| Azure          | Coming soon...                     |                                                                                                                                                 |

Connectivity as a service is available through the following 2 service types:

  - [PAIR_CONNECTIVITY](#pair_connectivity)

  - [GROUP_CONNECTIVY](#group_connectivity)

#### PAIR_CONNECTIVITY

##### Keys

| Key         | Description                                                             | Required?     |
|-------------|-------------------------------------------------------------------------|---------------|
| type        | PAIR\_CONNECTIVITY                                                      | Yes           |
| alias       | Service name                                                            | No            |
| source      | Composure Service IDs separated by comma or CIDR (e.g. 192.168.20./24). | Yes           |
| destination | Composure Service IDs separated by comma.                               | Yes           |
| protocol    | ANY, HTTP, HTTPS, SSH, TCP:3356, etc.                                   | Yes           |

##### Examples

**Example 1: Connecting two VMs identified by their service ID**

```yaml
service:
    type: PAIR_CONNECTIVITY
    source: "42422400025522584"           # must be a string
    destination: "52423490025522091"      # must be a string
    protocol: HTTP
```

**Example 2: Connecting a VM and allow SSH access from the internet**

```yaml
#---------------------------------------------
# VM in AWS Cloud with connectivity:
#---------------------------------------------

service:

    type: COMPOSITE
    alias: SSH_Gateway_OR
    decomposition:

        - service:                                       
            type: VM                                         
            alias: my-ssh-gateway                            
            prefix: my-ssh-gateway                           
            cloud: AWS_Cloud_Oregon                        
            subnet: the-subnet-1                             
            image: my-ssh-gateway-image                      
            flavor: t2.small                                 
        - service:                                       
            type: PAIR_CONNECTIVITY                         
            alias: outside-to-ssh-gateway-connectivity       
            source: "0.0.0.0/0"                              
            destination: "{{my-ssh-gateway}}"                
            protocol: "SSH"
```

#### GROUP_CONNECTIVITY

##### Keys

| Key       | Description                                           | Required?     |
|-----------|-------------------------------------------------------|---------------|
| type      | PAIR\_CONNECTIVITY                                    | Yes           |
| endpoints | Comma-delimited set of service IDs, e.g., “20,30,40”. | Yes           |
| protocol  | HTTP/HTTPS/SSH/TCP                                    | Yes           |

##### Example

```yaml
service:
    type: GROUP_CONNECTIVITY
    endpoints: "42422400025522584,52422400023522584,42422400025522586";
    connection: "HTTP,TCP:3573"
```


### CLONE

Used to clone containers across clouds.

#### Keys

| Key                       | Description                                                               | Required? |
|---------------------------|---------------------------------------------------------------------------|-----------|
| type                      | CLONE                                                                     | Yes       |
| name                      | Name for the clone service.                                               | Yes       |
| source: - cloud           | The name of the source cloud.                                             | Yes       |
| source: - containers      | The name of the containers in source cloud to be cloned.                  | Yes       |
| destination: - cloud      | The name of the destination cloud.                                        | Yes       |
| scheduling\_policy        | The scheduling policy for containers to be deployed in destination cloud. | No        |

#### Example

```yaml
service:
    type: CLONE
    name: cloning-wp-app
    source:
        - cloud: dockerCloud_c-src
    containers:
        - mosaix_wordpress_1
        - mosaix_mysqldb_1
        - nervous_kare
    destination:
        - cloud: dockerCloud_c-dst
    scheduling_policy: basic
```


### DOCKER_CLOUD

Composure users can deploy and mount one or more Docker Clouds as a service in a public or private cloud, each with multiple Docker hosts.

The Docker Cloud service type is operated like any other service supported by Composure (DOCKER\_CLOUD type), deploying standard Linux OS VMs (VM service type) with a standard docker container engine on it.

After the new Docker Cloud is deployed and mounted to Composure, it can immediately be used to deploy new containerized applications or any services types such as COMPOSITE, CONTAINER, NETWORK, SUBNET, CONNECTIVITY.

Similarly to other services, a Docker Cloud can be removed (or decommissioned) by terminating and deleting the service from the Cloud Composer tool.

#### Supported target clouds

| Cloud     | OS for Docker hosts                                                                                                                          |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------------|
| AWS       | Ubuntu 16.04 based AMI. Search for AMI id: [https://cloud-images.ubuntu.com/locator/ec2/](https://cloud-images.ubuntu.com/locator/ec2/)  |
| OpenStack | Standard Ubuntu 16.04 image                                                                                                                  |
| Azure     | Coming soon!...                                                                                                                              |

#### Automated deployment steps

1.  Creates a user-specified number of virtual machines (VM service type) to become docker hosts in the targeted cloud and attached to an existing subnet.

1.  Generates certificates and private keys for Composure to securely interface with the Docker Cloud.

1.  Uploads and installs the latest Docker engine version via “cloud-init”.

1.  Sets connectivity requirements ([CONNECTIVITY](/docs/guide/admin/admin-connectivity-optimizer) service type) also known as security groups/rules, allowing only the Composure server to reach these Docker hosts and allowing any Docker host to reach any other hosts within the Docker Cloud.

#### Pre-requisites

- At least one cloud mounted to Composure: see list of supported clouds.
- The image for the docker hosts must be standard Ubuntu 16.04 image
- The subnet specified in the AppSpec file for the DOCKER_CLOUD service needs to have the capability of auto-assigning public IPs to VM instances.

#### Keys

| Key                    | Type          | Description                                                           | Required |
|------------------------|---------------|-----------------------------------------------------------------------|----------|
| type                   | DOCKER_CLOUD  | Type of service.                                                      | Yes      |
| name                   | string        | Unique name to assign Docker Cloud.                                   | Yes      |
| count                  | string        | Number of Docker hosts.                                               | Yes      |
| cloud                  | string        | Name of an existing cloud to deploy the docker hosts on.              | Yes      |
| subnet                 | string        | Name of subnet to deploy the VM                                       | Yes      |
| imageid                | string        | Image by name to be run in VM. (e.g. AWS AMI, OpenStack flavor, etc.) | Yes      |
| flavor                 | string        | Instance type (AWS) or flavor name (OpenStack).                       | Yes      |
| keypair                | string        | Keypair name to be used (exclude file extension).                     | Yes      |
| msx_ip                 | string        | Public IP address of the Composure server.                            | Yes      |
| swarmPort              | string        | Must be 3376.                                                         | Yes      |
| msx_username           | string        | Username to login Composure.                                          | Yes      |
| msx_password           | string        | Password to login Composure.                                          | Yes      |
| msx_machine_password   | string        | Password of the machine where Composure is installed.                 | Yes      |


#### Example

```yaml
 #-----------------------------------------------------------------
 #                                                                   
 # DOCKER CLOUD as a Service                                         
 #                                                                   
 #-----------------------------------------------------------------  

 service:                                                             

   type: DOCKER_CLOUD

   alias: Docker_Cloud_Dev    #Name of your Docker Cloud and number of docker hosts:

   name: my-docker-cloud

   count: 5                     # Provide the Cloud where to deploy (e.g. AWS):               

   cloud: my-aws-cloud          # Must exist in Composure                  

   subnet: subnet-41c70028      # Must exist in the selected Cloud
                                # VM image for the Docker hosts (AWS AMI or OpenStack image):

   imageid: ami-969ab1f6

   flavor: c4.2xlarge

   keypair: my-aws-keypair      # Composure server public IP:                                     

   msx_ip: 54.214.82.157/32    # Port the Docker Cloud is listening to (must be 3376):             

   swarmPort: 3376              # Credentials to execute the cloud mount command*                  

   msx_username: admin
   msx_password: admin
   msx_machine_pwd: mosaix
```


{% endcapture %}


{% include templates/guide.md %}
