---
title: "Create an User in Kubernetes and Permit to use a Specific Namespace"
date: 2019-10-23T21:38:52+08:00
lastmod: 2017-08-28T21:41:52+08:00
#menu: "main"
weight: 50

# you can close something for this content if you open it in config.toml.
comment: false
mathjax: false
---
# Create an User in Kubernetes and Permit to use a Specific Namespace

Create an User in Kubernetes and Permit to use a Specific Namespace

In this post we try to learn how to configure an user for Kubernetes and allow only to use a certain name space. This is about to set RBAC policies for a user.

We are assuming that your Kubernetes uses SSL certificate based authentication. Make sure that RBAC is enabled. You can check that whether Kubernetes API

Please follow the steps below to create an user named user1 and assign developer role on namespace app1:

1. Create an SSL key for the user using the Openssl command:

```console
openssl genrsa -out user1.key 2048
```
2. Create a Certificate Signing Request using the key:

```console 
openssl req -new -key user1.key -out user1.csr -subj “/CN=user1/O=Test Organization”
```
Please remove the CN=user1 with user name that you need. O=Test Organization to your company name

3. Sign the CSR with the Kubernetes Cluster Certificate Authority and generate the Certificate for user1. For this you need ca.key and ca.crt from one of the cluster master. Usually the location is /etc/kubernetes/pki/ca.key and /etc/kubernetes/pki/ca.crt on Kind. Minikube it’s under ~/.minikube/certs/ca.key and ~/.minikube/certs/ca.crt. You can find this by looking at what keys API server service or Pod is using. Once you have the certs copied use:

```console 
openssl x509 -req -in user1.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out user1.crt -days 500
```
4. Create the Kubeconfig for the user1 with the key and cert:

```console
kubectl config set-credentials user1 — client-certificate=~/user1.crt — client-key=~/user1.key
```
Use the full path to key and cert

```console 
kubectl config set-context user1-context — cluster=kind — namespace=app1 — user=user1
```
5. Create the RBAC Role for the user with following manifest:
```console
kind: Role
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
 namespace: app1
 name: developer
rules:
- apiGroups: [“”, “extensions”, “apps”]
 resources: [“deployments”, “replicasets”, “pods”]
 verbs: [“get”, “list”, “watch”, “create”, “update”, “patch”, “delete”]*
```
Copy this to a yaml file and apply.

```console 
kubectl apply -f dev-role.yaml
```
6. Create a Rolebinding that to bind the role to user user1 to role developer:

```console 
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
 name: dev-binding
 namespace: app1
subjects:
- kind: User
 name: user1
 apiGroup: “”
roleRef:
 kind: Role
 name: user1
 apiGroup: “”*
```
copy this to a yaml file and apply:

```console
kubectl apply -f dev-role-binding.yaml
```
7. Test the RBAC rule:

```console
kubectl — context=employee-context get pods — namespace=defaultIt
```
Will not work and you would get the following error:

*Error from server (Forbidden): pods is forbidden: User “user1” cannot list resource “pods” in API group “” in the namespace “default”*

Now run pod on the app1 namespace:

```console 
kubectl — context=user1-context run — image nginx nginx
kubectl — context=user1-context get pods — namespace=app1
```
Now you have created user1 with only access to namespace app1
