# Kubernetes Deployment Based on Manifests

## Prerequisites

* Local or cloud (EKS, GKE, AKE, etc) cluster running any version of Kubernetes
* Application defined in plain Kubernetes manifest files
* Pod exposed to the public Internet or other potential sources of malicious web and API attacks
* Kubernetes ingress controller or external load balancer (like AWS ELB or ALB) adds HTTP request header `X-Forwarded-For` which contains the real public IP address of connecting client
* Wallarm account in the [EU cloud](https://my.wallarm.com/) or [US cloud](https://us1.my.wallarm.com/)
* Username and password of the user with the **Deploy** role added to your Wallarm account. To add a new user, please follow the [instructions](../../../user-guides/cloud-ui/settings/users.md#create-a-user)

## Installation

1. [Create](#step-1-creating-wallarm-configmap) Wallarm ConfigMap.
3. [Update](#step-2-updating-the-deployment-object-in-kubernetes) the definition of the `Deployment` object in Kubernetes.
4. [Update](#step-3-updating-the-service-object-in-kubernetes) the definition of the `Service` object in Kubernetes.
5. [Deploy](#step-4-deploying-the-manifest-to-the-kubernetes-cluster) the manifest to the Kubernetes cluster.
6. [Test](#step-5-testing-wallarm-sidecar-container) Wallarm sidecar container.

### Step 1: Creating Wallarm ConfigMap

<ol start="1"><li>Create a new manifest file or add a new object to the existing manifest for a new Kubernetes ConfigMap object which will hold Nginx configuration file for Wallarm sidecar container:</li></ol>

--8<-- "en/include-en/kubernetes-sidecar-container/wallarm-sidecar-configmap-manifest.md"

<ol start="2"><li>Update parameter values following the code comments.</li></ol>

### Step 2: Updating the Deployment Object in Kubernetes

<ol start="1"><li>Go to Kubernetes manifests and open the template defining the <code>Deployment</code> object for the pod you want to add the WAF node to. For example:</li></ol>

--8<-- "en/include-en/kubernetes-sidecar-container/deployment-template.md"

<ol start="2"><li>Copy the following elements to the template:<ul><li>the <code>wallarm</code> sidecar container definition to the <code>spec.template.spec.containers</code> section,</li><li>the <code>wallarm-nginx-conf</code> volume definition to the <code>spec.template.spec.volumes</code> section.</li></ul>An example of the template with added elements is provided below. Elements for copying are indicated by the <code>Wallarm element</code> comment.</li></li></ol>

--8<-- "en/include-en/kubernetes-sidecar-container/deployment-with-wallarm-example-manifest.md"

### Step 3: Updating the Service Object in Kubernetes

<ol start="1"><li>Return to Kubernetes manifests and open the template defining the <code>Service</code> object which points to <code>Deployment</code> modified in the previous step. For example:</li></ol>

--8<-- "en/include-en/kubernetes-sidecar-container/service-template.md"

<ol start="2"><li>Make sure the <code>ports.targetPort</code> value is identical to <code>ports.containerPort</code> from the definition of Wallarm sidecar container.</li></ol>

### Step 4: Deploying the Manifest to the Kubernetes Cluster

Update or deploy the new application manifest in the Kubernetes cluster.

      #### Warning:: NetworkPolicy Object in Kubernetes
    
      If the application also uses the `NetworkPolicy` object it should be updated to reflect the Wallarm sidecar container port specified above.

### Step 5: Testing Wallarm Sidecar Container

--8<-- "en/./include-en/kubernetes-sidecar-container/test-sidecar-container-in-kubernetes.md"