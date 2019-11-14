---
title: "Create AKS Cluster using Azure CLI"
date: 2017-08-23T21:38:52+08:00
lastmod: 2017-08-28T21:41:52+08:00
#menu: "main"
weight: 50

# you can close something for this content if you open it in config.toml.
comment: false
mathjax: false
---
# Create AKS Cluster using Azure CLI

Deployment Steps:

![](https://cdn-images-1.medium.com/max/2000/1*ZdC0gVFzrcgQ2ymZJ8bhvg.png)

**prerequisites:**

1. Azure account with a subcription

1. Azure Cli installed and logged in. You can install Azure CLI by following [https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

1. Kubectl — Kuberntes Command Line Interface installed

1. Create a resource group

```console
az group create -n <resource-group-name> -l <location>
```
2. Create Virtualnet
```console
az network vnet create -g <resource-group-name> -n <vnet-name> --address-prefix <CIDR>
```
Example: 
```console 
az network vnet create -g xxxgggg -n k8s-vnet --address-prefix xx.xx.xx.xx/x'''
```
3. Create Subnet

This Subnet is used for the Pods network and Nodes

```console
az network vnet subnet create -n <subnet-name> -g <resource-group> --vnet-name <vnet-name> --address-prefix <CIDR-**for**-**this**-subnet>
```
Example: 
```console
az network vnet subnet create -n k8s-nodes-subnet -g pocCCPwu2rg --vnet-name k8s-vnet --address-prefix xx.xx.xx/x
```
4. Create Service principal

This Service Principal will be used for creating other resources like Load Balancer, Storage Disk etc
```console
az ad sp create-**for**-rbac -n <service-principal-name> --skip-assignment
```
Example: 
```console 
az ad sp create-**for**-rbac -n aks-poc-sp --skip-assignment
```
Output would be something like below. Use the AppID and Password in the command for creating cluster
```
{

"appId": "xxxxxxxx",

"displayName": "aks-poc-sp",

"name": "[http://aks-poc-sp"](http://aks-poc-sp%22/),

"password": "xxxxxxxx-xxxxxxx",

"tenant": "xxxxxxxxx"

}
```
4. Show Available Kubernetes versions:

5 . Create AKS cluster
```console
az aks create --resource-group xxxgggggg --name aks-poc --kubernetes-version 1.11.5 --node-count 1 --node-vm-size Standard_DS1_v2 --vnet-subnet-id "/subscriptions/xxx-xxx-xxxx-xxxx-xxxx/resourceGroups/xxx-rg/providers/Microsoft.Network/virtualNetworks/k8s-vnet/subnets/k8s-nodes-subnet" --network-plugin azure --service-principal <app_id_from_service_principal> --client-secret <service_principal_password> --service-cidr xx.xx.xx.xx/x--dns-service-ip xx.xx.xx.xx
```
Note:

Use — generate-ssh-keys to generate a new ssh key for SSH access to nodes. If this option is not used key ~/.ssh/id_rsa is used if it’s present

VM sizes list [https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes) — node

Even though documentation says Network Policy with Calico is supported, it’s not yet available.

Please read previous story for help on Network design & CIDR.
