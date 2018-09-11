---
title: Resource Quotas
weight: 5000
---
_Resource quotas_ are a Rancher feature that limits the resources available to a project or namespace.

In situations where several teams share a cluster, one team may overconsume the resources available: CPU, memory, storage, services, Kubernetes objects like pods or secrets, and so on.  To prevent this overconsumption, you can apply a resource quota, which creates a pool of resources that the project's namespaces can use.

## Rancher Resource Quotas vs. Native Kubernetes Resource Quotas

Resource quotas in Rancher work similarly to how they do in the [native version of Kubernetes](https://kubernetes.io/docs/concepts/policy/resource-quotas/). However, Rancher's version of resource quotas have a few key differences from the Kubernetes version. The following table explains the key differences between the two quota types.


| Rancher Resource Quotas                                    | Kubernetes Resource Quotas                               |
| ---------------------------------------------------------- | -------------------------------------------------------- |
| Applies to projects.                                       | Applies to namespaces.                                   |
| Creates resource pool for all namespaces in project.       | Applies static resource limits to individual namespaces. |
| Applies resource quotas to namespaces through inheritance. | Applies only to the assigned namespace.                  |

## Resource Quota Types

When you create a resource quota, you are configuring the pool of resources available to the project. You can set the following research limits for each project. Expand the section below to see each resource type available.


{{% accordion id="resource-types" label="Resource Types" %}}

| Resource Type            | Description                                                                                                                                                                                       |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CPU Limit                | The maximum amount of CPU (in [millicores](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-cpu)) allocated to the project/namespace.<sup>1</sup> |
| CPU Reservation          | The minimum amount of CPU (in millicores) guaranteed to the project/namespace.<sup>1</sup>                                                                                                        |
| Memory Limit             | The maximum amount of memory (in bytes) allocated to the project/namespace.<sup>1</sup>                                                                                                           |
| Memory Reservation       | The minimum amount of memory (in bytes) guaranteed to the project/namespace.<sup>1</sup>                                                                                                          |
| Storage Reservation      | The minimum amount of storage (in gigabytes) guaranteed to the project/namespace.                                                                                                                 |
| Services Load Balancers  | The maximum number of load balancers services that can exist in the project/namespace.                                                                                                            |
| Services Node Ports      | The maximum number of node port services that can exist in the project/namespace.                                                                                                                 |
| Pods                     | The maximum number of pods that can exist in the project/namespace in a non-terminal state (i.e., pods with a state of `.status.phase in (Failed, Succeeded)` equal to true).                     |
| Services                 | The maximum number of services that can exist in the project/namespace.                                                                                                                           |
| ConfigMaps               | The maximum number of ConfigMaps that can exist in the project/namespace.                                                                                                                         |
| Persistent Volume Claims | The maximum number of persistent volume claims that can exist in the project/namespace.                                                                                                           |
| Replications Controllers | The maximum number of replication controllers that can exist in the project/namespace.                                                                                                            |
| Secrets                  | The maximum number of secrets that can exist in the project/namespace.                                                                                                                            |

>**<sup>1</sup>** In the quota, if you set CPU or Memory limits, all containers you create in the project/namespace must explicitly satisfy the quota. See the [Kubernetes documentation](https://kubernetes.io/docs/concepts/policy/resource-quotas/#requests-vs-limits) for more details.

 
{{% /accordion %}}

## Quota Project and Namespace Limits

When setting up your resource quotas, you will configure two values: a **Project Limit** and a **Namespace Default Limit**.

### Project Limits

### Namespace Default Limits


Each namespace inherits this quota unless you assign it one directly, which overrides the default. We recommend assigning most quota templates this way.

>**Note:** When you apply a project quota, any resource quotas already applied to the project namespaces are replaced with the default quota template.
