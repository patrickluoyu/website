---
title: "Deploy KubeSphere on DigitalOcean Kubernetes"
keywords: 'Kubernetes, KubeSphere, DigitalOcean, Installation'
description: 'Learn how to deploy KubeSphere on DigitalOcean.'

weight: 4230
---

![KubeSphere+DOKS](/images/docs/do/KubeSphere-DOKS.png)

This guide walks you through the steps of deploying KubeSphere on [DigitalOcean Kubernetes](https://www.digitalocean.com/products/kubernetes/).

## Prepare a DOKS Cluster

A Kubernetes cluster in DO is a prerequisite for installing KubeSphere. Go to your [DO account](https://cloud.digitalocean.com/) and refer to the image below to create a cluster from the navigation menu.

![create-cluster-do](/images/docs/do/create-cluster-do.png)

You need to select:

1. Kubernetes version (for example, *1.18.6-do.0*)
2. Datacenter region (for example, *Frankfurt*)
3. VPC network (for example, *default-fra1*)
4. Cluster capacity (for example, 2 standard nodes with 2 vCPUs and 4GB of RAM each)
5. A name for the cluster (for example, *kubesphere-3*)

![config-cluster-do](/images/docs/do/config-cluster-do.png)

{{< notice note >}}

- To install KubeSphere v3.1.1 on Kubernetes, your Kubernetes version must be v1.17.x, v1.18.x, v1.19.x or v1.20.x.
- 2 nodes are included in this example. You can add more nodes based on your own needs especially in a production environment.
- The machine type Standard / 4 GB / 2 vCPUs is for minimal installation. If you plan to enable several pluggable components or use the cluster for production, you can upgrade your nodes to a more powerfull type (such as CPU-Optimized / 8 GB / 4 vCPUs). It seems that DigitalOcean provisions the master nodes based on the type of the worker nodes, and for Standard ones the API server can become unresponsive quite soon.

{{</ notice >}}

When the cluster is ready, you can download the config file for kubectl.

![download-config-file](/images/docs/do/download-config-file.png)

## Install KubeSphere on DOKS

Now that the cluster is ready, you can install KubeSphere following the steps below:

- Install KubeSphere using kubectl. The following commands are only for the default minimal installation.

  ```bash
  kubectl apply -f https://github.com/kubesphere/ks-installer/releases/download/v3.1.1/kubesphere-installer.yaml
  
  kubectl apply -f https://github.com/kubesphere/ks-installer/releases/download/v3.1.1/cluster-configuration.yaml
  ```

- Inspect the logs of installation:

  ```bash
  kubectl logs -n kubesphere-system $(kubectl get pod -n kubesphere-system -l app=ks-install -o jsonpath='{.items[0].metadata.name}') -f
  ```

When the installation finishes, you can see the following message:

```bash
#####################################################
###              Welcome to KubeSphere!           ###
#####################################################
Console: http://10.XXX.XXX.XXX:30880
Account: admin
Password: P@88w0rd
NOTES：
  1. After logging into the console, please check the
     monitoring status of service components in
     the "Cluster Management". If any service is not
     ready, please wait patiently until all components
     are ready.
  2. Please modify the default password after login.
#####################################################
https://kubesphere.io             2020-xx-xx xx:xx:xx
```

## Access KubeSphere Console

Now that KubeSphere is installed, you can access the web console of KubeSphere by following the steps below.

- Go to the Kubernetes Dashboard provided by DigitalOcean.

  ![kubernetes-dashboard-access](/images/docs/do/kubernetes-dashboard-access.png)

- Select the **kubesphere-system** namespace.

  ![kubernetes-dashboard-namespace](/images/docs/do/kubernetes-dashboard-namespace.png)

- In **Services** under **Service**, edit the service **ks-console**.

  ![kubernetes-dashboard-edit](/images/docs/do/kubernetes-dashboard-edit.png)

- Change the type from `NodePort` to `LoadBalancer`. Save the file when you finish.

  ![lb-change](/images/docs/do/lb-change.png)

- Access the KubeSphere's web console using the endpoint generated by DO.

  ![access-console](/images/docs/do/access-console.png)

  {{< notice tip >}}

  Instead of changing the service type to `LoadBalancer`, you can also access KubeSphere console via `NodeIP:NodePort` (service type set to `NodePort`). You need to get the public IP of one of your nodes.

  {{</ notice >}}

- Log in to the console with the default account and password (`admin/P@88w0rd`). In the cluster overview page, you can see the dashboard as shown in the following image.

  ![doks-cluster](/images/docs/do/doks-cluster.png)

## Enable Pluggable Components (Optional)

The example above demonstrates the process of a default minimal installation. To enable other components in KubeSphere, see [Enable Pluggable Components](../../../pluggable-components/) for more details.
