---
title: "Lab 4.4: Link and Test Projects"
weight: 44
chapter: true
draft: false
description: "See GitLab GitOps pull deployment and configuration management in action."
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

# Lab 4.4: Link and Test Projects

> **Keyboard Time**: 5 mins, **Automation Wait Time**: 3 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

{{< admonition type=abstract title="Target Outcomes" open=true >}}

1. Link the projects using a scheduled pipeline.

   {{< /admonition >}}

{{< admonition type=tip title="Tip" open=true >}}

These methods of linking the projects are loosely coupled. The benefits of this approach are described in [Loose Project Coupling](https://gitlab.com/groups/guided-explorations/gl-k8s-agent/gitops/-/wikis/home#loose-project-coupling) 
{{< /admonition >}}

### Scheduled Pipeline Model

1. In 'yourpersonalgroup/world-greetings-env-1’ *Click* **CI/CD => Schedules**

2. On the upper right of the page, *Click* **New schedule** (button)

3. Under Description *Type* **CheckForNewContainerVersion**

4. Leave all other items at their defaults.

5. Near the bottom left of the page, *Click* **Save pipeline schedule**

   > Normally you would take time to create one or more schedules specific to your desired frequency.

6. On the right of CheckForNewContainerVersion *Click* **[the play button]**

7. On the left navigation, *Click* **CI/CD => Pipelines**

8. On the latest pipeline *Click* **[the Status badge]** or **[the pipeline \#]**

9. Wait for the pipeline to complete.

10. If you did not perform any extra builds on the Application Project, the “deploy” job will have a failed status and the pipeline will have a status of “Passed”

   {{< admonition type=warning title="Failed 'deploy' job is OK" open=true >}}
The deploy job status is 'failed' because no child pipeline jobs are scheduled because there has not been a new container published since the last run (and no changes were made to the Environment Deployment Project package manifests). GitLab considers it a failure when a parent pipeline fails to create a child pipeline, but we’ve marked this job “allowed_to_fail” which gives the Pipeline status of "Passed" (because it is the most efficient way to only run the manifest builds when there is actually a change we care about.)
   {{< /admonition >}}

{{< admonition type=success title="Accomplished Outcomes" open=true >}}

1. Link the projects using a scheduled pipeline.

{{< /admonition >}}

{{< admonition type=warning title="Instructor Led Classroom" open=true >}}

If you are in an Instructor-Led course, you usually not be given time to complete the below **[Extra Credit]** exercises. Please check with the instructor before attempting them.

{{< /admonition >}}

### [Extra Credit] Pipeline Subscription Model

{{< admonition type=warning title="Warning" open=true >}}

**If you are in an instructor-led workshop, please ask the instructor before performing this lab** as it could affect workshop timing or the stability of additional assigned labs. All your upbound groups and the hello-world must be public for pipeline subscriptions to work.
{{< /admonition >}}

> Pipeline subscriptions allow an Environment Deployment Project to trigger nearly immediately after the Application Project completes a build.

1. Open 'yourpersonalgroup/hello-world’

2. In the browser URL bar, copy the URL path without the domain. For example if the browser url is https://gitlab.com/group1/myuser/hello-world you would copy ‘group1/myuser/hello-world ’

3. **In a NEW browser tab**, open 'yourpersonalgroup/world-greetings-env-1’

4. *Click* **Settings => CI/CD**

5. Next to Pipeline subscriptions, *Click* **Expand**

6. Under Project path, *Paste* **[the path copied from the hello-world project]**

7. *Click* **Subscribe**

8. Next to Pipeline subscriptions, *Click* **Expand**

### [Extra Credit] Run Pipeline With NEXTVERSIONTOUSE Variable To Specify Version

{{< admonition type=warning title="Warning" open=true >}}

**If you are in an instructor-led workshop, please ask the instructor before performing this lab** as it could affect workshop timing or the stability of additional assigned labs. If used in production this method would not be paired with any auto-update mechanism above because that mechanism would dynamically install the latest.
{{< /admonition >}}

> This method can also be used to roll back an environment.

1. Open 'yourpersonalgroup/hello-world’

2. On the left navigation, *Click* **CI/CD => Pipelines**

3. In the upper right of the page, *Click* **Run pipeline** (button)

4. Under Variables, *Type* **NEXTVERSIONTOUSE** over ‘Input variable key’

5. On the same line, *Type* **0.0.1** over ‘Input variable value’

6. In  “NEXTVERSION” to **1.0.0**
7. In the lower left of the page *Click* **Run pipeline** (button)
8. Wait for the pipeline to complete successfully.
9. **In a NEW browser tab**, open 'yourpersonalgroup/world-greetings-env-1' in the Web IDE.

10. In the files list on the left *Click* **manifests > hello-world.staging.yaml**

11. *Search* for **- image:**

12. The image reference should point to the version “0.0.1”
13. In the left navigation *Click* **CI/CD => Pipelines**
14. Find the last non-skipped pipeline and *Click* it’s **[Status badge]** or **[Pipeline \#]** to open the pipeline.
15. Expand the Downstream pipeline with the great than arrow (`>`).
16. Next to the update-production-manifests job, *Click* **[the play button]**
17. Wait until the update-production-manifests job has a green check next to it.
18. *Switch* back to **[the Web IDE tab]**
19. *Click* **[the browser refresh button]**
20. In the files list on the left *Click* **manifests > hello-world.production.yaml**
21. *Search* for **- image:**
22. The image reference and version tag should match the staging manifest (“0.0.1”)

### [Extra Credit] Create an MR with NEXTVERSIONTOUSE File To Specify Version

{{< admonition type=warning title="Warning" open=true >}}

**If you are in an instructor-led workshop, please ask the instructor before performing this lab** as it could affect workshop timing or the stability of additional assigned labs. This section is just to let you know that you can create a Merge Request that creates or updates a file called NEXTVERSIONTOUSE that only contains the desired version on the first and only line in the file. This enables MR review by as many people as necessary to gather approvals before environment deployments are performed. If you have previously done MRs in GitLab, you could do this procedure to experience an MR approval based workflow in an Environment Deployment Project. 
{{< /admonition >}}
