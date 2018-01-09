---
title: AppSpec files
contributors:
- nicolas
---

{% capture overview %}

AppSpec files are [*YAML*](https://en.wikipedia.org/wiki/YAML) based configuration files that define services to create and run. AppSpec files can also use [*JSON*](https://en.wikipedia.org/wiki/JSON) format files, but the default format is YAML.

{% endcapture %}


{% capture steps %}

## Default templates

Composure comes with default sample templates, which can be used to help kick start creating new services. Here is a partial list.

| Template             | Description                                                                                    |
|----------------------|------------------------------------------------------------------------------------------------|
| all\_info.yaml       | Examples for setting up each type of cloud supported by Composure: AWS, Docker, and OpenStack. |
| AWS1.yaml            | VM service type running on AWS example.                                                        |
| aws\_info.yaml       | AWS cloud setup example.                                                                       |
| cn1.yaml             | CONNECTIVITY service type example to connect two services.                                     |
| docker1.yaml         | CONTAINER service type example for Docker based clouds.                                        |
| docker2.yaml         | CONTAINER\_GROUP service type example for Docker based cloud groups.                           |
| docker\_info.yaml    | Docker cloud setup example.                                                                    |
| openstack1.yaml      | VM service type running on OpenStack example.                                                  |
| openstack\_info.yaml | OpenStack cloud setup example.                                                                 |

These templates can be updated or deleted from Cloud Composer.


{% endcapture %}


{% include templates/guide.md %}
