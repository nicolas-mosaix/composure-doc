---
title: Cloud Search
contributors:
- nicolas
---

{% capture overview %}

The Composure presents a modern, search-based user interface that accepts free form text in order to find resources and services in your cloud.

Cloud Search can be accessed via the Search box on most pages or by clicking Home. In the search bar at the top of most pages, you can type keywords to search. Press Enter or click the <img src="/media/image76.png" width="28" height="22" />button to perform your search.

<img src="/media/image77.png" width="624" height="32" />

{% endcapture %}


{% capture steps %}

### Search Types

There are a variety of types of searches you can perform using Cloud Search.

#### Keyword

Type a single word in the search field and it will show you all the resources that related to the word.

<img src="/media/image78.png" width="494" height="316" />

#### Tag

Click on one of the auto-generated tags below the search field to search for the given element. Example: Clicking on container performs a search for type=container.

<img src="/media/image14.png" width="587" height="500" />

#### Attribute based

Searches can be narrowed down to specific attributes. For example, you can search for resources of type vm in order to view only VM resources. Any attribute and value pair can be used: name, type, status, etc.

<img src="/media/image79.png" width="305" height="240" />

#### Conditional

Combine attribute based searches with conditionals (AND, OR) to narrow or broaden searches.

Type several attribute based search terms in the search field separated by **AND** or OR to only find resources that have all those attributes. For example, searching for **type=vm AND status=up** will only find VMs that are currently running.

#### Natural language

Searches can also be complete sentences. For example: **What are the containers with maximum cpu?**

<img src="/media/image80.png" width="336" height="268" />

#### Details

After the list of cloud components is shown, clicking on an item under the **Name** column gives you details about the selected component.

<img src="/media/image81.png" width="624" height="558" />

{% endcapture %}


{% include templates/guide.md %}
