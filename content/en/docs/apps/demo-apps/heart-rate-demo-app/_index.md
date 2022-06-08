---
title : "Heart Rate Demo App"
weight: 160
aliases:
  - /darcy/darcy-cloud/get-started-ec/heart-rate-application
---
Apps are groups of microservices bundled to work together. They are defined using YAML files and can
be deployed and updated by uploading those YAML files through Darcy Cloud or through
[edgectl]({{<ref "/docs/cloud/edgectl/add-an-edge-node.md">}}). An app can consist of an arbitrary
number of interacting or standalone microservices that are deployed on one or may nodes.

## Prerequisites

To deploy an app, you will need an Darcy Cloud account with at least one node accessible
and `ONLINE`. You can deploy the application with no experience in YAML or application building.

## About the Heart Rate Demo App

The Heart Rate Application provided on the Darcy Cloud platform simulates a wearable device
transmitting a person's heartbeat at the Edge. The Wearable sends heart rate data over bluetooth to
a data collector microservice located on the primary Node. The Data collector microservice then
communicates with another microservice running a web server on the secondary Node to display the
heart rate data on a graph.

![Microservice Interaction Diagram](/images/14565bf8-4100-48da-841a-6e3cf0dbd395.png)

{{<alert>}} Although this application works best with two nodes, you can deploy it with only one
node and run all microservices on the same device. The YAML will automatically detect if you have
one or two nodes. {{</alert>}}

![App YAML Definition](/images/3b76e231-64c8-4988-bbee-f9b2a447a2ec.png)

{{<alert>}} If you wish to know more about the application before deploying it, you can inspect
its [YAML definition]({{<ref "app-yaml.md">}}) by clicking on the curly brace in the modal on Darcy Cloud.
{{</alert>}}