---
title: "Install Istio using Helm Template Generation"
date: 2018-12-02T21:38:52+08:00
lastmod: 2017-08-28T21:41:52+08:00
#menu: "main"
weight: 50

# you can close something for this content if you open it in config.toml.
comment: false
mathjax: false
---
# Install Istio using Helm Template Generation

Install Istio using Helm Template Generation

We can install Istio with 3 methods. We have already discussed the first method on previous post.

Complete installation steps for Istio on Kubernetes can be found a
[**Kubernetes**
*Instructions for installing the Istio control plane on Kubernetes and adding virtual machines into the mesh.*istio.io](https://istio.io/docs/setup/kubernetes/)

[https://istio.io/docs/setup/kubernetes/quick-start/](https://istio.io/docs/setup/kubernetes/quick-start/)

Now lets discuss the second method using Helm template generate. This method makes it easier to customise the installation ( even post install).

1. Download Istio latest using:
```console
curl -L [https://git.io/getLatestIstio](https://git.io/getLatestIstio) | sh -
cd istio-1.0.6
```

Current latest is 1.0.6

2. Generate the manifest file:
```console
helm template install/kubernetes/helm/istio --name istio --namespace istio-system --set global.proxy.includeIPRanges="10.0.0/16"> ~/workspace/istio/istio.yaml
Eg:

helm template install/kubernetes/helm/istio --name istio --namespace istio-system --set global.proxy.includeIPRanges="10.0.0.0/16",global.mtls.enabled=false,grafana.enabled=true,kiali.enabled=true,tracing.enabled=true > ~/workspace/istio/istio.yaml

```

10.0.0.0/16 is Network CIDR for your cluster. We had to use this to make outbound/internet traffic work on the app pods.

3. Create a Istio Namespace and apply the manifest:
```console
kubectl create ns istio-system
kubectl apply -f istio.yaml
```
You can keep versioning on the yaml file name for easy recognition like istio-v1.yaml so on.

If you want to make changes after applying the manifest you can generate the new yaml with different — set options then apply it using kubectl.
