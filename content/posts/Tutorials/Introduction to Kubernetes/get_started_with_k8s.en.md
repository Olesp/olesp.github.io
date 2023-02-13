---
title: "Introduction to Kubernetes"
date: 2023-02-13T08:06:25+06:00
description: Introduction to Kubernetes
hero: images/vault.png
menu:
  sidebar:
    name: Intro to K8S
    identifier: k8s
    parent: tuto
    weight: 10
tags: ["Réalisations techniques", "Multi-langues"]
categories: ["Réalisations techniques"]

---

Kubernetes is an open-source container management system. It was designed to automate the deployment, scaling, and management of container-based applications.

## Main concepts of Kubernetes
Kubernetes is based on different concepts that together form a coherent system to carry any application.

- Node
- Cluster
- Container
- Pod
- Deployment
- Service

### Node

A node is a physical or virtual machine that is part of a Kubernetes cluster and performs cluster tasks. Each node has an agent software called kubelet, which monitors the state of containers on the node and keeps them running.

### Cluster

A Kubernetes cluster is a group of coordinated nodes that run containerized applications. The nodes can be deployed on virtual machines in the cloud or on dedicated servers. A cluster can be managed using the kubectl command, which communicates with the cluster controller to perform tasks such as deploying applications and managing resources.

### Container

A container is an instance running an application in an isolated environment. Containers are created from images that describe the software configuration of the application. Containers can be deployed on a single node or on multiple nodes to manage high availability and fault tolerance. It is important to keep in mind that Kubernetes did not invent the concept of containers. It simply relies on them to make deploying and scaling applications easier.
For more information on containers, see here

### Pod

A pod is the smallest deployable unit on a Kubernetes cluster. It can contain one or more containers that share the same resources, such as storage space and memory. Pods are deployed on a single node and can be managed as a single unit for tasks such as deployment, scaling, and updating.

### Deployment

A deployment is a configuration object that describes an application and its associated resources. It defines the number of replicas of an application that should be running at any given time, as well as the update and scaling strategies. Deployments are managed using the kubectl command to perform tasks such as deploying, scaling, and updating applications.

### Service

A service is an abstraction that exposes the running applications on a cluster using a stable URL or IP address. Services manage communication between the different parts of the application, such as frontends and backends.

## Steps to deploy an application on Kubernetes
Create a deployment file describing your application and its associated resources.
Use the `kubectl apply` command to deploy your application on the cluster.
Check the deployment using the `kubectl get pods` command.
Create a service to expose your application using the `kubectl expose command`.
Check the service using the `kubectl get services` command.
These are the basics to deploy an application on Kubernetes. There are many other concepts and features to manage more complex applications on Kubernetes that I won't cover in this introduction. You can still find everything on the official [documentation](https://kubernetes.io/fr/) of the project, which is very well written.

## Helm

Helm is a package manager for Kubernetes that makes it easier to deploy and manage applications on a Kubernetes cluster. With Helm, you can define an application using a collection of templates called charts that define how the application should be deployed. The charts encapsulate the complex details of a deployment into a simple and reusable package.

By using Helm, you can easily install, update, and manage the lifecycle of an application. You can also revert to a previous version of an application if necessary. Additionally, Helm charts can be shared and reused, allowing you to leverage the collective knowledge and expertise of the Kubernetes community to streamline the deployment of your own applications.

To use Helm, you just need to install the Helm client on your local computer and have access to a running Kubernetes cluster. From there, you can use the Helm client to interact with the cluster and deploy your applications.

Helm is a valuable tool for anyone who wants to deploy and manage applications on a Kubernetes cluster.
