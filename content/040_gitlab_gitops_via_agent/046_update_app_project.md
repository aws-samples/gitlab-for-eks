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

1. Open 'yourpersonalgroup/hello-world’

2. In the left navigation, *Click* **Repository => Files** 

3. On the upper right of the Project page, *Click* **Web IDE**

4. *Navigate to* the file **src/microwebserver.py**

5. Around line 11, *locate* the text `<BODY style="background:lightsalmon">`

6. Change the color after the word `background:` to `lightgreen`

   Result: `<BODY style="background:lightgreen">`

   > If you need a different color, other available color values [are listed here](https://www.w3.org/wiki/CSS/Properties/color/keywords)

7. *Click* **Create commit...**

8. *Select* **Commit to main branch** (change from “Create a new branch”)

9. *Click* **Commit**

10. In the very bottom left, immediately after the text ‘Pipeline’ *Click* **[the pipeline number which is preceeded with a \#]**

11. **[Automation wait: ~2 min]** Wait for the pipeline to complete.

12. *Click* **Packages & Registries => Container Registry**

13. *Click* **[the line ending in ‘/main’]**

14. Search for the latest-prod tag

    > It should have been built moments ago. There should also be a new version tag with the same value for ‘Digest’

15. Open 'yourpersonalgroup/world-greetings-env-1’ project.

16. *Click* **CI/CD => Schedules**

17. On the right side of schedule called ‘CheckForNewContainerVersion’, *Click* **[the play button]**

    > If the schedule is missing, simply Click CI/CD => Pipelines => Run Pipeline = and then => Run Pipeline

18. On the left navigation, *Click* **CI/CD => Pipelines**

19. Open the most recent non-skipped pipeline by clicking **[the pipeline Status badge]** or **[the pipeline #]**

20. Expand the Downstream pipeline - next to the deploy job, *Click* **[the small right arrow]**

21. **[Automation wait: ~3 min]** Wait for the ‘update-staging-manifests’ job should complete successfully.

22. In the pipeline, *Click* **update-staging-manifests**

23. Search the job log (manually or with your brower’s ‘in page search’ feature) for the text “Changes to be committed” (near the bottom)

{{% notice tip%}}

This job only did a commit back to the World Greetings Environment 1 project - it did not do any CD push operations. Since we are also using CI processes in this workshop it can be easy to mistakenly think this job pushed the changes, rather than the GitLab Agent in the cluster pulling them.

{{% /notice %}}

24. In the left navigation, *Click* **Repository => Files**

25. *Click* **manifests**

26. *Click* **hello-world.staging.yaml**

27. *Find* `- image: `

28. Note the version number at the very end of the image string should match the image registry version you just saw. Keep this version in mind so you can compare to production in the next steps .

29. *Click* **[the browers back button]**

30. *Click* **hello-world.production.yaml**

31. *Find* `- image: `

{{% notice tip%}}

The version differences between the current state of these two manifests is what explains the results you will see when viewing the active environments in the next steps.

{{% /notice %}}

32. *Click* **Deployments => Environments**

33. **[Automation wait: ~3 min]** Keep refreshing until staging deployment activities complete.

{{% notice warning%}}

For all GitOps mode projects, when the deployment shows complete in the Environments page, it only means the manifests are completely setup, the GitLab Kubernetes Agent still has to find and deploy the changed manifests.

{{% /notice %}}

{{% notice warning%}}

**If you are in an instructor-led workshop, the instructor may need to access the cluster for you.** If you were to run into unusual deployment problems, you would need to login to the Kubernetes Cluster and run the below command. To do this, login to the EKS Bastion host the same was as was done in “Prep Lab 2.3: Use GitLab K8s Agent to Integrate The Cluster with GitLab” to install the GitLab Agent. Then run this command `kubectl logs -f -l=app=gitlab-agent -n gitlab-agent`

For common errors and more troubleshooting information visit [Troubleshooting the GitLab agent for Kubernetes](https://docs.gitlab.com/ee/user/clusters/agent/troubleshooting.html)

{{% /notice %}}

34. On the ‘staging’ line, to the right, *Click* **Open**

    > You should see that the staging environment is now the new color.

36. To approve the production deployment, in the left navigation, *Click* **CI/CD => Pipelines**

37. Open the most recent non-skipped pipeline by clicking **[the pipeline Status badge]** or **[the pipeline #]**

38. Next to the deploy job, *Click* **[the small right arrow]**

39. On the ‘update-production-manifests’ job, *Click* **[the play button in a circle]**

40. **[Automation wait: ~3 min]** Keep refreshing until production deployment activities complete.

{{% notice warning%}}

For all GitOps mode projects, when the deployment shows complete in the Environments page, it only means the manifests are completely setup, the GitLab Kubernetes Agent still has to find and deploy the changed manifests.

{{% /notice %}}

41. *Click* **Deployments => Environments**

42. **[Automation wait: ~3 min]** Keep refreshing until staging deployment activities complete.

43. On the ‘production’’ line, to the right, *Click* **Open**

    > You should see that the production environment is now the new color.

{{% notice tip%}}

While it is not necessarily easy to observe directly from GitLab, it is the GitLab Agent that is pulling the changes into the cluster. You can understand more about this flow by examining the box ‘GitLab K8s Agent Channel’ in the [GitLab K8s Agent Connections and Flows diagram]({{< relref "../070_architecture_patterns/gitlab-agent-connections" >}}).

{{% /notice %}}
