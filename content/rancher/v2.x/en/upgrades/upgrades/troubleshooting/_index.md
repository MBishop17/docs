---
title: Troubleshooting Upgrades
weight:
aliases:
---

## Post Upgrade: Cluster Networking Broken

### Symptoms

If you're experiencing _both_ of the symptoms below, follow the [solution](#solution) to restore networking.

- Following upgrade from Rancher v2.0.7+ to a newer version of Rancher, networking no longer works for one or more clusters.
- In the clusters affected, one or more of the following namespaces are moved _out_ of the **System** project and into another project:
 
    - `kube-system`
    - `kube-public`
    - `cattle-system`
    - `cattle-alerting`<sup>1</sup>
    - `cattle-logging`<sup>1</sup>
    - `cattle-pipeline`<sup>1</sup>
    - `ingress-nginx`

    ><sup>1</sup> Only displays if this feature is enabled for the cluster.

### Cause

Rancher requires these namespaces to be in the **System** project for the upgrade for cluster networking to work correctly.

### Solution

>**Prerequisites:**
>
>Download and setup [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

{{% tabs %}}
{{% tab "Rancher Server Nodes" %}}
1. From **Terminal**, change directories to your kubectl file that's generated during Rancher install, `kube_config_rancher-cluster.yml`. This file is usually in the directory where you ran RKE during Rancher installation.

1. Before repairing networking, run the following two commands to make sure that your nodes are `Ready` and `Healthy`.

    ```
    kubectl get nodes
    NAME              STATUS    ROLES                      AGE       VERSION
    x                 Ready     controlplane,etcd,worker   40m       v1.11.1
    x                 Ready     controlplane,etcd,worker   40m       v1.11.1
    x                 Ready     controlplane,etcd,worker   40m       v1.11.1
    
    kubectl get cs
    NAME                 STATUS    MESSAGE              ERROR
    scheduler            Healthy   ok
    controller-manager   Healthy   ok
    etcd-0               Healthy   {"health": "true"}
    etcd-2               Healthy   {"health": "true"}
    etcd-1               Healthy   {"health": "true"}
    ```

1. Check the `networkPolicy` for all clusters by running the following command. Point toward `kube_config_rancher-cluster.yml`.

        kubectl --kubeconfig kube_config_rancher-cluster.yml get cluster -o=custom-columns=ID:.metadata.name,NAME:.spec.displayName,NETWORKPOLICY:.spec.enableNetworkPolicy
    
        ID        NAME      NETWORKPOLICY
        c-59ptz   custom    <nil>
        local     local     <nil>
        

1. Disable the `networkPolicy` for all clusters, still pointing toward your `kube_config_rancher-cluster.yml`.

        kubectl --kubeconfig kube_config_rancher-cluster.yml get cluster -o jsonpath='{range .items[*]}{@.metadata.name}{"\n"}{end}' | xargs -I {} kubectl --kubeconfig kube_config_rancher-cluster.yml patch cluster {} --type merge -p '{"spec": {"enableNetworkPolicy": false}}' 

    >**Tip:** If you want to keep `networkPolicy` enabled for all created clusters, you can run the following command to disable `networkPolicy` for `local` cluster (i.e., your Rancher Server nodes):
    >
    >```
     kubectl --kubeconfig kube_config_rancher-cluster.yml patch cluster local --type merge -p '{"spec": {"enableNetworkPolicy": false}}' 
     ``` 

1. Check the `networkPolicy` for all clusters again to make sure the policies have a status of `false `.

        kubectl --kubeconfig kube_config_rancher-cluster.yml get cluster -o=custom-columns=ID:.metadata.name,NAME:.spec.displayName,NETWORKPOLICY:.spec.enableNetworkPolicy
        
        ID        NAME      NETWORKPOLICY
        c-59ptz   custom    false
        local     local     false

1. Now remove all `networkpolicies` from system namespaces.  Run this command for each cluster, using the kubeconfig generated by RKE.

    ```
    for namespace in kube-system kube-public cattle-system cattle-alerting cattle-logging cattle-pipeline ingress-nginx; do
        kubectl --kubeconfig kube_config_rancher-cluster.yml -n $namespace delete networkpolicy --all;
    done
    ```

1. Wait a few minutes and then log into the Rancher UI. 

    - If you can access Rancher, you're done, so you can skip the rest of the steps.
    - If you still can't access Rancher, complete the steps below.

1. Force your pods to recreate themselves by entering the following command.

    ```
    kubectl --kubeconfig kube_config_rancher-cluster.yml delete pods -n cattle-system --all
    ```
    
1. Log into the Rancher UI and view your clusters. Created clusters will show errors from attempting to contact Rancher while it was unavailable. However, these errors should resolve automatically.
 
{{% /tab %}}
{{% tab "Rancher Launched Kubernetes" %}}
<br/>
If you can access Rancher, but one of the clusters you launched using Rancher has no networking, repair them by removing their networkpolicies as well.
You can do this by running the command below in either of the following contexts:

- From the cluster's [embedded kubectl shell]({{< baseurl >}}/rancher/v2.x/en/k8s-in-rancher/kubectl/#accessing-clusters-with-kubectl-shell).
- By [downloading the cluster kubeconfig file and running it]({{< baseurl >}}/rancher/v2.x/en/k8s-in-rancher/kubectl/#accessing-clusters-with-kubectl-and-a-kubeconfig-file) from your workstation.

    ```
    for namespace in kube-system kube-public cattle-system cattle-alerting cattle-logging cattle-pipeline ingress-nginx; do
      kubectl --kubeconfig kube_config_rancher-cluster.yml -n $namespace delete networkpolicy --all;
    done
    ``` 
 
{{% /tab %}}
{{% /tabs %}}


