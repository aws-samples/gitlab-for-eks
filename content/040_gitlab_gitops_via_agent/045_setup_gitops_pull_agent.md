---
title: "Lab 4.5: Setup the GitOps CD Pull Agent"
weight: 45
chapter: true
draft: false
description: "See GitLab GitOps CD pull deployment and configuration management in action."
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

# Lab 4.5: Setup the GitOps CD Pull Agent

> **Keyboard Time**: 10 mins, **Automation Wait Time**: 5 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

{{< admonition type=gitops title="GitOps Conventions" open=true >}}

1. Monitoring manifests by an agent running in a Kubernetes cluster. This lab configures the GitLab Agent to monitor the manifests in this repository.

{{< /admonition >}}

{{< admonition type=abstract title="Target Outcomes" open=true >}}

1. Configure the Kubernetes Agent to monitor the CI constructed kubernetes manifests.

2. Observe the initial deployment of staging and production via tailing the Kubernetes Agent log and the appearance of the target environments.

   {{< /admonition >}}

{{< admonition type=warning title="Warning" open=true >}}

**For instructor-led classes, this portion will be done by the instructor.**

**If you are not in an instructor-led course, perform the lab as described.**

{{< /admonition >}}

{{< admonition type=quote title="Done By Instructor for Instructor-Led Courses" open=true >}}

1. Logon the cluster administration machine => [Instructions for SSM Session Manager for EKS]({{< relref "../090_appendices/tuning_and_troubleshooting.md#using-the-eks-bastion-for-cluster-administration-with-kubectl-and-helm" >}})

2. Run the following command to tail the kubernetes agent log while deployments are happening:

   `kubectl logs -f -l=app=gitlab-agent -n gitlab-agent`

    <mark class="hlgreen">Leave this view open as you will be instructed to consult it to see the deployment logging activity when the GItLab Agent pulls and processes the kubernetes manifest.</mark>

3. In a web browser *Navigate to* **classgroup/cluster-management**

4. Near the upper right of the page, *Click* **Web IDE** (button)

5. Navigate to the file .gitlab/agents/spotazuseast2-agent/config.yml

6. Add the following to the file only once:

   ```yaml
   gitops:
     manifest_projects:
   
   ```

7. Under “gitops:manifest_projects:” add as below - replacing `_classgroup_` and `_yourpersonalgroup_` with the actual names for your project. Ensure indenting and “gitops:manifest_projects” should only appear once in the entire file.

   <mark class="hlgreen">**For Instructors**: add one of these sections per participant. Ensure indentation is perserved.</mark>

   ````yaml
   
     - id: _classgroup_/_yourpersonalgroup_/world-greetings-env-1
       default_namespace: default
       paths:
       - glob: '/manifests/**/*.yaml'
       reconcile_timeout: 3600s # 1 hour by default
       dry_run_strategy: none # 'none' by default
       prune: true # enabled by default
       prune_timeout: 360s # 1 hour by default
       prune_propagation_policy: foreground # 'foreground' by default
       inventory_policy: must_match # 'must_match' by default
   ````

8. Final result should be something like this (including indentation - with repeating “id” sections for each participant if in a classroom):

   ````yaml
   gitops:
     manifest_projects:
     - id: _classgroup_/_yourpersonalgroup_/world-greetings-env-1
       default_namespace: default
       paths:
       - glob: '/manifests/**/*.yaml'
       reconcile_timeout: 3600s # 1 hour by default
       dry_run_strategy: none # 'none' by default
       prune: true # enabled by default
       prune_timeout: 360s # 1 hour by default
       prune_propagation_policy: foreground # 'foreground' by default
       inventory_policy: must_match # 'must_match' by default
   ````

9. *Click* **Create commit...**

10. *Select* **Commit to master branch** (change from “Create a new branch”)

11. *Click* **Commit**

{{< /admonition >}}

11. Watch the previously opened view of the GitLab Agent log for deployment activity.
    
    <mark class="hlgreen">**For Instructor-Led**: the instructor may have this view displayed for everyone</mark>
    
12. To watch the progress, navigate to ***classgroup/yourpersonalgroup*/world-greetings-env-1**

2. *Click* **Deployments => Environments**

3. **[Automation wait: ~3 min]** Keep refreshing until staging deployment activities complete.

   {{< admonition type=warning title="Warning" open=true >}}
   For all GitOps mode projects, when the deployment shows complete in the Environments page, it only means the manifests are completely setup, the Gitlab Agent for Kubernetes still has to find and deploy the changed manifests
   {{< /admonition >}}

4. **[Automation wait: ~3 min]** Wait after the status shows complete…

5. On the ‘staging’ line, to the right, *Click* **Open**

   > You can see the staging deployed application.

6. In the browser tabs, *Click* **[the tab with the Environments page]**

7. On the ‘production’ line, to the right, *Click* **Open**

   > You can see the production deployed application

8. If there an error indicating there is no site yet, keep refreshing the browser window until the site displays.

{{< admonition type=warning title="Warning" open=true >}}

For all GitOps mode projects, when the deployment shows complete in the Environments page, it only means the manifests are completely setup, the Gitlab Agent for Kubernetes still has to find and deploy the changed manifests. Also note that on the very first time the agent is configured to monitor your manifests - all environments are deployed. From this point forward the manifests will be updated sequentially and will require approval for production.

{{< /admonition >}}

{{< admonition type=danger title="Critical Mindfulness: Only Pull Deployment In Environment Deployment Project" open=true >}}

Subsequent labs will be adding many Runner Based jobs to enable security scanning and dynamic environments. However, the deployment of this application will always be accomplished by a Pull Deployment through the GitLab Agent as you have seen in this lab. You may consult the job log (to see a manifest commit only) and/or the Kubernetes Agent log to verify this.

{{< /admonition >}}

{{< admonition type=success title="Accomplished Outcomes" open=true >}}

1. Configure the Kubernetes Agent to monitor the CI constructed kubernetes manifests.

2. Observe the initial deployment of staging and production via tailing the Kubernetes Agent log and the appearance of the target environments.

   {{< /admonition >}}
