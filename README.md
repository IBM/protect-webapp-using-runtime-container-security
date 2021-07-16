*** Work in-progress ***

# Protect your web application using advanced runtime container security

> Learn to implement Container firewall and Web-application firewall using NeuVector

## Introduction

Web application security is a mandatory requirement for any web application accessible on the internet. There are many types of attacks on web applications that can result in loss of sensitive and confidential information, frauds and denial of service. The Open Web Application Security Project® (OWASP) which is a nonprofit foundation have published the [top ten attacks](https://owasp.org/www-project-top-ten/) on web applications. The web application security must take care to prevent such attacks.

In the cloud native era, the applications are deployed in a container environments based on Kubernetes. The container runtime security can be implemented in addition to web application security. The container runtime security can monitor all activities within a container to detect threats.

In this code pattern, you will see how to use [NeuVector](https://neuvector.com/) to prevent web application and container runtime threats.
Once you complete the code pattern, you will learn how to:
- Deploy a vulnerable web application on IBM Kubernetes cluster
- Install NeuVector on IBM Kubernetes cluster
- Configure NeuVector to detect and prevent the following types of attack:
  - Cross Site Request Forgery (CSRF)
  - Malicious File Upload
  - Cross Site Scripting (XSS)
  - Sensitive Data Exposure
  - SQL Injection
  - API Service Protection
  - Container shell access
- Test the web application for the security threats

## Flow

## Pre-requisites

* IBM Cloud Account - If you are using NeuVector Service available on IBM Cloud
* IBM Kubernetes Cluster or OpenShift Cluster
* kubectl CLI
* oc CLI
* helm 3 CLI

## Steps

1. Deploy NeuVector on your Cluster
2. Deploy Sample Application
3. Configure NeuVector
4. Trigger Security Events

### 1. Deploy NeuVector on your Cluster

Create an instance of NeuVector Container Security Platform.
Create an instance of IKS
Gain the access of cluster
After creating the instance of the service, click on the instance on dashboard. It will take you the page which provides you a set of instructions to deploy NeuVector on your IKS cluster.
Perform those steps
After successful deployment, it will give you URL to access NeuVector WebUI.

### 2. Deploy Sample Application

For this code pattern, we have chosen the popular and open-sourced sample application `DVWA (Damn Vulnerable Web Application)` as the target for the attacks. The deploy configuration is provided in this repository to deploy the application into Kubernetes cluster. Run the below command:

```
kubectl apply -f deployment.yaml
```

Access the application at `http://<public-ip-of-cluster>:32425/`. It will show the following page when you login first time:

<landing-page>

 Click on `Create/Reset Database`. It will configure database with some tables for the application. On re-logging, you will get following screen:
 
 <landing page 2>
 

### 3. Configure NeuVector

Access NeuVector using its webui link. Use `admin/admin` for the first time login.

* Accept the End User license agreement. Click on `Accept`.
* You will see the following in bottom-right corner.

<snapshot>
  
* Click on it to change the password. It will take you to the Profile Settings. Click on `Edit Profile`. Provide the current password and new password then `Save`.
* Login again with new password.
* Add license Key.
  
    Copy the license key from IBM Cloud Dashboard page.
  
    Go to Settings > License
  
    Paste the license key in License Code box. Click Activate.

Now you are all set to use NeuVector with your IKS Cluster. You can start with setting your own security policies and test.

### 4. Set Policies

### 5. Trigger Security Events and Analyze the Alerts








## License

This code pattern is licensed under the Apache License, Version 2. Separate third-party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1](https://developercertificate.org/) and the [Apache License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache License FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)
