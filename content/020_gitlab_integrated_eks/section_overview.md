---
title: "Section Overview"
weight: 15
chapter: true
draft: false
description: "Explanation of Section"
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

### Section 2: Shared Resources Deployment Activities

Prep steps can be deployed once per group if EKS deployment and integration is not the main focus. For individuals or those practicing cluster setup and integration, these can be done by each participant as well<br />**Dependencies:** All labs depend on section 2 because it sets up the cluster and GitLab integration.<br />**Outcomes**: Deploy EKS with official AWS IaC [in a cost-efficient format for training](#simple--inexpensive-eks-pattern-reusable-for-training) (performed by instructor for instructor-led)

### Visual Overview of GitLab Agent Group Level Cluster Integration

{{%expand "Click Here To Expand a Visual Overview of GitLab Agent Group Level Cluster Integration" %}}![gl-k8s-agent-least-config-least-privilege](../040_gitlab_gitops_via_agent/gl-k8s-agent-least-config-least-privilege.png)

**Key Aspects:**

1. While the cluster integration project lives within the group, the configuration file can point to it’s parent group.
2. Specific GitLab variables are set at the group level and inherit by all downbound groups.
3. KUBE_CONTEXT and KUBE_INGRESS_BASE_DOMAIN are shared with all projects to ensure they work appropriately in the cluster.
4. The KUBE_NAMESPACE variable uses project variables that are unique to ensure all projects go into the cluster uniquely
   1. KUBE_NAMESPACE is overridden in GitOps projects to include the environment.
5. REG_READ_TOKEN - Only Read Access, Only to the Container Registry is given. In labs this is a Deployment Token. In production it should use a Group Access Token to give read permissions to all App Build Project container registries to all related Environment Deployment Projects.

{{% /expand%}}

### Visual Overview of Agent Registration Process

{{%expand "Click Here To Expand a Visual Overview of The Agent Registration Process" %}}

![Agent_Registration_for_DevOps_or_Any_CD_Push_to_K8s.png](./Agent_Registration_for_DevOps_or_Any_CD_Push_to_K8s.png){{% /expand%}}