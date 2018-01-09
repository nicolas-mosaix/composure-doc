---
title: Cloud Graph
contributors:
- nicolas
---

{% capture overview %}

Cloud Graph allows you to visualize all the clouds you have set up in Composure. Open Cloud Graph by clicking **Tools  Graph**.

The cloud is shown as a graph, meaning with points and lines:

  <img src="/media/image82.png" width="624" height="541" />

Click on a node to see the details of a certain cloud component. The details appear in the attributes box on the right-hand side.

{% endcapture %}


{% capture steps %}

## Customizing a View

On the left-hand side are the graph view customization choices to filter the node type you want to display in the graph.

Click one node type at a time (Cloud, Tenants, Physical Servers, VMs/Containers, etc.) or click “**Select All**”:

  <img src="/media/image83.png" width="624" height="514" />

## Additional settings:

- **Freeze** or **Unfreeze:** (top left corner) freeze or unfreeze the gravitational movement of the nodes in the graph.

- **Layout options**: (bottom left), you can select different layouts for showing the graph. The hierarchical and radial layouts are experimental at this time.

- **Resync** the graph: The Resync icon (left bottom) will refresh all the service information for all the clouds you have mounted.

At the top of the page there is also a link called **Table** to the left of the search box. By clicking **Table** you can see the list of the nodes shown in a table format in Cloud Search.

{% endcapture %}


{% include templates/guide.md %}
