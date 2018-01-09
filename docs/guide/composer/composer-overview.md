---
title: Overview
contributors:
- nicolas
---

{% capture overview %}

The Cloud Composer enables easy user interaction with cloud services. It is used to define and execute all service requests. Cloud Composer consists of three main sections **Services, Hub, and Composer**.

<img src="/media/image40.png">

The general process of submitting a service is:

1.  An AppSpec file which specifies a service (or services) to deploy. This is done either by editing a pre-saved **Appspec file** or by creating a **new Appspec** or submitting a service from one of the apps published in the **Hub**.

1.  The service is submitted and is listed as an item in the **Services** tab.

1.  The service moves to a new state (e.g.: **Running, Terminate, Delete**) based on the desired state requested in the **Actions** button.

{% endcapture %}



{% capture steps %}


{% endcapture %}


{% include templates/guide.md %}
