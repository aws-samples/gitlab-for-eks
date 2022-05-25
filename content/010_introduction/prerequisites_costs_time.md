---
title: "Prerequisites, Costs and Time"
weight: 02
chapter: true
draft: false
description: "What You Need To Get Started, Cost Estimates, Time Estimates, Name Substitutions."
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

## Instructor-Led Notes

If you are the instructor for an instructor-led version of this workshop, please read [Instructor-Led Notes]({{< relref "instructor-led-notes.md" >}})

## What You Must Bring

If infrastructure experience of deploying clusters is desired for participants, then participants can do all instructor led steps as well even if an instructor is present. If participants will have their own cluster, then also ensuring they each have their own AWS account will allow the exercises to be performed without modifications or AWS Account resource conflicts and therefore go faster as well.

| Item                                                         | Self-Paced           | Instructor Led                                               |
| ------------------------------------------------------------ | -------------------- | ------------------------------------------------------------ |
| An AWS Account                                               | Participant Provides | Instructor Provides - 1 Per Class                            |
| A GitLab Classroom Group                                     | Participant Creates  | Instructor Provides - 1 Per Class                            |
| An EKS Cluster                                               | Participant Creates  | Instructor Provides - 1 Per Class                            |
| A GitLab User ID                                             | Participant Creates  | Instructor Provides <br />If Not Running from <br />GitLab.com SaaS |
| Access to Public Projects in https://gitlab.com/guided-explorations/gl-k8s-agent (or exports of these projects if air gapped) | Participant Accesses | Instructor Exports/Iports for Offline Training               |
| A GitLab Self-Managed Runner (Easy Button Setup Provided)<br />**Note:** Only needed when using free accounts and/or an ultimate trial on GitLab.com SaaS. Can use existing runners if using licensed users or self-managed GitLab instances. | Participant Creates  | Instructor Provides - 1 Per Class                            |

## Time Estimates

These timing estimates are just for completing exercises and do not include breaks nor handling individual assistance.

| Activity                                                     | Keyboard Time | Automation Wait Time | Total Time   | Do As Prep? <br />(Only When Instructor Led) |
| ------------------------------------------------------------ | ------------- | -------------------- | ------------ | -------------------------------------------- |
| **Shared Resources Deployment Activities** <br />Prep steps can be deployed once per group if EKS deployment and integration is not the main focus. For individuals or those practicing cluster setup and integration, these can be done by each participant as well | **65 mins**   | **100 mins**         | **165 mins** |                                              |
| Prep: GitLab Group and AWS Account                           | 20 mins       | 20 mins              |              | Yes                                          |
| Prep Lab 2.1: Provision a Kubernetes Cluster Using The AWS EKS Quick Start | 5 mins        | 60 mins              |              | Yes                                          |
| Prep Lab 2.2: Prepare GitLab classgroup and Deploy a Runner  | 20 mins       | 20 mins              |              | Yes                                          |
| Prep Lab 2.3: Deploy a Runner and Use GitLab K8s Agent to Integrate The Cluster with GitLab | 20 mins       | 2 mins               |              | Yes as a Demo                                |
| **GitLab CD and Auto DevOps via The GitLab Kubernetes Agent**  <br />**Note:** Can be skipped if not relevant to participant use cases. | **20 mins**   | **15 mins**          | **35 mins**  |                                              |
| Lab 3.1: Create a Personal Group                             | 5 mins        | N/A                  |              |                                              |
| Lab 3.2: GitLab Auto DevOps via the K8s Agents<br /><br />**Outcome:** Participants understand that traditional CI/CD Push operations (including Auto DevOps) are still supported with the GitLab Kubernetes Agent Cluster integration method. | 15 mins       | 15 mins              |              |                                              |
| **4. GitLab GitOps via The GitLab Kubernetes Agent** <br />**Note:** Can be skipped if not relevant to participant use cases. | **80 mins**   | **20 mins**          | **100 mins** |                                              |
| Lab 4.1: Create a Personal Group                             | 1 mins        | N/A                  |              |                                              |
| Lab 4.2: Prepare the Application Project<br /><br />**Outcome:** Participants create an Application Creation project that demonstrates a GitOps best practice of cleanly seperating Container Build from Environment Deployment. This also cleanly seperates the need for GitLab runners for Build, while the GitLab Agent alone can perform deployment. | 20 mins       | 2 mins               |              |                                              |
| Lab 4.3: Prepare the Environment Deployment Project<br /><br />**Outcome:** Participants create an Environment Deployment project that demonstrates a GitOps best practice of cleanly seperating Container Build from Environment Deployment. The GitOps agent is completely responsible for pulling the application onto the clusters. | 25 mins       | 5 mins               |              |                                              |
| Lab 4.4: Link and Test Projects<br /><br />**Outcome:** Observe how changes in the Application Project are consumed by the Environment Deployment project. At this point, the only changes are in application manifests. | 5 mins        | 3 mins               |              |                                              |
| Lab 4.5: Setup the GitOps Pull Agent<br /><br />**Outcome:** Participants enable the GitOps mode of the GitLab Agent and see their appilcation manifests deploy. | 10 mins       | 5 mins               |              |                                              |
| Lab 4.6: Update the Application Project<br /><br />**Outcome:** Participants update the Application Code and watch the automation cascade between both projects and into both environments. | 20 mins       | 5 mins               |              |                                              |
| **GitLab Review Environments and Security Scanning for GitOps (optional)**<br />Builds on top of section 4 labs. Can stop at any lab if subsequent labs are not of interest. | **70 Mins**   | **50 Mins**          | **120 Mins** |                                              |
| Lab 5.1: Add Environments and Security Scanning to the Application Project (Optional)<br />**Outcome:** Learn to setup GitLab Environments and Security Scanning work with GitOps workflows. | 20 Mins       | 10 Mins              |              |                                              |
| Lab 5.2: Merge Request and Review Environment (Optional)<br />**Outcome:** Learn to setup GitLab Dynamic Per-Feature Branch Environments and MR Security Results work with GitOps workflows. | 10 Mins       | 5 Mins               |              |                                              |
| Lab 5.3: Merge MR and Deploy to Environment (Optional)<br />**Outcome:** Observe full workflow of changes with Dynamic Environments and MRs to push all the way to production environment with GitOps workflows. | 10 Mins       | 10 Mins              |              |                                              |
| Lab 5.3 Part B: GitOps CD Pull Changes to Production (Optional)<br />**Outcome:** Observe full workflow of changes with Dynamic Environments and MRs to push all the way to production environment with GitOps workflows. | 10 Mins       | 5 Mins               |              |                                              |
| Lab 5.4: Add Kubernetes Manifest Security Scanning (Optional)<br />**Outcome:** Learn to setup GItLab Security Scanning of Kubernetes Manifests with GitOps workflows. | 15 Mins       | 10 Mins              |              |                                              |
| Lab 5.5: Add Cluster Image Security Scanning (Optional)<br />**Outcome:** Learn to setup GItLab Security Scanning of Kubernetes Clister Images (that are used in your cluster, but not developed in your CI)  with GitOps workflows. | 5 Mins        | 10 Miins             |              |                                              |

## Cost Estimates

Based on us-east-1

| Item                                                | Estimated Cost          | Fixed or <br />Varaible | Reduce or Eliminate                                    |
| --------------------------------------------------- | ----------------------- | ---------------------- | ------------------------------------------------------ |
| K8s Node Instances (2 x t3.medium spot)             | $0.03/hr (0.014/hr x 2) | Variable               | Cost = $0 by Scaling EKS Nodes to Zero                       |
| Bastion Host (1 x t2.micro)                         | $0.05/hr                | Variable               | Cost = $0 by Scaling EKS Nodes to Zero                       |
| EKS Control Plane                                   | $0.10/hr ($73/Mo)       | Fixed                  | Cannot be turned off, only destroying elimintates cost |
| ELB for Ingress Controller (Auto DevOps Requirement) | 0.025/hr ($18/Mo)       | Fixed                  | Cannot be turned off, only destroying elimintates cost |

- Total Fixed Costs Whether Scaled to 0 or not: 0.35/hr ($25/wk, $101/mo)
- Variable Costs While Turned on (for two t3.medium spot instances): $0.08/hr
- The EKS Quick Start and Ingress controller make teardown and setup relatively easy if avoiding the fixed costs between periods of usage is necessary.

## Values to Substitute Throughout Exercises

**Note:** The following values are used in the example and will usually be different in your project - be careful to keep track of what your values are to substitute them consistently throughout. If you are an instructor, provide the list of what the values should be to the class from the prep steps you have completed.

| Description                                       | Substitution Value                                           | Actual Value In Your Case |
| ------------------------------------------------- | ------------------------------------------------------------ | ------------------------- |
| **<u>Within AWS</u>**                             |                                                              |                           |
| AWS EKS Quick Start Config set name               | spot-t2-medium-v120-paramset                                 |                           |
| AWS EKS Quick Start cluster name                  | spot2azuseast2                                               |                           |
| AWS region                                        | us-east-2                                                    |                           |
| **<u>Within GitLab</u>**                          |                                                              |                           |
| Top Level Group (Can be a GitLab.com Namespace)   | Classgroup                                                   |                           |
| Cluster management group and project path         | classgroup/cluster-management                                |                           |
| Personal group                                    | yourpersonalgroup (subgroup of classgroup)                   |                           |
| agent path segment (which is also the agent name) | spot2azuseast2-agent1<br />**Note:** does not have to match EKS name like it does here |                           |
| Cluster Auto DevOps domain with SSL               | <mark>\<the Load Balancer IP\></mark>.nip.io                 |                           |

### A Production Pattern That is Training Ready

Please read [Reusable Patterns for Production and Training]({{< relref "../010_introduction/learning-outcomes-and-reusable-patterns.md#reusable-patterns-for-production-and-training" >}})

