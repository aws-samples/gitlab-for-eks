---
title: "Lab 4.6: Update the Application Build Project and Deploy to Production"
weight: 46
chapter: true
draft: false
description: "See GitLab GitOps pull deployment and configuration management in action."
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

# Lab 4.6: Update the Application Build Project and Deploy to Production

> **Keyboard Time**: 20 mins, **Automation Wait Time**: 5 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

In this Lab you will update the background color of the application and track the progress of the automation through both repositories and both environments.

{{< admonition type=abstract title="Target Outcomes" open=true >}}

Observe an end-to-end application change:

1. Update the background color of the application

2. Track the progress of the automation through the Application Build project and

3. Through both environments of the Environment Deployment project using the background page color.

   {{< /admonition >}}

1. Open 'yourpersonalgroup/hello-world’

5. In the left navigation, *Click* **Repository => Files** 

6. On the upper right of the Project page, *Click* **Web IDE**

7. *Navigate to* the file **src/microwebserver.py**

8. Around line 11, *locate* the text `<BODY style="background:lightsalmon">`

9. Change the color after the word `background:` to `lightgreen`

   Result: `<BODY style="background:lightgreen">`

   > If you need a different color, other available color values [are listed here](https://www.w3.org/wiki/CSS/Properties/color/keywords)

10. *Click* **Create commit...**

11. *Select* **Commit to main branch** (not selected by default)

12. *Click* **Commit**

13. In the very bottom left, immediately after the text ‘Pipeline’ *Click* **[the pipeline number which is preceeded with a \#]**

14. **[Automation wait: ~2 min]** Wait for the pipeline to complete.

15. *Click* **Packages & Registries => Container Registry**

16. *Click* **[the line ending in ‘/main’]**

17. Scan for the latest-prod tag

    > It should have been built moments ago. There should also be a new version tag with the same value for ‘Digest’

    {{< admonition type=success title="Accomplished Outcome" open=true >}}
You just observed the automatic creation of a new production ready container based on the normal development activity of changing the application files.
    {{< /admonition >}}

20. Open 'yourpersonalgroup/world-greetings-env-1’ project.

21. *Click* **CI/CD => Schedules**

22. On the right side of schedule called ‘CheckForNewContainerVersion’, *Click* **[the play button]**

    > If the schedule is missing, simply Click CI/CD => Pipelines => Run Pipeline = and then => Run Pipeline

24. On the left navigation, *Click* **CI/CD => Pipelines**

25. Open the most recent non-skipped pipeline by clicking **[the pipeline Status badge]** or **[the pipeline #]**

26. Expand the Downstream pipeline - next to the deploy job, *Click* **[the small right arrow]**

27. **[Automation wait: ~3 min]** Wait for the ‘update-staging-manifests’ job to complete successfully.

28. In the pipeline, *Click* **update-staging-manifests**

29. Search the job log (manually or with your brower’s ‘in page search’ feature) for the text “Changes to be committed” (near the bottom)

    {{< admonition type=success title="Observation" open=true >}}
This job only did a commit back to the World Greetings Environment 1 project - it did not do any CD push operations. Since we are also using CI processes in this workshop it can be easy to mistakenly think this job pushed the changes, rather than the GitLab Agent in the cluster pulling them.
    {{< /admonition >}}

30. In the left navigation, *Click* **Repository => Files**

31. In the main page body, in the files and directories list, *Click* **manifests**

32. *Click* **hello-world.staging.yaml**

33. *Find* `- image: `

34. Note the version number at the very end of the image string should match the image registry version you just saw. Keep this version in mind so you can compare to production in the next steps .

35. *Click* **[the browers back button]**

36. *Click* **hello-world.production.yaml**

37. *Find* `- image: `

    {{< admonition type=tip title="Tip" open=true >}}
The version differences between the current state of these two manifests is what explains the results you will see when viewing the active environments in the next steps.
    {{< /admonition >}}

38. *Click* **Deployments => Environments**

39. **[Automation wait: ~3 min]** Keep refreshing until staging deployment activities complete.

    {{< admonition type=warning title="Warning" open=true >}}
For all GitOps mode projects, when the deployment shows complete in the Environments page, it only means the manifests are completely setup, the Gitlab Agent for Kubernetes still has to find and deploy the changed manifests.
    {{< /admonition >}}

    {{< admonition type=warning title="Warning" open=true >}}
**If you are in an instructor-led workshop, the instructor may need to access the cluster for you.** If you were to run into unusual deployment problems, you would need to login to the Kubernetes Cluster and run the below command. To do this, login to the EKS Bastion host the same was as was done in “Prep Lab 2.3: Use GitLab K8s Agent to Integrate The Cluster with GitLab” to install the GitLab Agent. Then run this command `kubectl logs -f -l=app=gitlab-agent -n gitlab-agent`

For common errors and more troubleshooting information visit [Troubleshooting the GitLab agent for Kubernetes](https://docs.gitlab.com/ee/user/clusters/agent/troubleshooting.html)
    {{< /admonition >}}

34. On the ‘staging’ line, to the right, *Click* **Open**

    > You should see that the staging environment is now the new color.

36. To approve the production deployment, in the left navigation, *Click* **CI/CD => Pipelines**

37. Open the most recent non-skipped pipeline by clicking **[the pipeline Status badge]** or **[the pipeline #]**

38. Next to the deploy job, *Click* **[the small right arrow]**

39. *Locate* the **update-production-manifests** job
> You may have to horizontally scroll right to see this final job.

40. *Click* **[the play button in a circle]**
41. **[Automation wait: ~3 min]** Keep refreshing until production deployment activities complete.

    {{< admonition type=warning title="Warning" open=true >}}
For all GitOps mode projects, when the deployment shows complete in the Environments page, it only means the manifests are completely setup, the Gitlab Agent for Kubernetes still has to find and deploy the changed manifests.
    {{< /admonition >}}

41. *Click* **Deployments => Environments**

42. **[Automation wait: ~3 min]** Keep refreshing until staging deployment activities complete.

43. On the ‘production’’ line, to the right, *Click* **Open**

    {{< admonition type=success title="Observation" open=true >}}
    
    You should see that the production environment is now the new color.
    
    {{< /admonition >}}

{{< admonition type=tip title="Tip" open=true >}}

While it is not necessarily easy to observe directly from GitLab, it is the GitLab Agent that is pulling the changes into the cluster. You can understand more about this flow by examining the box ‘GitLab K8s Agent Channel’ in the [GitLab K8s Agent Connections and Flows diagram]({{< relref "../070_architecture_patterns/gitlab-agent-connections" >}}).

{{< /admonition >}}

{{< admonition type=success title="Accomplished Outcomes" open=true >}}

1. Update the background color of the application

2. Track the progress of the automation through the Application Build project and

3. Through both environments of the Environment Deployment project using the background page color.

   {{< /admonition >}}
