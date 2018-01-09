---
title: Clusters
contributors:
- nicolas
---

{% capture overview %}

This page shows how to compose different clustered applications.

{% endcapture %}


{% capture steps %}

### Elastic Search with Kibana and Logstash (ELK)

{% include code.html language="yaml" file="/yamls/ELK-single.yaml" ghlink="/docs/appspecs/yamls/ELK-single.yaml" %}

### Neo4j Causal Clusters

{% include code.html language="yaml" file="/yamls/neo4j-enterprise.yaml" ghlink="/docs/appspecs/yamls/neo4j-enterprise.yaml" %}

### SumoLogic

{% include code.html language="yaml" file="/yamls/sumologic-cluster.yaml" ghlink="/docs/appspecs/yamls/sumologic-cluster.yaml" %}

### Consul

{% include code.html language="yaml" file="/yamls/consul-cluster.yaml" ghlink="/docs/appspecs/yamls/consul-cluster.yaml" %}


{% endcapture %}


{% include templates/appspecs.md %}
