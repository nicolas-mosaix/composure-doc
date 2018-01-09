---
title: Web Console Overview
author: nicolas
---

{% capture overview %}

This page shows how to navigate through the various sections of the Composure web console.

{% endcapture %}

{% capture steps %}

The web console provides a graphical user interface to manage Composure. The console also includes a web command line interface. All the commands in the CLI are also available in the GUI.

### Menu bar

<img src="/media/image25.png" width="572" height="50" />

| Option      | Description                                                                                                           |
|-------------|-----------------------------------------------------------------------------------------------------------------------|
| Search      | [Cloud Search](/docs/guide/cloud/cloud-search)                                                                                         |
| About       | Information about the product version and link to Composure.ai EULA                                                   |
| Tools       | [Menu for all Composure tools](#tools-menu)                                                                           |
| Profile     | Displays current user, user’s token, users’ active group, users’ group membership, change their password, and logout. Go to the [User Management section](/docs/guide/admin/admin-user-management) for more information about those options.  |

When there is an action taking time (e.g., saving a composer file template) you will see the following animated icon to the left of the search box. It moves side-to-side when working:

<img src="/media/image26.png" width="44" height="41" />

### Tools menu

Click on the **Tools** pull-down menu to see a list of available tools:

<img src="/media/image27.png" width="267" height="368" />

#### Description of each tool

<table>
    <tbody>
      <tr class="odd">
      <td><strong>Item</strong></td>
      <td><strong>Description</strong></td>
      </tr>
      <tr class="even">
      <td><p><img src="/media/image28.png" width="60" height="44" /></p>
      <p><a href="/docs/guide/cloud/cloud-graph"><em>Cloud Graph</em></a></p></td>
      <td>Displays nodes in the cloud and shows how they are connected to one another as a graph.</td>
      </tr>
      <tr class="odd">
      <td><p><img src="/media/image29.png" width="45" height="41" /></p>
      <p><a href="/docs/guide/cloud/cloud-search"><em>Cloud Search</em></a></p></td>
      <td>Provides keyword and natural language search across all clouds mounted in Composure.</td>
      </tr>
      <tr class="even">
      <td><p><img src="/media/image30.png" width="44" height="62" /></p>
      <p><a href="/docs/guide/admin/admin-heatmap"><em>Heat Map</em></a></p></td>
      <td>Displays the capacity and load of the physical servers in your cloud.</td>
      </tr>
      <tr class="odd">
      <td colspan="2" align="center"><strong>CLOUD COMPOSER</strong></td>
      </tr>
      <tr class="even">
      <td><p><img src="/media/image31.png" alt="services.png" width="58" height="37" /></p>
      <p><a href="/docs/guide/composer/composer-services"><em>Services</em></a></p></td>
      <td>Services : list and manage (run, stop, terminate, delete)</td>
      </tr>
      <tr class="odd">
      <td><p><img src="/media/image32.png" alt="1.png" width="46" height="38" /></p>
      <p><a href="/docs/guide/composer/composer-hub"><em>HUB</em></a></p></td>
      <td>Save services AppSpec in a catalog for reuse.</td>
      </tr>
      <tr class="even">
      <td><p><img src="/media/image33.png" width="61" height="36" /></p>
      <p><a href="/docs/guide/composer/composer-composer"><em>Composer</em></a></p></td>
      <td>Create and manage services the cloud, like VM, CONTAINER, LOADBALANCER, etc.</td>
      </tr>
      <tr class="odd">
      <td colspan="2" align="center"><strong>CLOUD OPTIMIZER</strong></td>
      </tr>
      <tr class="even">
      <td><p><img src="/media/image34.png" width="55" height="46" /></p>
      <p><a href="/docs/guide/optimizer/optimizer-autoscaler"><em>Auto Scaler</em></a></p></td>
      <td>Automatically scale-out instances in the cloud.</td>
      </tr>
      <tr class="odd">
      <td><p><img src="/media/image35.png" width="49" height="49" /></p>
      <p><a href="/docs/guide/admin/admin-connectivity-optimizer"><em>Connectivity</em></a></p></td>
      <td>Visualize and provision connectivity.</td>
      </tr>
      <tr class="even">
      <td><img src="/media/image36.png" width="78" height="74" /></td>
      <td>Chargeback on a per service and per cloud basis</td>
      </tr>
      <tr class="odd">
      <td colspan="2" align="center"><strong>ADMINISTRATION</strong></td>
      </tr>
      <tr class="even">
      <td><img src="/media/image37.png" width="53" height="39" />>
      <a href="/docs/guide/admin/admin-web-cli"><em>CLI</em></a></td>
      <td>Composure command line interface via web browser.</td>
      </tr>
      <tr class="odd">
      <td><img src="/media/image38.png" width="51" height="69" /></td>
      <td>Browser and test Composure REST API via the Swagger tool.</td>
      </tr>
      <tr class="even">
      <td><img src="/media/image39.png" width="65" height="69" />
      <p><a href="/docs/guide/optimizer/optimizer-scheduler"><em>Scheduler</em></a></p></td>
      <td>Experimental - Deploy containers in any Docker cloud per scheduling policies (e.g.., resources, cost, performance).</td>
      </tr>
    </tbody>
</table>

{% endcapture %}

{% include templates/guide.md %}
