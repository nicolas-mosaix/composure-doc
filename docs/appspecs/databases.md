---
title: Databases
contributors:
- yourname
---

{% capture overview %}

This page shows how to compose different database types.

{% endcapture %}


{% capture steps %}

### MySQL

{% include code.html language="yaml" file="/yamls/mysql.yaml" ghlink="/docs/appspecs/yamls/mysql.yaml" %}

### MariaDB

{% include code.html language="yaml" file="/yamls/MariaDB.yaml" ghlink="/docs/appspecs/yamls/MariaDB.yaml" %}

### MongoDB

{% include code.html language="yaml" file="/yamls/mongo.yaml" ghlink="/docs/appspecs/yamls/MongoDB.yaml" %}

### CockroachDB

{% include code.html language="yaml" file="/yamls/cockroachdb.yaml" ghlink="/docs/appspecs/yamls/cockroachdb.yaml" %}

### Neo4j

{% include code.html language="yaml" file="/yamls/neo4j-single.yaml" ghlink="/docs/appspecs/yamls/neo4j-single.yaml" %}

{% endcapture %}


{% include templates/appspecs.md %}
