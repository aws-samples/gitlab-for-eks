---
title: "Integrating Amazon EKS with the GitLab Kubernetes Agent"
weight: 50
chapter: true
draft: false
pre: '<i class="fa fa-film" aria-hidden="true"></i>'
---


# Integrating Amazon EKS with the GitLab Kubernetes Agent

This workshop focuses specifically on using the GitLab Kubernetes Agent to accomplish integration of a GitLab Instance (including GitLab.com SaaS) with an EKS cluster for managing cluster applications that are built and tested by GitLab. 

### New GitLab Kubernetes Agent Based Integration

Previous to the availability of the GitLab Kubernetes Agent, GitLab cluster integration was done via an SSL connection to the Kubernetes Control API endpoint for a cluster using a certificate (Known as “Certificate Connection”). All cluster deployment happened as a matter of CI pushes to the cluster API endpoint. The Kubernetes Cluster endpoint had to be network accessible to whatever GitLab instance was being used - including if using GitLab.com SaaS.

With the creation of the GitLab Kubernetes Agent three key capabilities are gained:

1. The GitLab K8s Agent can function in a pure GitOps management mode to perform pull only deployments and configuration management like other GitOps management agents. 
2. Since this agent connection is initiated from inside the cluster, no Kubernetes Control API has to be exposed to the network.
3. When CI/CD based pushes are still desired, the GitLab Runner now "tunnels" through the agent connection. This improves the security posture of GitLab's existing CI/CD integration even when GitOps agent management is not being used. This is the pattern used for continuing to use Auto DevOps pipelines when integrating EKS using the GitLab K8s Agent.
4. A CI runner is generally still needed to build the application containers that are used by the GitLab Agent GitOps deployment mode.

{{< admonition type=tip title="Tip" open=true >}}
The first part of this workshop creates a GitLab Integrated EKS cluster using GitLabs most recent integration technology the GitLab Kubernetes Agent. It does it in a simple, inexpensive and secure way. **The EKS cluster setup sections can be used to create a GitLab Integrated EKS cluster for any kind of GitLab training, demoing or experimenting.**
{{< /admonition >}}

### A Production Pattern That is Training Ready

Read more at: [A Production Pattern That is Training Ready]({{< relref "./010_introduction/learning-outcomes-and-reusable-patterns.md#reusable-patterns-for-production-and-training" >}})

## In This Workshop
{{% children style="h3" %}}