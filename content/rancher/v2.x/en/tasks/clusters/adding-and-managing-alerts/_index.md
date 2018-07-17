---
title: Adding and Managing Cluster Alerts
weight: 3575
draft: true
---

You can create alerts that monitor the resources in your Kubernetes cluster. When an event occurs that triggers the alert, you'll receive notification about the issue.

For clusters, you can set alerts to monitor:

  - The state of your nodes.
  - The system services that manage your Kubernetes cluster.
  - The resource events from specific resource types.

After you create an alert, you can edit its settings or toggle it on or off.

>**Tip:** If you want to set alerts for specific pods or workloads in a project, see [Adding and Managing Project Alerts]({{< baseurl >}}/rancher/v2.x/tasks/projects/#adding-project-alerts).

## Prerequisite

You must [add a notifier]({{< baseurl >}}/rancher/v2.x/tasks/clusters/adding-notifiers/), which is the medium that you'll be informed of the alert.

## To Add an Alert

1. From the **Global** view, open the cluster that you want to configure alerts for.

1. From the main menu, select **Tools > Alerts**. Then click **Add Alert**.

1. Enter a **Name** for the alert that describes its purpose.

1. Based on the type of alert you want to create, complete one of the instruction subsets below.
{{% accordion id="system-service" label="System Service Alerts" %}}
This alert type monitors for events that affect one of the Kubernetes master components, regardless of the node it occurs on.

1. Select the **System Services** option, and then select an option from the drop-down.

  - [controller-manager](https://kubernetes.io/docs/concepts/overview/components/#kube-controller-manager)
  - [etcd](https://kubernetes.io/docs/concepts/overview/components/#etcd)
  - [scheduler](https://kubernetes.io/docs/concepts/overview/components/#kube-scheduler)

1. Select the urgency level of the of alert. The options are:

    - **Critical**: Most urgent
    - **Warning**: Normal urgency
    - **Info**: Least urgent

    Select the urgency level of the alert based on how many nodes fill the role within your cluster. For example, if you have 5 node cluster, and all 5 nodes run `etcd`, select **Info**. However, if only 1 node in your cluster runs `etcd`, select **Critical**.
{{% /accordion %}}
{{% accordion id="resource-event" label="Resource Event Alerts" %}}
This alert type monitors for any event that involves a selected resource type, rather than a unique event.

1. Choose the type of resource event that triggers an alert. The options are:

  - **Normal**: triggers an alert when resource events occur, either expected or unexpected.
  - **Warning**: triggers an alert when unexpected resource events occur.

1. Select a resource type from the **Choose a Resource** drop-down that you want to trigger an alert.

  - [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
  - [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
  - [Node](https://kubernetes.io/docs/concepts/architecture/nodes/)
  - [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/)
  - [StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)

1. Select the urgency level of the of alert.

    - **Critical**: Most urgent
    - **Warning**: Normal urgency
    - **Info**: Least urgent

    Select the urgency level of the alert by considering factors such as how often the event occurs or its importance. For example:

    - If you set a normal alert for pods, you're likely to receive alerts often, and individual pods usually self-heal, so select an urgency of **Low**.
    - If you set a warning alert for StatefulSets, its very likely to impact operations, so select an urgency of **Critical**.


{{% /accordion %}}
{{% accordion id="node" label="Node Alerts" %}}
This alert type monitors for events that occur on a specific node.

1. Select the **Node** option, and then make a selection from the **Choose a Node** drop-down.

1. Choose an event to trigger the alert.

  - **Not Ready**: Sends you an alert when the node is unresponsive.
  - **CPU usage over**: Sends you an alert when the node raises above an entered percentage of its processing allocation.
  - **Mem usuage over**: Sends you an alert when the node raises above an entered percentage of its memory allocation.

1. Select the urgency level of the of alert.

    - **Critical**: Most urgent
    - **Warning**: Normal urgency
    - **Info**: Least urgent

    Select the urgency level of the alert based on its impact on operations. For example, an alert triggered when a node's CPU raises above 60% deems a urgency of **Info**, but a node that is **Not Ready** deems an urgency of **Critical**.
{{% /accordion %}}
{{% accordion id="node-selector" label="Node Selector Alerts" %}}
This alert type monitors for events that occur on any node on marked with a key value pair. For more information, see the Kubernetes documentation for [NodeSelectors](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#nodeselector).

1. Select the **Node Selector** option, and then click **Add Selector** to enter a nodeSelector key value pair. This nodeSelector should be applied to one or more of your nodes. Add as many selectors as you'd like.

1. Choose an event to trigger the alert.

  - **Not Ready**: Sends you an alert when selected nodes are unresponsive.
  - **CPU usage over**: Sends you an alert when selected nodes raise above an entered percentage of processing allocation.
  - **Mem usuage over**: Sends you an alert when selected nodes raise above an entered percentage of memory allocation.

1. Select the urgency level of the of alert.

    - **Critical**: Most urgent
    - **Warning**: Normal urgency
    - **Info**: Least urgent

      Select the urgency level of the alert based on its impact on operations. For example, an alert triggered when a node's CPU raises above 60% deems a urgency of **Info**, but a node that is **Not Ready** deems an urgency of **Critical**.
{{% /accordion %}}
1. Finally, choose the notifiers send you alerts. If you've configured multiple notifiers, you can receive alerts in multiple mediums.