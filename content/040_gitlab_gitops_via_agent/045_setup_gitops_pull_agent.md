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

{{< admonition type=abstract title="Target Outcomes" open=true >}}

1. Configure the Kubernetes Agent to monitor the CI constructed kubernetes manifests.

2. Observe the initial deployment of staging and production.

   {{< /admonition >}}

{{< admonition type=warning title="Warning" open=true >}}

**Check With The Instructor**: If you are in an Instructor-Led course where multiple participants are sharing the same GitLab group and Kubernetes cluster, it is important that all changes to the agent configuration file are retained.  The instructor may elect to make these changes for each participant to avoid file saving conflicts and/or information overwriting. Using Merge Requests would be likely to generate many merge conflicts and slow the class progress signficantly. **If you are not in an instructor-led course, perform the lab as described.**

{{< /admonition >}}

1. In a web browser *Navigate to* **classgroup/cluster-management**

2. Near the upper right of the page, *Click* **Web IDE** (button)

3. Navigate to the file .gitlab/agents/spotazuseast2-agent/config.yml

4. Add the following to the file only once:

   ```yaml
   gitops:
     manifest_projects:
   
   ```

5. Under “gitops:manifest_projects:” add as below - replacing `_classgroup_` and `_yourpersonalgroup_` with the actual names for your project. Ensure indenting and “gitops:manifest_projects” should only appear once in the entire file.
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

6. *Click* **Create commit...**

7. *Select* **Commit to master branch** (change from “Create a new branch”)

8. *Click* **Commit**

9. To watch the progress, navigate to ***classgroup/yourpersonalgroup*/world-greetings-env-1**

10. *Click* **Deployments => Environments**

11. **[Automation wait: ~3 min]** Keep refreshing until staging deployment activities complete.

    {{< admonition type=warning title="Warning" open=true >}}
For all GitOps mode projects, when the deployment shows complete in the Environments page, it only means the manifests are completely setup, the GitLab Kubernetes Agent still has to find and deploy the changed manifests
    {{< /admonition >}}

12. **[Automation wait: ~3 min]** Wait after the status shows complete…

13. On the ‘staging’ line, to the right, *Click* **Open**

    > You can see the staging deployed application.

14. In the browser tabs, *Click* **[the tab with the Environments page]**

15. On the ‘production’ line, to the right, *Click* **Open**

    > You can see the production deployed application

16. If there an error indicating there is no site yet, keep refreshing the browser window until the site displays.

{{< admonition type=warning title="Warning" open=true >}}

For all GitOps mode projects, when the deployment shows complete in the Environments page, it only means the manifests are completely setup, the GitLab Kubernetes Agent still has to find and deploy the changed manifests. Also note that on the very first time the agent is configured to monitor your manifests - all environments are deployed. From this point forward the manifests will be updated sequentially and will require approval for production.

{{< /admonition >}}

{{< admonition type=success title="Accomplished Outcomes" open=true >}}

1. Configure the Kubernetes Agent to monitor the CI constructed kubernetes manifests.

2. Observe the initial deployment of staging and production.

   {{< /admonition >}}
