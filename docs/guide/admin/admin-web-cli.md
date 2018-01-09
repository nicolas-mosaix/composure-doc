---
title: Web CLI
contributors:
- nicolas
---

{% capture overview %}

The Web command line interface (Web CLI) provides the ability to run the Composure CLI in your web browser. There is no need to SSH into the Composure host.

{% endcapture %}


{% capture steps %}

Start a Web CLI session by click **Tools  Web Command Line Interface**

You can see a list of all the available commands by typing:

          Composure> help

          Usage: command \[OPTIONS\] \[arg...\]

          Command line interface to the Mosaix cloud microkernel
          To get more information about each command use: command --help
          Commands:

          action-service: run a certain action on running service
          archive-service: archive any submitted service.
          cat: print the contents of a file
          check-service-state: check state of a previously submitted service
          chmod: change permission of a service
          chown: change service owner and group
          clear: clear the console
          delete-all-services: delete all previously submitted services
          delete-service: delete a previously submitted service
          delgroup: remove a group from the system
          delmembership: remove a user from a group
          deluser: remove a user from the system
          extract-service: extract a previously submitted service
          grant-group-membership: grant user a group membership
          groupadd: create a new group
          help: provides help on the mosaix command line commands
          history-service: get the history of a service
          install-remote-linux-monitor: install Linux monitoring agent on a...
          list-clouds: list all the cloud names
          list-services: list all the current services
          list-sub-services: list all the child services
          ls: list files in the user's upload directory
          mount: mount a new cloud service
          newgrp: log in to a new group
          passwd: change user password
          remove-rule: remove an access rule from a service
          resolve-service: start/stop a previously submitted service
          rm: remove a file from the user's upload directory
          show-rule: show permission rule of a service
          submit-service: submit a service to the mosaix cloud
          unmount: unmount a cloud service
          update-service: update a service
          useradd: Create a new user. Default password will be "mosaixsoft"
          
{% endcapture %}


{% include templates/guide.md %}
