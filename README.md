# DevOps for Cloud Native Reference Application

*This project is part of the 'IBM Cloud Native Reference Architecture' suite, available at
https://github.com/ibm-cloud-architecture/refarch-cloudnative*

## Introduction

DevOps, specifically automated Continuous Integration and Continuous Deployment (CI/CD), is important for Cloud Native Microservice style application. This project is developed to demonstrate how to use tools and services available on IBM Bluemix to implement the CI/CD for the BlueCompute reference application.

The project uses the [Jenkins Helm Chart](https://github.com/kubernetes/charts/tree/master/stable/jenkins) to install a Jenkins Master pod with the [Kubernetes Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Kubernetes+Plugin) in a Kubernetes Cluster. [**Helm**](https://github.com/kubernetes/helm) is Kubernetes's package manager, which facilitates deployment of prepackaged Kubernetes resources that are reusable. This setup allows Jenkins to spin up ephemeral pods to run Jenkins jobs and pipelines without the need of Always-On dedicated Jenkins slave/worker servers, which reduces Jenkins's infrastructural costs.

Here is the DevOps Architecture Diagram for the Jenkins setup on Kubernetes, along with a typical CI/CD workflow:

![DevOps Toolchain](static/imgs/architecture.png?raw=true)  

Let's get started.

## This guide will install the following resources:
* 1 x 20GB persistent volume claim to Store Jenkins data.
* 1 x Jenkins Master with port 8080 exposed on an external LoadBalancer.
* All using Kubernetes Deployments.

## Pre-Requisites
1. **CLIs for Bluemix, Kubernetes, Helm, JQ, and YAML:** Run the following script to install the CLIs:

    `$ ./install_cli.sh`

2. **Bluemix Account.**
    * Login to your Bluemix account or register for a new account [here](https://bluemix.net/registration).
    * Once you have logged in, create a new space for hosting the application in US-Southregions.
3. **Paid Kubernetes Cluster:** If you don't already have a paid Kubernetes Cluster in Bluemix, please go to the following links and follow the steps to create one.
    * [Log into the Bluemix Container Service](https://github.com/ibm-cloud-architecture/refarch-cloudnative-kubernetes#step-2-provision-a-kubernetes-cluster-on-ibm-bluemix-container-service).
    * [Create a paid Kubernetes Cluster](https://github.com/ibm-cloud-architecture/refarch-cloudnative-kubernetes#paid-cluster).

## Step 1: Install Jenkins on Kubernetes Cluster
As mentioned in the [**Introduction Section**](#introduction), we will be using a Jenkins Helm Chart to deploy Jenkins into a Bluemix Kubernetes Cluster. Before you do so, make sure that you installed all the required CLIs as indicated in the [**Pre-Requisites**](#pre-requisites).

Here is a script that installs the Jenkins Chart for you:

    ```
    $ cd jenkins
    $ ./install_jenkins.sh <cluster-name> <Optional:bluemix-space-name> <Optional:bluemix-api-key>
    ```

The output of the above script will provide instructions on how to access the newly installed Jenkins Pod.

**Note** that the Jenkins Master itself takes a few minutes initialize even after showing installation success

## Step 2: Disable Kubernetes HTTPS Certificate Check
This is a quick and easy way to get Jenkins to create slave pods using Kubernetes API and kill them when no longer needed.

With this option enabled, communication with kubernetes API master will rely on HTTPS but will fully ignore ssl certificate verification. This is useful for quick setup but does make your installation unsecured, so please consider twice before using this in a Production system.

![HTTPS Certificate Check](static/imgs/certificate.png?raw=true)  

## Step 3: Create and Run a Sample Pipeline
Run 
