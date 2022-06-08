---
title : "Darcy AI Explorer App"
weight: 70
aliases:
  - /darcy/darcy-cloud/get-started-ec/darcy-ai-explorer-application
---

Apps are groups of microservices bundled to work together. They are defined using YAML files and can
be deployed and updated by uploading those YAML files through Darcy Cloud or through
[edgectl]({{<ref "/docs/cloud/edgectl/add-an-edge-node.md">}}). An app can consist of an arbitrary
number of interacting or standalone microservices that are deployed on one or may nodes.

## Prerequisites

To deploy the Darcy AI Explorer app, you will need:

* A Darcy Cloud Account
* Compatible ARM edge node with a Coral Accelerator
  * Raspberry Pi + Coral AI accelerator
  * Coral Dev Board
  * Coral Mini
  * Tinker T

{{<alert>}} If you have previously deployed an application to your node, you can either
start a new project and add a node there or delete the existing application from the node. We just
need to be sure that we have a free node inside our project to deploy to {{</alert>}}

## About the Darcy AI Explorer Application

The Darcy AI Explorer is a showcase app for the Darcy AI SDK. Bootstrap your AI project using
powerful face detection, pose detection, QR code scanning and more.