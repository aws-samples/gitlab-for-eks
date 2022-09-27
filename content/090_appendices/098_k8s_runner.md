---
title: "Install a Kubernetes Runner"
weight: 98
chapter: true
draft: false
description: "Installing a runner into the EKS cluster."
---

## Using Cluster Management Project to Install a Kubernetes Runner

> **Scenarios:** Instructor-Led, Self-Paced
>
> A Kubernetes GitLab Runner Registration does not survive scaling the cluster to zero. If you scale to zero you will need to redeploy the chart to register another runner and you will need to remove orphaned runner registrations in GitLab from each scale to zero event.

This example deploys a new runner in order to be free of runner minute limits on GitLab.com SaaS Runners and to be free on Credit Card registration requirements for participants to use free minutes.
You may use a GitLab.com shared runner to configuration Cluster Applications because cluster access is achieved through the installed agent, rather than a traditional Kubernetes control plane API access (like was formerly used by the GitLab Certificate connection method.)

{{< admonition type=warning title="Warning" open=true >}}

Since you will have needed to have access to a runner of some type to deploy the cluster managed apps for ingress and 

{{< /admonition >}}

**IMPORTANT:** This setup is assuming that you wish to setup Auto DevOps for the entire downbound group heirarchy 'classgroup' - this can be scoped tighter by paying attention to fully specifying group paths and by creating Group level CI/CD variables at a deeper group level.

This section assumes you have a shared runner attached to your group or project and that it has minutes available for usage.

1. In 'classgroup', *Click* **CI/CD > Runners**

2. Near the top right, *Click* **Register a group runner** (button)

3. Next to the ‘Registration token’, *Click* **[the Clipboard Icon]**

4. In 'classgroup/cluster-management' *Click* **Settings > CI/CD**

5. Next to 'Variables' *Click* **Expand**

6. *Click* **Add variable** once for each table row

   | Key                              | Value                                                | Protect | Mask |
   | -------------------------------- | ---------------------------------------------------- | ------- | ---- |
   | GITLAB_RUNNER_REGISTRATION_TOKEN | <mark>paste the Runner Registration Token copied earlier</mark> | No      | Yes  |

7. In 'classgroup/cluster-management' *Start* the **Web IDE**.

8. Edit the file 'helmfile.yaml' and uncomment the line <mark>- path: applications/gitlab-runner/helmfile.yaml</mark>

9. Edit the file 'applications/gitlab-runner/values.yaml.gotmpl' and

10. Uncomment the line <mark>runUntagged: true</mark>

11. *Click* **Create commit...**

12. *Select* **Commit to master branch**

13. *Click* **Commit**

14. Monitor the CI job that your commit triggered for successful completion.

Note: GITLAB_RUNNER_REGISTRATION_TOKEN variable could be removed now - but would need to be recreated to reinstall the runner.

**Optional Disable Other Runners**

If you'd like to see your Kubernetes runner in action, follow this procedure.

### Runner Cleanup

> If you had deployed a runner there may be one or more Runner Registrations associated with the EKS cluster that is now gone.

1. In GitLab, within 'classgroup' *Click* **Settings => Ci/CD**
2. Under 'Available runners' identify the rows that have the IP address of your load balancer and **delete them**.

