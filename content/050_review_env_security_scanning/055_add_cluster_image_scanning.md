---
title: "Lab 5.5: Add Cluster Image Security Scanning (Optional)"
weight: 55
chapter: true
draft: false
description: "Cluster image security scanning configuration."
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

# Lab 5.5: Add Cluster Image Security Scanning (Optional)

> **Keyboard Time**: 5 mins, **Automation Wait Time**: 10 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

{{< admonition type=gitlab title="Scanning Other Images In The Cluster" open=true >}}

Cluster image scanning is able to add security findings for images used in your cluster that are not a part of your development process and therefore do not get routinely scanned during CI of the application. This scanning currently focuses on the cluster’s images and not other aspects of cluster security. The other security scanning we’ve been configuring has explictly to do with development - this scanning is part of GitLab’s Protect stage - part of operational integrity that can be enabled via the GitLab Agent.

{{< /admonition >}}

{{< admonition type=abstract title="Target Outcomes" open=true >}}

1. Configure cluster image scanning using the GitLab Agent for Kubernetes.
2. Examine cluster image scanning findings.

{{< /admonition >}}

{{< admonition type=bug title="Troubleshooting: Cluster Image Scanning" open=true >}}

[Troubleshooting Guide: Cluster Image Scanning]({{< relref "../010_introduction/tuning_and_troubleshooting.md#cluster-or-agent-gets-in-an-uncertain-state" >}})

{{< /admonition >}}

{{< admonition type=pick title="Done By Instructor for Instructor-Led Courses" open=true >}}

1. Logon the cluster administration machine => [Instructions for SSM Session Manager for EKS]({{< relref "../010_introduction/tuning_and_troubleshooting.md#using-the-eks-bastion-for-cluster-administration-with-kubectl-and-helm" >}})

{{< admonition type=warning title="Temporary Fix" open=true >}}

Until [361792](https://gitlab.com/gitlab-org/gitlab/-/issues/361972) is resolved, you will need to run this command in the cluster:

   `kubectl create serviceaccount gitlab-agent -n gitlab-agent`

{{< /admonition >}}

2. Run the following command to tail the kubernetes agent log while deployments are happening:

   `kubectl logs -f -l=app=gitlab-agent -n gitlab-agent`

    <mark class="hlgreen">Leave this view open as you will be instructed to consult it to see the deployment logging activity when the GItLab Agent pulls and processes the kubernetes manifest.</mark>

4. Open 'classgroup/cluster-management’

5. In the left navigation, *Click* **Repository => Files** 

6. On the upper right of the Project page, *Click* **Web IDE**

7. Navigate to the file .gitlab/agents/spotazuseast2-agent/config.yml

8. Look at the minutes past the hour of the current time.

9. Add 5 minutes and insert the following snippet - substitute your minutes number for ‘55’ in the below:

   ```yaml
   starboard:
     cadence: '55 * * * *' #Every hour at 55 minutes past the hour
   ```

   {{< admonition type=warning title="Do not set to a low frequency like every minute" open=true >}}

   If the cluster scanning job launches multiple simultaneous instances, it is more likely to get in a bad state.

   {{< /admonition >}}

10. *Click* **Create commit...**

11. *Select* **Commit to master branch**

12. Under ‘Commit Message’, *Type* **[skip ci] Adding Manfest Security Scanning**

13. *Click* **Commit**

    > The time can be updated to retrigger the agent if there are problems getting it to run.

{{< /admonition >}}

12. Watch the previously opened view of the GitLab Agent log for deployment activity.

   <mark class="hlgreen">**For Instructor-Led**: the instructor may have this view displayed for everyone</mark>

13. **[Automation Wait Time: ~5 mins]** Wait for the cluster to receive the new directive and perform a scan.

2. To see scanning results, while in 'classgroup/cluster-management’

3. *Click* **Infrastructure => Kubernetes clusters => spot2azuseast2-agent1 => Security** (Tab)

4. Under ‘Status’ *Click* **[to expand the drop down]** and then *Click* **All statuses**

5. These findings are also visible in the standard security dashboards.

6. Open ‘classgroup’

7. On the left navigation, *Click* **Security & Compliance => Vulnerability Report**

8. In the tab bar under ‘Vulnerability report’, *Click* **Operational vulnerabilities**

9. Under ‘Status’ *Click* **[to expand the drop down]** and then *Click* **All statuses**

   > Notice the list of vulnerabilities.

{{< admonition type=abstract title="Accomplished Outcomes" open=true >}}

1. Configure cluster image scanning using the GitLab Agent for Kubernetes.
2. Examine cluster image scanning findings.

{{< /admonition >}}

{{< admonition type=warning title="Warning" open=true >}}

To speed up the class results we set the cluster scanner to every minute. If this is a long lived cluster it would be prudent to update the starboard:cadence above to once a day or less.

{{< /admonition >}}to
