---
title: User Management
contributors:
- nicolas
---

{% capture overview %}

User management includes login (authentication) and controlling what permissions user have (authorization). In the the following pages we show how you set those rights and create userids and passwords. But first here is an overview of the process.

{% endcapture %}

Each of the boxes below shows where in Composure you go to execute specific functions. There are four: **CLI** (command line interface), **CLI ACL** commands, the **user dialogue box** at the top of the Composure search screen, and the **WSO2 Identity Management System**. Each of these are explained in following sections.

<img src="/media/image96.jpg" width="624" height="350" />

Each function is handled by a different set of screens:

- **User password**—users can change their own password at the top of the main screen.

- **CLI: create users and groups**—administrators create users and groups here, assign users to groups, and change passwords. Non-admin Uusers can change their own password here.

- **Linux-Style ACLs**—access to services is controlled by Linux-Style ACLs and commands, like chmod.

- **WSO2: User Rights and Permissions**—here you add users and grant them permissions through policies. Those can be entered in a flexible manner such as by user or by group and using regular expressions. For companies that already have a user store, like Active Directory, Composure can use LDAP, OKTA, and other third-party systems. Or Composure can use its own internal user store. You administer all of this using the opensource WSO2 Identity Management system.

{% capture steps %}

### Users, Roles, and Permissions

When you first install Composure, it includes only the admin user with admin privileges and admin. After you login for the first time, change the admin password.

Services created by one user cannot be changed by another, unless the service owner grants those permissions explicitly. And the actions that users can take, like create a container server, are controlled by authorizations granted in WSO2.

### User Authentication

Authentication means the technique used to login. The supported authentication methods are:

| Method             | Description                                                        |
|--------------------|--------------------------------------------------------------------|
| local              | Users local user store, i.e., those added through the CLI.         |
| External OAuth 2.0 | Using social media accounts.                                       |
| SAML               | XML-based industry standard for connecting one service to another. |
| LDAP               | Uses LDAP, like Active Directory, OpenLDAP, etc.                   |

### Local user authentication

For local user authentication, users can be added manually via the CLI and their password changed through the GUI. To grant users permissions you have to add them to the WSO2 system as well (see the [User Authorization](#User-Authorization) section).

In the Composure CLI (**Tools \> CLI**):

          add-group: group\_name
          add-user: user\_name admin\_true\_or\_false

Example:

          add-user john false

Creates user John. False means he is not an admin. Create **finance** group and add him to that group.

          add-group finance
          grant-group-membership john finance

Now John is a member of the finance group. Now in WSO2 you can write a policy either for Walker or the finance group.

Get more information about a Composure command:
          add-user --help

### User Authorization

Authorization controls what users can do, like create a container or load balancer service or check network connectivity. You do this through the WSO2 IDM system.

Login to the WSO2 Management Console at https://(IP):9443/carbon/admin/login.jsp. The default userid and password is admin/admin.

Here is the main screen. Users are not added automatically from Composure to the WSO2 Identity Server. You have to add them here. Add them using the **add** function. Below we explain how to give them permissions by writing a policy.

  <img src="/media/image97.png" alt="WSO2 Management Console.png"/>

1.  Click **Policy Administration**. Then under Entitlement/PAP click **Add New Entitlement Policy**.

  <img src="/media/image98.png" alt="Add new entitlement policy" width="624" height="345" />

1.  Select **Basic Policy Editor**.

  <img src="/media/image99.png" alt="Basic policy editor"/>

1.  Select **Create XACML Policy**. The options are clearly explained on the screen. For example, this is a **Permit** rule and the user name is given. There are options for regular expression, equals, and not equals. The resource names are documented at the end of this section. Here we use **container**, meaning a user can created and start a container services. Click **finish** when done.

  <img src="/media/image100.png" alt="q.png" width="624" height="394" />

1.  Success message is shown.

  <img src="/media/image101.png" alt="Policy creation completed" width="278" height="107" />

1.  Now select policy and **Publish to PDP**.

  <img src="/media/image102.png" alt="Publish to PDP">

1.  Press **Publish**.

  <img src="/media/image103.png" alt="Publish policy">

  Now the user has container privileges.

**Note**: For more details on how to use WSO2 with OKTA, SAML, or LDAP, see the **Access Rights Management for Composure manual.**

**Resource Names**

| Attribute                   | Definition                                                                                                          |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------|
| COMPOSITE                   | Grouping of any service types listed in this table.                                                                 |
| VM                          | Virtual machine in private or public clouds                                                                         |
| VM\_GROUP                   | Group of the same virtual machine (e.g. count: 3)                                                                   |
| CONTAINER                   | Container in Docker Clouds                                                                                          |
| CONTAINER\_GROUP            | Group of the same container (e.g. count: 3)                                                                         |
| AUTOSCALER                  | Autoscaler service                                                                                                  |
| LOADBALANCER                | Load balancer service                                                                                               |
| WSCHED                      | Scheduler service                                                                                                   |
| NETWORK                     | Network such as a VPC in AWS.                                                                                       |
| SUBNET                      | subnet                                                                                                              |
| ROUTER                      | router                                                                                                              |
| PAIR\_CONNECTIVITY          | Connectivity between two VMs (security group configuration), two Containers (overlay network) or a VM and a subnet. |
| GROUP\_CONNECTIVITY         | Connecting two groups of VMs or containers (security group configuration for VM, overlay network for containers)    |
| RESET\_CONNECTIVITY         | Remove connectivity services previously created with either PAIR\_CONNECTIVITY or GROU\_CONNECTIVITY                |
| DOCKER\_CLOUD               | Docker Cloud as a service, aka 1-click Docker Cloud.                                                                |
| VM2C                        | Containerize a VM as a service                                                                                      |
| CASSANDRA\_CLUSTER\_MANAGER | Auto-scalable Cassandra Ring as a service                                                                           |

### Unix-style ACLs to Control Access Rights to Services

ACLS (access controls) controls rights and between a user and services. This mechanism is similar to the traditional UNIX file access control. Each user is assigned a user id. A user can be a member of one or more groups. In the example below, the logged in user is **walker.** He is the the group **finance**.

<img src="/media/image104.png" width="266" height="235" />

In Composure, when a service is submitted (created) by a user, it is designated as owned by a particular user. By default, when the service is submitted (i.e., created and started) the owner of the service gets read and write access. The id is the service id and not user id.

<img src="/media/image105.png" width="900" height="90" />

As shown above, user **walker** created a container service with id 57075104312130159 . The **list-services** command shows that the currently logged in user has both read and write access, shown as **rw**.

**Service Permissions** and **Service Modes** define who can access what service and what rights they have.

### Service Permissions

In Composure, there are three general classes of users:

- The user who owns the service.
- The group who owns the service.
- Anyone who is not the service owner or member of that group.

There are two types of service access:

- The ability to look at the contents of the service (read) such as to inspect a service and view the service description.
- The ability to change the state of the service (write) such as to running, stop, or terminate.

**Service Permission Representations:**

As shown in the previous graphic, the **list-services** shows access rights for a service in the **Rights** column. The table below shows possible values.

| Rights | Meaning                                  |
|--------|------------------------------------------|
| rw     | User has both read and write privileges. |
| r\_    | User has only read privileges.           |
| \_w    | User has only write privileges.          |
| \_\_   | User has no privileges.                  |

Now if a different user logs is he will not have access to the service created by Walker unless explicitly granted those rights using chmod:

<img src="/media/image106.png" width="900" height="65" />

The **chmod** command means **change mode**. It is used to define the way a service can be accessed. Just like UNIX, the owner of the service can use this command to set the service permission to take away or add permissions to the owner, group member, and people who are neither the owner or in the group.

chmod commands take the form:

          chmod permissions service-id

Here, permissions defines the permissions for the owner of the service, group member, and others. These permissions are specified by the digits 0 through 3, where:

- **3**—read and write.
- **2**—read.
- **1**— write.
- **0**—no permissions.

So, just like Linux, chmod 3000 would give read and write permissions to the service owner and no permissions to the group or people not in the group. Chmod 3320 would give the group and owner read and write permissions and people not in the group read-only permissions. The 4th digit is not used.

<img src="/media/image107.png" width="900" height="450" />


{% endcapture %}


{% include templates/guide.md %}
