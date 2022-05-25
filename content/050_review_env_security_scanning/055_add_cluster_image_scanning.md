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

{{% notice tip%}}

Cluster image scanning is able to add security findings for images used in your cluster that are not a part of your development process and therefore do not get routinely scanned during CI of the application. This scanning currently focuses on the cluster’s images and not other aspects of cluster security. The other security scanning we’ve been configuring has explictly to do with development - this scanning is part of GitLab’s Protect stage - part of operational integrity that can be enabled via the GitLab Agent.

{{% /notice %}}

{{% notice warning%}}

**Check With The Instructor**: If you are in an Instructor-Led course where multiple participants are sharing the same GitLab group and Kubernetes cluster, the instructor may elect to make these changes. **If you are not in an instructor-led course, perform the lab as described.**

{{% /notice %}}

1. Open 'classgroup/cluster-management’

2. In the left navigation, *Click* **Repository => Files** 

3. On the upper right of the Project page, *Click* **Web IDE**

4. Navigate to the file .gitlab/agents/spotazuseast2-agent/config.yml

5. Add the following to bottom of the file (only once per agent config.toml file).:

   ```yaml
   starboard:
     cadence: '* * * * *' #Every 5 minutes
   ```

6. *Click* **Create commit...**

7. *Select* **Commit to main branch**

8. Under ‘Commit Message’, *Type* **[skip ci] Adding Manfest Security Scanning**

9. *Click* **Commit**

   > Using the following command on the machine with kubectl cluster access will show the scanning in progress:  `kubectl logs -f -l=app=gitlab-agent -n gitlab-agent`

10. **[Automation Wait Time: ~10 mins]** Wait for the cluster to receive the new directive and perform a scan.

11. To see scanning results, while in 'classgroup/cluster-management’

12. *Click* **Infrastructure => Kubernetes clusters => spot2azuseast2-agent1 => Security** (Tab)

13. Under ‘Status’ *Click* **[to expand the drop down]** and then *Click* **All statuses**

14. These findings are also visible in the standard security dashboards.

15. Open ‘classgroup’

16. On the left navigation, *Click* **Security & Compliance => Vulnerability Report**

17. In the tab bar under ‘Vulnerability report’, *Click* **Operational vulnerabilities**

18. Under ‘Status’ *Click* **[to expand the drop down]** and then *Click* **All statuses**

    > Notice the list of vulnerabilities.

{{% notice warning%}}

To speed up the class results we set the cluster scanner to every minute. If this is a long lived cluster it would be prudent to update the starboard:cadence above to once a day or less.

{{% /notice %}}
