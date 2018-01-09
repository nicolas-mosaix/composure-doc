---
approvers:
- nicolas
title: Configuring Docker Hosts
---

## Installation instructions on AWS

### Prerequisites:
 * You own an AWS account

### Creating and configuring a Docker host in AWS:
  1. Create an Ubuntu 14.04 Amazon instance (any flavor)
  1. The associated AWS Security group must allow TCP connections to port 4243 from Optima's IP address
  1. From your favorite terminal, login to this instance:
  1. 'scp' the attached bash script to the instance

     ```
     $ scp docker-host-install.sh ubuntu@<docker-host-ip>
     ```

  1. run the docker-host-install script:

     ```
     $ ./docker-host-install.sh
     ```

     This bash script basically does the following:
        * Installs docker (version 17.03.1-ce)
        * Activates Docker Remote API
        * Generates a unique ID for the Docker engine
