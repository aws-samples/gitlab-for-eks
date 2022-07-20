---
title: "Ultimate GitOps: Deploy Secure Microservices to AWS EKS with the GitLab Agent"
weight: 50
chapter: true
draft: false
pre: '<i class="fa fa-film" aria-hidden="true"></i>'
---


# Ultimate GitOps: Deploy Secure Microservices to AWS EKS with the GitLab Agent

This workshop focuses specifically on integration of a GitLab Instance (including GitLab.com SaaS) with an EKS cluster for managing cluster applications that are built and tested by GitLab. The workshop uses the new GitLab Agent for Kubernetes as it’s Kubernetes connection method. While the agent is new as of this writing it will soon be the only way to integrate GitLab and Kubernetes - so the workshop is also relevant even after the Certificate Connection method is full depreciated.

### New Gitlab Agent for Kubernetes Based Integration

Previous to the availability of the Gitlab Agent for Kubernetes, GitLab cluster integration was done via an SSL connection to the Kubernetes Control API endpoint for a cluster using a certificate (Known as “Certificate Connection”). All cluster deployment happened as a matter of CI pushes to the cluster API endpoint. The Kubernetes Cluster endpoint had to be network accessible to whatever GitLab instance was being used - including if using GitLab.com SaaS.

With the creation of the Gitlab Agent for Kubernetes three key capabilities are gained:

1. Since this agent connection is initiated from inside the cluster, no Kubernetes Control API has to be exposed to the network.
2. When CI/CD based pushes are still desired, the GitLab Runner now "tunnels" through the agent connection. This improves the security posture of GitLab's existing CI/CD integration even when GitOps agent management is not being used. This is the pattern used for continuing to use Auto DevOps pipelines when integrating EKS using the GitLab K8s Agent.
3. The GitLab K8s Agent can function in a pure GitOps management mode to perform pull only deployments and configuration management like other GitOps management agents. 
4. A CI runner is generally still needed to build the application containers that are used by the GitLab Agent GitOps deployment mode.

{{< admonition type=tip title="Tip" open=true >}}
The first part of this workshop creates a GitLab Integrated EKS cluster using GitLabs most recent integration technology the Gitlab Agent for Kubernetes. It does it in a simple, inexpensive and secure way. **The EKS cluster setup sections can be used to create a GitLab Integrated EKS cluster for any kind of GitLab training, demoing or experimenting.**
{{< /admonition >}}

### A Production Pattern That is Training Ready

Read more at: [A Production Pattern That is Training Ready]({{< relref "./010_introduction/learning_outcomes_and_reusable_patterns.md#reusable-patterns-for-production-and-training" >}})

## In This Workshop
{{% children style="h3" %}}