---
title: "Learning Outcomes and Reusable Patterns"
weight: 01
chapter: true
draft: false
description: "The learning outcomes that are designed into this workshop."
---

{{< toc >}}

## Learning Outcomes

| Outcome                                                      | In Labs |
| ------------------------------------------------------------ | ------- |
| Deploy EKS with official AWS IaC in a cost-efficient format for training (performed by instructor for instructor-led) | 2.1-2.2 |
| GitLab EKS Cluster Integration Using GitLab Agent Connection Method (performed by instructor for instructor-led) | 2.3     |
| GitLab CD Push Using GitLab Agent (For all CD Push Scenarios including Auto DevOps) | 3.2     |
| GitLab GitOps Application Build Using GitLab CI              | 4.2     |
| GitLab GitOps Pull Based CD Using GitLab Agent               | 4.3-4.5 |
| Round trip Application Changes to Environment Update Using GitOps Workflow | 4.6     |
| GitLab Merge Requests, Dynamic Review Environments and Security Scanning Using GitLab Agent | 5.1-5.2 |
| Round trip Application Changes to Environment Update Having Used Per-Branch Dynamic Review Environments. | 5.3     |

## Reusable Patterns For Production and Training

### Reusable Patterns for Production

The following patterns are reusable in production environments.

#### For CD Push and GitOps: Group Level Integration With Automatic Per-Project Abstractions

For both major scenarios covered by the Kubernetes Agent, this workshop models doing the integrations at a group level and then ensuring that Kubernetes namespace uniqueness is handled for environments - including Auto DevOps environments. This is similar to how both Auto DevOps and Kubernetes Certificate Connections have traditionally operated, but it requires that specific patterns are followed to achieve it.

#### CD Push Scenarios: Auto DevOps, Dynamic Review Environments and Security Scanning 

Demonstrates preservation of GitLab Auto DevOps, Auto Dynamic Review Environments and Security Scanning while using GitLab Agent connection method. Does so for **an entire group heirarchy** similar to the former Certificate Connection Method. Done through strategic group placement of Auto DevOps CI/CD Variables and GitLab Agent registration shown here:

![gl-k8s-agent-least-config-least-privilege](gl-k8s-agent-least-config-least-privilege.png)

#### GitOps: Auto DevOps, Dynamic Review Environments and Security Scanning

GitLab GitOps pattern retains usage of Auto DevOps, Dynamic Review Environments and Security Scanning to **qualify new application versions for GitOps scenarios.** This is done through Auto DevOps enablement in the Application Building Project.

#### GitOps: Loose Project Coupling

Loose coupling between the Application Building Project and the Environment Deployment Project **[accomodates many production oriented GitOps deployment scenarios](https://gitlab.com/guided-explorations/gl-k8s-agent/gitops/envs/world-greetings-env-1#loose-project-coupling).**

#### GitOps: Promote Image to Latest Only If Testing Passes

An Application Build Project image is only tagged as “latest” if the pipeline passes. If staging is enabled, it is also a manual step.

#### GitOps: Least Configuration

**Least Configuration** - template repositories for exercises are designed to be self-abstracting so that there is as little manual code updates to the scaffolding as possible before it is ready to run. For instance, the Application Build Project is simply copied to a new sub-group or new name and it is ready to build. The Environment Deployment project requires a manual update to map it to the correct Application Project and it is ready to run. This capability is a result of the above models of Group Level Integration combined with leveraging existing artifact metadata to detect the latest version of an image for incrementing in the Application Build Project and for version update detection in the Environment Deploment Project as depicted here:

![gl-k8s-agent-least-config-least-privilege](gl-k8s-agent-version-tracking-using-existing-metadata.png)

#### GitOps: Least Privilege

**Least Privilege** - the two GitOps projects define the least possible shared privileges at the lowest possible group level. For instance the Application Build Project only defines a read registry permissions for a deployment token and only stores it high enough in the group heirarchy that any consuming Environment Deployment Projects can utilize it. The Environment Deployment Projects define a git read and write token back to themselves and only publish it at their own project level where it is used.

## Reusable Patterns for Training

These exercises configure the GitLab Kubernetes Agent to function similarly to the depreciated certificate connection method in that configuring at a group level enabled the entire subgroup heirarchy of that group to be able to access the cluster and not have name uniqueness conflicts. This is highly desirable in a classroom context and may be desirable for production if the previous group level GitLab certificate cluster connections were leveraged by your organization.

To accomplish this:

1. A single Kubernetes Cluster is deployed and…
2. Integrated using a single GitLab Kubernetes Agent…
3. That has a management scope of a top level group and all its subgroups and…
4. That services both **CI/CD Push** AND **GitOps** scenarios…
5. Which have **working Review Environments** for participants to observe results of changes…
6. The small amount of code editing is specified as being done in the Web IDE - avoiding all challenges with getting repository cloning working to each participant workstation (but you can still have them do workstation cloning if desired)

This keeps classroom logistics simple.

It is also what leads to the limitation of having all projects be public - if a Kubernetes agent registration is done in the same project as the manifest files, then the project does not have to be public.

{{% notice warning %}}

In a real world scenario there can be many GitLab Agents in many GitLab groups and projects integrating to the same Kubernetes cluster and/or agents integrating to many independent clusters. The agents can be registered at any group level and could be dedicated to just CI/CD Push or just GitOps.

{{% /notice %}}

## Simple & Inexpensive EKS Pattern Reusable for Training

{{% notice warning %}}
A key difference between the legacy GitLab Kubernetes Certificate cluster connection and the GitLab Kubernetes agent configuration is that you cannot multi-attach a single cluster agent to multiple locations in the group heirachy.
{{% /notice %}}

### Rapid Deployment & Simplicity

- **AWS EKS Quick Start** - IaC that results in autoscaling EKS cluster running on spot instances
- **Simple Zero Footprint IaC Workstation** - EKS Quick Start Built-in Bastion Host.
  - The bastion host automatically comes with aws cli, kubectl, helm.
  - The bastion host machine has IAM permissions to the cluster.
  - Multiple folks can login or the bastion host ASG can be resized to provide a bastion for each participant.
- **Simple and Fast SSL and DNS** - leveraging Kubernetes Ingress, Cert Manager and nip.io DNS support.

### Security Best Practices

- **Private K8s Control API Endpoint via GitLab K8s Agent** - GitLab integration without exposing K8s Control API Endpoint.
- **SSM Session Manager to Bastion Host for CLI Admin** - SSM allows IAM access control (instead of keys) and full audit logging of all commands run on the cluster to CloudWatch. No ports or network routes need to be setup to access the bastion host. SSM Session Manager web console allows zero workstation configuration.


### Cost Control

- uses a 2 AZ, 2 spot Node Cluster with small t2 instances in us-east-2 region - keeping these settings makes the process as simple as possible
- With the EKS Cluster Autoscaler Installed to allow use of small instances that scale when used by more people or projects
- With Spot Instances Nodes for Cost Control
- With "Scale to Zero Instances" support (EKS Control plane is a service so does not need nodes to run)
- With cluster node availability scheduling for cost control when the cluster is not in use
