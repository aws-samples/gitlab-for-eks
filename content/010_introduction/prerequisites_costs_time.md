---
title: "Prerequisites and Costs"
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
| A Computer<br />**Note:** no changes will need to be done to the laptop and the operating system does not matter. | Participant Provides | Participant Provides                                         |
| An AWS Account                                               | Participant Provides | Instructor Provides - 1 Per Class                            |
| A GitLab Classroom Group                                     | Participant Creates  | Instructor Provides - 1 Per Class                            |
| An EKS Cluster                                               | Participant Creates  | Instructor Provides - 1 Per Class                            |
| A GitLab User ID                                             | Participant Creates  | Instructor Provides <br />If Not Running from <br />GitLab.com SaaS |
| Access to Public Projects in https://gitlab.com/guided-explorations/gl-k8s-agent (or exports of these projects if air gapped) | Participant Accesses | Instructor Exports/Imports for Offline Training              |
| A GitLab Self-Managed Runner (Easy Button Setup Provided)<br />**Note:** Only needed when using free accounts and/or an ultimate trial on GitLab.com SaaS. Can use existing runners if using licensed users or self-managed GitLab instances. | Participant Creates  | Instructor Provides - 1 Per Class                            |

## Time Estimates

Time estimates live with learning outcomes in [Learning Outcomes and Time Estimates]({{< relref "../010_introduction/learning-outcomes-and-reusable-patterns.md#learning-outcomes-and-time-estimates" >}})

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

