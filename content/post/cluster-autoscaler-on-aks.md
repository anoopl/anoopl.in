---
title: "Cluster Autoscaler on AKS"
date: 2017-08-23T21:38:52+08:00
lastmod: 2017-08-28T21:41:52+08:00
#menu: "main"
weight: 50

# you can close something for this content if you open it in config.toml.
comment: false
mathjax: false
---
# Cluster Autoscaler on AKS

Photo by Rebecca Georgia on Unsplash

Cluster Autoscaler is used to scale up or down cluster nodes according to load ( pending pods) or free nodes.

![](https://cdn-images-1.medium.com/max/2240/1*tR9qeCMDoTAMyUeNiaefTQ.png)

Earlier we had to manually configure Cluster Autoscaling on AKS cluster. Recently Azure AKS released this as a feature. So in this tutorial I would be explaining both the steps.

Manual:

1. Generate the secret with the cluster details by running the following script:

[https://gist.github.com/anoopl/d0bfb7c78a7a0027934689a7f675796d](https://gist.github.com/anoopl/d0bfb7c78a7a0027934689a7f675796d)

<iframe src="https://medium.com/media/d6785b5080592c749b09546714690c85" frameborder=0></iframe>

2. Run the script and copy the output to a yaml file cluster-autoscaler.yaml:

<iframe src="https://medium.com/media/708896d44e7cd1e80b504823e5f74ad1" frameborder=0></iframe>

3. Create the secret :
```console
kubectl apply -f cluster-autoscaler-secret.yaml
```
4. Use the following yaml to create other resources for the CA:

<iframe src="https://medium.com/media/8620ba61d038a81baeb4026c1a63f649" frameborder=0></iframe>
> You can set the minimum and maximum number of nodes in the yaml: — — nodes=1:10:nodepool1
> Create the resources with : 
```console
kubectl apply -f cluster-autoscaler-all.yaml
```
You can check the logs to see everything working as expected by checking CA Pod’s logs.
> 
```console
kubectl -n kube-system logs -f <cluster-autoscaler-pod>
```
Enable Cluster Autoscale feature on AKS

1. Update the Az Cli to the latest version:
```console
sudo apt-get update && sudo apt-get install --only-upgrade -y azure-cli
```
2. Enable aks preview mode
```console
az extension add --name aks-preview
```
3. Create new cluster with Autoscaling enabled:

<iframe src="https://medium.com/media/440ec6f542d87861eaa2a87ad4a9bda8" frameborder=0></iframe>

You can test the autoscaling by running a deployment of nginx with many replicaset:

Example:

[https://gist.github.com/anoopl/891965dd6c9110cdc7950f2d1529041a](https://gist.github.com/anoopl/891965dd6c9110cdc7950f2d1529041a)
