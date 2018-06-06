---
title: Adding System Service Alerts
weight: 25
draft: true
---

You can create an alert that notifies you when one of the Kubernetes [Master Components](https://kubernetes.io/docs/concepts/overview/components/#master-components) in your cluster is unhealthy.

>**Prerequisite:** Before setting up an alert, you must [add a notifier]({{< baseurl >}}/rancher/v2.x/tasks/clusters/adding-notifiers/), which is the medium that you'll be informed of the alert.

1. From the **Global** view, open the cluster that you want to configure alerts for.

1. From the main menu, select **Tools > Alerts**. Then click **Add Alert**.

1. Enter a **Name** for the alert that describes its purpose.

1. Select the **System Services** option, and then select an option from the drop-down.

  - [controller-manager](https://kubernetes.io/docs/concepts/overview/components/#kube-controller-manager)
  - [etcd](https://kubernetes.io/docs/concepts/overview/components/#etcd)
  - [scheduler](https://kubernetes.io/docs/concepts/overview/components/#kube-scheduler)

1. Select the urgency level of the of alert. The options are:

    - **Critical**: Most urgent
    - **Warning**: Normal urgency
    - **Info**: Least urgent

    Select the urgency level of the alert based on how many nodes fill the role within your cluster. For example, if you have 5 node cluster, and all 5 nodes run `etcd`, select **Info**. However, if only 1 node in your cluster runs `etcd`, select **Critical**

1. Finally, choose the notifiers send you alerts. If you've configured multiple notifiers, you can receive alerts in multiple mediums.