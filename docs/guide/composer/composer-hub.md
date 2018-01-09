---
title: Hub
contributors:
- nicolas
---

{% capture overview %}

The Hub is a list of published apps in the catalog. These include everything needed to deploy an application (e.g. pre-set target cloud, target subnet, etc). For example, for an ElasticSearch application the Hub configures subnets and pushes out Kibana and ElasticSearch as containers.

{% endcapture %}



{% capture steps %}

### The Hub

Composer comes preloaded with default apps similar to those shown below:

<img src="/media/image52.png">

### Publish an app

The user can publish a new app in the catalog from the **Composer** tab as shown below:

Go to **Tools  COMPOSER** and click on the “**Save Options**” button and select the “**Save in CATALOG**” option:

<img src="/media/image53.png">

To publish a new app in your hub, complete the dialog window, click **Save**.

<img src="/media/image54.png">


### Submit an app as a service

For example, you can deploy ElasticSearch and Kibana by pressing the submit button.

Then on the **Cloud Composer  SERVICES** tab you see the newly submitted service in the “Request” state and ready for you to run (**Actions  Running**):

<img src="/media/image55.png">

Click the arrow to the left of the service name to see what sub-services are included:

<img src="/media/image56.jpg">

{% endcapture %}


{% include templates/guide.md %}
