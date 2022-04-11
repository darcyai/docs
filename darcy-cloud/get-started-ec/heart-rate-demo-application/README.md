# Heart Rate Demo Application

Apps are groups of microservices bundled to work together. They are defined using YAML files and can be deployed and updated by uploading those YAML files through Darcy Cloud or through EdgeCTL. An app can consist of an arbitrary number of interacting or standalone microservices that are deployed on a single or an arbitrary number of nodes

## Prerequisites

To deploy an app, you will need an Darcy Cloud account with at least one node accessible and `ONLINE`. You can deploy the demo app with no experience in YAML or application building

### About the "Heart Rate Demo App"

The Heart Rate Demo App provided on the Darcy Cloud platform simulates a wearable device transmitting a person's heartbeat at the Edge. The Wearable sends heart rate data over bluetooth to a data collector microservice located on the primary Node. The Data collector microservice then communicates with another microservice running a web server on the secondary Node to display the heart rate data on a graph

![Microservice Interaction Diagram](../../../.gitbook/assets/14565bf8-4100-48da-841a-6e3cf0dbd395.png)

{% hint style="info" %}
Although this demo app works best with two nodes, you can deploy the app with only one node and run all microservices on the same device. The demo app YAML will automatically detect if you have one or two nodes.
{% endhint %}

{% hint style="info" %}
If you wish to know more about the demo app before deploying it, you can inspect its [YAML definition](../../glossary/yaml-heart-rate-app.md) by clicking on the curly brace in the modal on Darcy Cloud
{% endhint %}

![Demo App YAML Definition](../../../.gitbook/assets/3b76e231-64c8-4988-bbee-f9b2a447a2ec.png)