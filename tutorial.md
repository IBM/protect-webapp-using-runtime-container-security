## Add runtime container security to your production Kubernetes workloads

### Introduction -

The NeuVector solution is comprised of security containers, which can be deployed on each node similar to how you deploy your applications using Kubernetes. 
For evaluation purposes, NeuVector makes an Allinone container and an Enforcer container available. 

--> These can be pulled from Docker Hub, along with documentation, by requesting access from info@neuvector.com.

### Pre-reqs -


* IBM Cloud account. Note: To create a Kubernetes cluster to complete this tutorial, you must have a Pay-As-You-Go or Subscription account.
    IBM Cloud Command Line Interface (CLI) tool.
    IBM Cloud Kubernetes Service plug-in.
* Helm 3 CLI
* Kubernetes CLI (kubectl)
    
-->    
    Docker account.
    Free trial license for NeuVector full lifecycle container security console from the Red Hat Marketplace.

In addition, you must contact support@neuvector.com to request that your Docker Hub ID be added to the NeuVector private registry.

### Steps -

### Step 1.

1. Select Free from the list of pricing plans -> Select `Standard` from the list of pricing plans.


You can also create a free cluster from the command line by using the following IBM Cloud CLI command:

ibmcloud ks cluster create classic --name my_cluster

### Step 2. Access the Kubernetes Cluster

Now that the environment is provisioned, you can access it from the IBM Cloud CLI tool that you downloaded in the Prerequisites.

Go to IBM Cloud Dashboard, click on Clusters under Resource Summary section, then click the name of the cluster that you created in step 1.

< image 1>

It will lists the instructions to be performed as shown:

<image 2>

Follow the instructions on the terminal to:

* Log into your cluster.
* Set the Kubernetes context to your cluster.
* Verify that you can connect to your cluster.

### Step 3. Deploy NeuVector onto your Kubernetes cluster

#### 3.1 Create NeuVector Service Instance using IBM Cloud

Create an instance of NeuVector Container Security Platform using IBM Cloud Catalog. Provide the name of the service of your choice and click on create.

Once the service is created, go to IBM Cloud Dashboard > Services and Softwares (under Resource Summary) > click on the NeuVector service name. It will take you 
the page which provides a set of instructions to deploy NeuVector on your IKS cluster.

< image 3>

Perform the steps mentioned under `Deploying the NeuVector Platform on an IBM Cloud IKS cluster`. It asks you to download two configuration files inclusing secret 
manifest and helm values. You can download those and copy the below steps in one script and execute all the steps in one go using the script.

```
ibmcloud ks cluster ls |grep <cluster-name>

# Set IKS cluster id (e.g. c1cd1i4xxxj1v6g)
IC_IKS_CLUSTER_ID=c1cd1i4xxxj1v6g

ibmcloud ks cluster config --admin --cluster $IC_IKS_CLUSTER_ID

IC_IKS_INGRESS_DOMAIN=$(ibmcloud ks cluster get --cluster $IC_IKS_CLUSTER_ID --json | python -c "import json,sys;obj=json.load(sys.stdin);print((obj['ingress']['hostname'] if 'ingress' in obj and 'hostname' in obj['ingress'] else (obj['ingressHostname'] if 'ingressHostname' in obj else '')));")
echo $IC_IKS_INGRESS_DOMAIN

IC_IKS_INGRESS_SECRET_NAME=$(ibmcloud ks cluster get --cluster $IC_IKS_CLUSTER_ID --json | python -c "import json,sys;obj=json.load(sys.stdin);print((obj['ingress']['secretName'] if 'ingress' in obj and 'secretName' in obj['ingress'] else (obj['ingressSecretName'] if 'ingressSecretName' in obj else '')));")
echo $IC_IKS_INGRESS_SECRET_NAME

kubectl config current-context
kubectl get pod --all-namespaces

kubectl create namespace neuvector

kubectl apply -n neuvector -f ./neuvector-secret-registry.yaml

NV_VERSION=4.2.2

helm install \
    'neuvector-core' \
    'core' \
    --repo 'https://neuvector.github.io/neuvector-helm/' \
    --namespace neuvector \
    --values ./neuvector-helm.yaml \
    --set "manager.ingress.host=neuvector.${IC_IKS_INGRESS_DOMAIN}" \
    --set "manager.ingress.secretName=${IC_IKS_INGRESS_SECRET_NAME}" \
    --set "tag=${NV_VERSION}" \
    --atomic --wait
```

After successful deployment, it will give you URL to access NeuVector WebUI as https://neuvector.${IC_IKS_INGRESS_DOMAIN}.

**Apply NeuVector License**

Access the URL provided after successful deployment and login to NeuVector using default credentials `admin/admin`.

* Accept the End User license agreement. Click on `Accept`.
* You will see the following in bottom-right corner.

<snapshot>
  
* You can click on it to change the password.  It will take you to the Profile Settings. Click on `Edit Profile`. Provide the current password and new password then `Save`.
* Login again with new password.
* Add license Key.
  
    Copy the license key from IBM Cloud Dashboard page.
  
    Go to Settings > License
  
    Paste the license key in License Code box. Click Activate.

Now you are all set to use NeuVector with your IKS Cluster. Y

#### 3.2 Directly using Helm chart

