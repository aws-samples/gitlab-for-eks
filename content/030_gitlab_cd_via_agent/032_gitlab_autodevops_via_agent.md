---
title: "Lab 3.2: GitLab Auto DevOps and CD via the K8s Agent"
weight: 32
chapter: true
draft: false
description: "See Auto DevOps in action. The same configuration enables GitLab CD using helm and kubectl commands."
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

# Lab 3.2: GitLab Auto DevOps via the K8s Agents

> **Keyboard Time**: 15 mins, **Automation Wait Time**: 15 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

{{% notice tip %}}
This configuration also works for any kind of GitLab CD to the cluster using Helm and kubectl commands, not only Auto DevOps.
{{% /notice %}}

## Configure An Auto DevOps Project

{{% notice warning %}}Before continuning make sure to use DNSChecker.com to check if both <mark>the Load Balancer DNS Name</mark> and <mark>\<the Load Balancer IP>.nip.io</mark> have propagated through global DNS and wait (or troubleshoot) if they have not.
{{% /notice %}}

1. While in 'yourpersonalgroup' *Click* **New project** (button) and then *Click* **Import project**

2. On the 'Import project' page, *Click* **Repository by URL**

3. On the next page, for 'Git repository URL' *Paste* **https://gitlab.com/guided-explorations/gl-k8s-agent/gl-ci/simply-simple-notes.git**

4. In 'Project name' *Type* **yourgitlabusername DevOps Security Scanning**

5. Near the bottom of the page *Click* **Create project** (button)

6. When the import is complete

7. *Click* **Settings => CI/CD**

8. Next to 'Variables' *Click* **Expand**

9. *Click* **Add variable** once for each table row

    | Key              | Value | Protect | Mask |
    | ---------------- | ----- | ------- | ---- |
    | POSTGRES_ENABLED | false | No      | No   |
    | TEST_DISABLED    | 1     | No      | No   |

10. *Click* **Settings => CI/CD**

11. Next to ‘Auto DevOps’, *Click* **Expand**

12. Under 'Auto DevOps' *Check* **Default to Auto DevOps pipeline**

13. Leave 'Deployment strategy' at the default and *Click* **Save changes**

14. On the left navigation *Click* **CI/CD => Pipelines**

15. Only if a pipeline is not already running:

      1. On the upper right of the page *Click* **Run pipeline**

      2. On the 'Run pipeline' page, *Click* **Run pipeline**

16. Watch the pipeline progress by clicking the linked number starting with \# under the ‘Pipeline’ column.

17. To explore the various pipeline jobs by clicking their status icon.

18. To return to the pipelines view, on the left navigation bar, *Click* **CI/CD => Pipelines**

19. **[Automation wait: 15 mins]** wait for the pipeline to complete the ‘production’ job

20. On the left navigation *Click* **Deployments => Environments**

21. To see the environment deployment status, to the left of 'production' *Click* **[the small right arrow]**

22. To the right of 'production' *Click* **Open** (button)

     > It can take a while for SSL to register, you can click through the advanced button to see the site if SSL is not working yet.

23. If everything worked as expected, you should see an application page called Simply Simple Notes and should not have any warnings or problems with SSL certificates.