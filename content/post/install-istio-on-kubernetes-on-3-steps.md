---
title: "Install Istio on Kubernetes on 3 steps"
date: 2018-12-01T21:38:52+08:00
lastmod: 2017-08-28T21:41:52+08:00
#menu: "main"
weight: 50

# you can close something for this content if you open it in config.toml.
comment: false
mathjax: false
---
# Install Istio on Kubernetes on 3 steps

Install Istio on Kubernetes in 3 steps

This describes how to Install Istio on Kubernetes in 3 easy steps.

Same Steps can be used on any Kubernetes Cluster on AKS/EKS/Minikube.

For more detailed information please refer: [https://istio.io/docs/setup/kubernetes/](https://istio.io/docs/setup/kubernetes/)

Here we follow installing Kubernetes using yaml. There are other options available like installing with Helm etc..

1. Download Istio latest using:

```console
curl -L [https://git.io/getLatestIstio](https://git.io/getLatestIstio) | sh -**
```
### Current latest version 1.0.6 it seems that 1.1.0 will be released very soon

```console
cd istio-1.0.6**
export PATH=$<ISTIO_HOME>/bin:$PATH**
```
[https://istio.io/docs/concepts/security/#mutual-tls-authentication](https://istio.io/docs/concepts/security/#mutual-tls-authentication)Please replace <ISTIO_PATH> with where you have downloaded Istio

This export sets istioctl command available on your system

2. Install Istio’s Custom Resource Definitions using:

```console 
kubectl apply -f install/kubernetes/helm/istio/templates/crds.yaml
```
3. Install Istio with default Mutual TLS authentication:
```console
kubectl apply -f install/kubernetes/helm/istio/templates/crds.yaml
```
If you need with no mutual TLS use:
```console
kubectl apply -f install/kubernetes/istio-demo.yaml
```
To understand more about Mutual TLS on Istio, please refer: [https://istio.io/docs/concepts/security/#mutual-tls-authentication](https://istio.io/docs/concepts/security/#mutual-tls-authentication)

With this 3 steps Istio is installed. Make sure all services are fine and Pods are running by:

```console
kubectl -n istio-system get svc
kubectl -n istio-system get po
```
Output would be something like:
```console
NAME READY STATUS RESTARTS AGE
grafana-59b8896965-qbj97 1/1 Running 2 4d
istio-citadel-6f444d9999-xghlz 1/1 Running 0 3d7h
istio-egressgateway-68994d7c69-lv4v5 1/1 Running 2 3d9h
istio-egressgateway-68994d7c69-mzxzm 1/1 Running 1 4d
istio-egressgateway-68994d7c69-pbdnd 1/1 Running 0 3d4h
istio-egressgateway-68994d7c69-ph986 1/1 Running 0 3d4h
istio-egressgateway-68994d7c69-v2vj2 1/1 Running 0 3d4h
istio-galley-685bb48846-zmwzt 1/1 Running 2 4d
istio-ingressgateway-544c458fd6-hpsxb 1/1 Running 0 2d23h
istio-ingressgateway-544c458fd6-pqmgh 1/1 Running 0 2d16h
istio-ingressgateway-544c458fd6-qbxwf 1/1 Running 0 3d4h
istio-pilot-6cb5c55fb8-hpdvn 2/2 Running 4 4d
istio-policy-66ccc8df5f-7lwxv 2/2 Running 2 4d
istio-security-post-install-55rqd 0/1 Completed 0 4d
istio-sidecar-injector-5d8dd9448d-7dw5t 1/1 Running 0 3d7h
istio-telemetry-586ccd6d57-cjdtb 2/2 Running 5 4d
istio-telemetry-586ccd6d57-n9g62 2/2 Running 1 3d7h
istio-tracing-6b994895fd-wf2g4 1/1 Running 0 3d7h
prometheus-76b7745b64-pq576 1/1 Running 0 3d7h
servicegraph-cb9b94c-bg8xm 1/1 Running 0 2d23h
```
On another post I could explain how to install a test app/nginx test deployment using Istio
