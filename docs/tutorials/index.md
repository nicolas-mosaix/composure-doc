---
title: Tutorials
---

Composure is a software product from Composure.ai and is available for evaluation today. Please send an email to [eval@composure.ai](mailto:eval@composure.ai) if you are interested to evaluate Composure today.

As a fun way to quickly discover Composure, you are invited to go through the following simple tutorials.  Each tutorial should not take more than 5 minutes:

### List of tutorials
1. [Load & deploy your cloud app using the Cloud Composer]()
1. [Deploy a cloud app from the Service Hub]()
1. [Build your own cloud app for Docker]()
1. [Deploy & autoscale Cassandra Rings as a service]()
1. [Connect to an AWS region]()
1. [Connect to an OpenStack cluster]()
1. [Deploy a Docker Cloud as a service in your AWS cloud]()
1. [Deploy a VM in AWS or OpenStack]()


Before starting any of those tutorials, login to your Composure server using your credentials (login/password) via a your browser (Firefox or Chrome):

<div style="text-align:center">
<img src="./screenshots/screenshot-01.png" alt="Screenshot" width="250">
</div>

## 1. Load & deploy your cloud app to the Cloud Composer

If you have received an Appspec file (\*.yaml) by email from Composure.ai, you can deploy your composite service or cloud application immediately on your Composure server.

  1. Go to **Tools > Composer**

  1. Open your **local file browser** (e.g. Finder on macos)

  1. **Drag & drop** your appspec file (\*.yaml) from your file browser into the Appspec browser in your web browser.

  1. Select the Appspec you just added: it's content will be revealed in the YAML editor.

  1. Click the **Submit** button

  1. Select the **Service** tab

  1. Select the service you just added to the list

  1. Click **Actions** button and select "Running" to deploy your service in your Docker Cloud.


## 2. Deploy a cloud app from the Service Hub

  1. Go to **Tools > Hub**

  <div style="text-align:center">
  <img src="./screenshots/screenshot-02.png" alt="Screenshot" width="800">
  </div>

  1. Browse through the list of validated cloud applications:

  <div style="text-align:center">
  <img src="./screenshots/screenshot-03.png" alt="Screenshot" width="800">
  </div>

  1. Click submit on the **Apache** app

  Note the "Info" message on top of the page confirming the service has been submitted:

  <div style="text-align:center">
  <img src="./screenshots/screenshot-04.png" alt="Screenshot" width="800">
  </div>

  1. Go to the Service tab and select the service called "Apache_service"

  When selecting a service, you can see more details about it at the bottom of the page:

  <div style="text-align:center">
  <img src="./screenshots/screenshot-05.png" alt="Screenshot" width="800">
  </div>

  **Role of each sub-tab:**

| Tab     | Description |
| ------- | ----------- |
| Details | Show list of containers and/or VMs created by that service    |
| Compose | Shows the original Appspecs (YAML file) |
| Inspect | Shows detailed information on a service post-deployment|



## 2. Build your own cloud app for Docker
Let's compose a 2-tier application with a web server and a database:

## 3. Deploy & autoscale Cassandra Rings as a service

## 4. Connect to an AWS region

## 5. Connect to an OpenStack cluster

## 6. Deploy a Docker Cloud as a service in your AWS cloud

## 7. Deploy a VM in AWS or OpenStack
