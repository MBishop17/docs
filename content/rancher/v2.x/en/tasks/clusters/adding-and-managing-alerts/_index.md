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

After you create an alert, you edit its settings or toggle it on or off.

- [Adding System Service Alerts]({{< baseurl >}}/rancher/v2.x/en/tasks/clusters/adding-and-managing-alerts/adding-system-service-alerts/)
- [Adding Node Resource Event Alerts]({{< baseurl >}}/rancher/v2.x/en/tasks/clusters/adding-and-managing-alerts/adding-node-resource-event-alerts/)
- [Adding Node Alerts]({{< baseurl >}}/rancher/v2.x/en/tasks/clusters/adding-and-managing-alerts/adding-node-alerts/)
- [Adding Node Selector Alerts]({{< baseurl >}}/rancher/v2.x/en/tasks/clusters/adding-and-managing-alerts/adding-node-selector-alerts/)
- [Managing Cluster Alerts]({{< baseurl >}}/rancher/v2.x/en/tasks/clusters/adding-and-managing-alerts/managing-cluster-alerts/)
  
>**Tip:** If you want to set alerts for specific pods or workloads in a project, see [Adding and Managing Project Alerts]({{< baseurl >}}/rancher/v2.x/tasks/projects/#adding-project-alerts).