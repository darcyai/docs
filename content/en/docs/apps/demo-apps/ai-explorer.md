---
title: "AI Explorer demo app"
weight: 70
aliases:
  - /darcy/darcy-cloud/get-started-ec/darcy-ai-explorer-application
---

![Heart Rate Demo App](/images/ai-exp-hero.jpg)

Darcy AI Explorer is a showcase app for Darcy AI. Bootstrap your AI project using
powerful face detection, pose detection, QR code scanning and more.

#### Requirements

* A Darcy Cloud account
* Compatible edge node with 1GB or more of disk space available for application download
  * ARM-based edge device or virtual node
  * x86-based edge device or virtual node

{{<alert>}} If you have previously deployed an application to your node, you can either
start a new project and add a node there or delete the existing application from the node. We just
need to be sure that we have a free node inside our project to deploy to {{</alert>}}

---

## Deploy the app using Darcy Cloud Portal

Deploying the AI Explorer Demo app from the Darcy Cloud platform is simple. Make sure
you have [added at least 1 node]({{<ref "/docs/cloud/adding-nodes/add-node.md">}}) to your Darcy Cloud project, then
follow the steps below:

1. Go to your Darcy Cloud project page
2. Select `Apps`
3. Click on `+ DEPLOY APP`
4. Click on `DARCY AI EXPLORER APP`
5. Select the node(s) where you want it to run
6. Click `DEPLOY`

## See App Results

When your app is deployed successfully you will see an `Application Deployed` message in the top
right of your screen.

If you are not already seeing the app listed, click the `APPS` tab to see all of the apps in your
current project. Click the app to access the app detail view.

Once your Darcy AI Explorer app is running, you can view the UI and use the app by visiting
the following URL in any browser. Replace `YOUR.DEVICE.IP.ADDRESS` with the actual IP address of your device.

```bash
http://YOUR.DEVICE.IP.ADDRESS:5555/
```

## App YAML

```yaml
kind: Application
apiVersion: iofog.org/v3
metadata:
  name: darcy-ai-explorer
spec:
  microservices:
    - name: darcy-ai-explorer
      agent:
        name: ''
      images:
        arm: 'darcyai/darcy-ai-explorer:1.0.2'
        x86: 'darcyai/darcy-ai-explorer:1.0.2'
      container:
        rootHostAccess: true
        env:
          - key: PORT
            value: '5555'
        ports:
          - external: '{{self.microservices[0].env[0].value | toNumber}}'
            internal: '{{self.microservices[0].env[0].value | toNumber}}'
        volumes:
          - containerDestination: /dev
            hostDestination: /dev
            type: bind
            accessMode: rw

```

{{<alert style="info">}}
Note that AI Explorer collects [anonymous usage statistics](/docs/ai/anonymous-statistics.md).
{{</alert>}}
