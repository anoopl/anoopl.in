---
title: "AKS — Azure Kubernetes Service Overview"
date: 2017-08-20T21:38:52+08:00
lastmod: 2017-08-28T21:41:52+08:00
#menu: "main"
weight: 50

# you can close something for this content if you open it in config.toml.
comment: false
mathjax: false
---
# AKS — Azure Kubernetes Service Overview

AKS is a managed Kubernetes service by Azure.

Kubernetes master/control plane is maintained and managed by Azure. We can SSH in to Nodes and manage nodes to a certain extent.

We can access the cluster through Kuberenetes API. Azure provides monitoring integration with Azure monitor.

Docker UCP is the container runtime in Nodes. We do not have much options to customise API server configuration parameters or change Container Run time.

Different versions of Kubernetes are available in AKS. Latest version available as of today is 1.12.4

It’s easy to upgrade kubernetes versions on AKS, which is possible with a single command.

In an AKS cluster creating or managing other Azure resources like Load Balancer, Azure file, Azure disk are done through Service Principal

Architecture overview:

![](https://cdn-images-1.medium.com/max/2120/1*Prgpffw8skXPCCX3myIjhw.png)
