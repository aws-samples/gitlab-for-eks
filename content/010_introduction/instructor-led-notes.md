---
title: "Instructor-Led Notes"
weight: 03
chapter: true
draft: false
description: "Important information for instructor-led sessions conducted in classrooms."
---

# Instructor-Led Notes

{{< toc >}}

## Pre-doing

* Do the prereading :)
* Instructors should perform the entire set of labs before their first delivery:
  1. For full preparation and understanding of what students are expected to perform.
  2. To have a demo system with completed labs to demonstate any additional concepts or overview. (performing the labs creates a demo environment for you)
  3. To have a hot spare environment if something ends up being wrong with the lab environment - this copy could actually be scaled to handle the class if needed.
* 3-5 days before **each** delivery instructors may want to validate the entire set of labs - but especially section 2:
  1. AWS, GitLab and AWS Quick Starts all undergo continuous updates - occassionally these updates create challenges for labs.
  2. The labs have been constructed to be as version agnostic as possible - but it is not possible to forsee all possible interactions and challenges.

## Prereading

### Required

-  [Prerequisites, Costs and Time]({{< relref "prerequisites_costs_time.md" >}}) please pay attention to the Instructor-Led columns and call outs.
-  [Reusable Patterns for Production and Training]({{< relref "../010_introduction/learning-outcomes-and-reusable-patterns.md#reusable-patterns-for-production-and-training" >}}) - outlines the details of how the GitLab structure in these labs enables an entire classroom to share specific resources without conflicts.
- [Simple, Inexpensive, Secure for Training]({{< relref "../010_introduction/learning-outcomes-and-reusable-patterns.md#simple--inexpensive-eks-pattern-reusable-for-training" >}}) - discusses how using the EKS Quick Start and other configuration details keep costs very low for classroom and other learning scenarios.
- [Tuning and Troubleshooting Guide]({{< relref "tuning_and_troubleshooting.md" >}}) - discusses how to troubleshoot problems and how to tune the setup for classrooms.
- [Reporting Problems]({{< relref "reporting_problems_or_features.md" >}}) - discusses how using the EKS Quick Start and other configuration details keep costs very low for classroom and other learning scenarios.

### Suggested

- [World Greetings Env 1 README.md](https://gitlab.com/guided-explorations/gl-k8s-agent/gitops/envs/world-greetings-env-1/-/blob/main/README.md)
- [World Greetings Env 1 IMPLEMENTATION.md](https://gitlab.com/guided-explorations/gl-k8s-agent/gitops/envs/world-greetings-env-1/-/blob/main/IMPLEMENTATION.md)

## Instruction and Demonstrations

- Some labs have expandable “Visual Overviews” - these make excellent conceptual talks about what the lab is doing.

## Tuning and Troubleshooting Guide

The [Tuning and Troubleshooting Guide]({{< relref "tuning_and_troubleshooting.md" >}}) has a special section for “Classrooms” which includes helpful tips for scaling compute resources for the classroom.

## Reporting Problems

[How to Report Problems]({{< relref "reporting_problems_or_features.md" >}}) after [Troubleshooting]({{< relref "tuning_and_troubleshooting.md" >}})

## Instructor Led or Self-Paced

**Instructor Led Non-Infrastruture** - allows the shared GitLab and AWS resources to be prestaged and then allow participants to focus on using GitLab to build applications for AWS EKS.  Also does not require that participants have AWS accounts, nor Kubernetes cluster access.

**Instructor Led with Infrastructure** - participants can use per-student AWS accounts and per-student EKS Clusters to learn how to setup these components - the runtime of this format is several hours longer. Per-student AWS accounts will ensure no resources conflict and participants do not need to make too many region oriented modifications to the exercises and out of the box cloud formation templates.

**Self-Paced** - participants need to perform all infrastructure setup, unless they have a team that can preset the instructor prep portions for them.

### Creation of Infrastructure Prerequisites

Instructors may be tempted to have everyone setup their own cluster in their own AWS account or setup their own GitLab Server - you should be certain that your workshop audience really needs to do these infrastructure provisioning activities in their production jobs before making them part of the workshop. It is a classic antipattern to lose your audience by putting them through hours of prep work before they get to the meat of what they want to learn for their day job.

If a given run of the workshop is actually for a team who needs to perform cluster and GitLab setup - then participants can simply complete all Instructor-Led sections. It is best to isolate with an AWS account per-participant in this use case.

## Lab Notes

### Lab 4.5 Setup the GitOps Pull Agent => Check With Instructor Warning

The Lab “Setup the GitOps Pull Agent” requires entries for student projects in a file in the classroom group level “Cluster Management” project. This approach is much simpler than each student gaining cluster access to install their own agent.

**Please read the “Check With Instructor” note in that lab** to assess and select an approach that will work within the timeframe and teaching objectives you are operating under.

### Lab 4.6 Update The Application Build Project and Deploy to Production Demo

Since we have the GitLab Agent doing both CD Push (and Labs in 3.x show it first) as well as CD Pull, it can be challenging for participants to perceive when the Agent is receiving a Push from a runner and when it is independently pulling using manifests. The exercises do address this by having participants examine the contents of the manifest update jobs and the changed manifests - but to really drive it home you can tail the log of the Kubernetes Agent log as a “running” demo while they complete exercises 4.5 and 4.6.

In 4.5 and this lab you may also want to have a trace of the GitLab Agent displaying while participants are working in order to reinforce that the GitLab Agent is **pulling** the changes. Otherwise it can seem as though they are being pushed by CI jobs.

To do this, login to the EKS Bastion host the same was as was done in “Prep Lab 2.3: Use GitLab K8s Agent to Integrate The Cluster with GitLab” to install the GitLab Agent. Then run this command `kubectl logs -f -l=app=gitlab-agent -n gitlab-agent`

For more troubleshooting information visit [Troubleshooting the GitLab agent for Kubernetes](https://docs.gitlab.com/ee/user/clusters/agent/troubleshooting.html)

