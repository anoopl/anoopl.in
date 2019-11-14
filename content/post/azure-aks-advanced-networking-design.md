---
title: "Azure AKS advanced Networking Design"
date: 2017-08-21T21:38:52+08:00
lastmod: 2017-08-28T21:41:52+08:00
#menu: "main"
weight: 50

# you can close something for this content if you open it in config.toml.
comment: false
mathjax: false
---
# Azure AKS advanced Networking Design

Kubernetes cluster is created under Virtual Network k8s-vnet 10.0.0.0/8 (but it is limited to 65,536 configured IP addresses)

Under this Vnet we need to create a new Subnet named k8s-nodes-subnet with CIDR 10.10.0.0/16. This subnet is used by K8s nodes and Pods, so plan the sizing accordingly

Service Subnet 10.20.0.0/16. This IP ranges are used by the services in Kubernetes. This subnet should not conflict with the Nodes Subnet or Docker Bridge CIDR (Default 172.17.0.0/16)

**Network Architecture**

![Azure AKS Network Design](https://cdn-images-1.medium.com/max/2880/1*3fenAEwsyhZUy150KfnbdQ.png)*Azure AKS Network Design*
