---
title: Installation Requirements
author: nicolas
---

{% capture overview %}

This page shows how to connect to your first cloud and deploy your first service.

{% endcapture %}

{% capture steps %}

**Composure server**

- Dedicated physical or virtual machines
  - Ubuntu 14.04.4 LTS.
  - 8 CPU (16 CPU recommended).
  - 16 GB of RAM (32 GB recommended).
  - Network port
  - 80 GB of disk space minimum (SSD preferred, 512 GB recommended).

- Networking
  - Network port. At least one 1 GbE.
  - Access to the Internet (for pushing components to public clouds).
  - IP address that can be reached from your web browser.
  - A hostname resolvable to this IP address (internal or external).
  - SSH server running on the node.
  - Port security:
      - all ports open to itself
      - Ingress ports to open: SSH (22), HTTPS (443).

- Unix user account:
  - Mosaix user with superuser access for installation.

**Web browser to Composure web console**: Firefox, Chrome

**Supported Clouds**

| Cloud                   | Version/Description                                                           |
|-------------------------|-------------------------------------------------------------------------------|
| **Amazon Web Services** | AWS                                                                           |
| **Openstack**           | Releases: Juno, Kilo                                                          |
| **Microsoft Azure**     | Azure                                                                         |
| **Docker**              | Docker version 1.10 and higher

Optional requirements for Docker:

  - Authentication supported only if Docker hosts are using TLS verification.
  - Docker Swarm prior to 1.12
  - Networking:
    - Host, bridge, overlay networking (non-Swarm mode).
    - Overlay networking with Key/Value store (etcd, Zookeeper, etc)

{% endcapture %}

{% include templates/guide.md %}
