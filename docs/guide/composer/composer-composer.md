---
title: Composer
contributors:
- nicolas
---

{% capture overview %}

The Composer tool is used to submit services from new or existing AppSpec files available in your AppSpec library hosted on your Composure server.

The editor pane (right) is used to view, create, update AppSpec files. deleted and organized in user-defined directories (e.g. projects, departments, etc.).

{% endcapture %}



{% capture steps %}

#### Editor pane

The editor pane is where you submit services from AppSpec files you created, updated or just selected from the file browser on the left.

To quickly submit multiple services, you can also **drag and drop** appspec files from the AppSpec file browser on the left into the editor pane.

Click the **New** button to create a new AppSpec file.

<img src="/media/image57.png" width="641" height="362" />

You can change the background color of the editor clicking the gear icon (top right):

<img src="/media/image58.png" width="250" height="240" />

The Editor panel has a text editor for creating and editing service template files.

<table>
  <thead>
    <tr>
      <th>Button</th>
      <th>Description</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td><img markdown="1" src="/media/image59.png" width="81" height="46"></td>
      <td>Undo changes.</td>
    </tr>
    <tr>
      <td><img src="/media/image60.png" width="88" height="26"></td>
      <td>Click to expand and see save options: Save as new, Publish in hub, Create a composite.</td>
    </tr>
    <tr>
      <td><img src="/media/image61.png" width="66" height="32"></td>
      <td>Validate the content of the current AppSpec file.</td>
    </tr>
    <tr>
      <td><img markdown="1" src="/media/image62.png" width="64" height="32"></td>
      <td>Submit the service to create.</td>
    </tr>
  </tbody>

</table>

As shown below, click on an Appspec file on the left file browser (or file library), to display and start editing its content in the editor pane (“YAML” tab):
<img src="/media/image63.png" width="644" height="566" />

#### Auto-completion

To assist the user, the editor includes several types of automated text completion:

<table>
  <tr>
    <td><img src="/media/image64.png" width="273" height="186" /></td>
    <td>Type <strong>the first characters of a supported key</strong> to automatically start showing a narrowing list of keys to pick from. Move the up or down arrow keys (or scroll your mouse) to select one.
    </td>
  </tr>

  <tr>
    <td><img src="/media/image65.png" width="273" height="132" /></td>
    <td>Type <strong>CTRL + spacebar</strong> : Automatic listing of resource names which type depends on the key at the beginning of the line. (e.g. type “cloud:” then a space, then CTRL+space to see a list of all cloud names mounted and discovered by Composure).
    </td>
  </tr>

  <tr>
    <td>
      <img src="/media/image66.png" width="298" height="130" /><br>
      <em>then select CONTAINER to auto-expand:</em><br>
      <img src="/media/image67.png" width="226" height="214" /><br>
    </td>
    <td>Type the keyword “<strong>snippet: ” and type CTRL+spacebar</strong> to show a list of service types and select one of the service types (e.g. VM, CONTAINER) to automatically expand a list of both required and optional keys for the selected service type. The word “snippet” is automatically removed.
    </td>
  </tr>
</table>


{% endcapture %}


{% include templates/guide.md %}
