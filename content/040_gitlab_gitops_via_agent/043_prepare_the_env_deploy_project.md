---
title: "Lab 4.3: Prepare the Environment Deployment Project"
weight: 43
chapter: true
draft: false
description: "See GitLab GitOps pull deployment and configuration management in action."
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

# Lab 4.3: Prepare the Environment Deployment Project

> **Keyboard Time**: 25 mins, **Automation Wait Time**: 5 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

{{%expand "Click Here To Expand a Visual Overview of The GitLab Environment Deployment Project Pipeline" %}}![GitOps_Part_2_One_or_More_Env_Deploy_Only_Repos2](GitOps_Part_2_One_or_More_Env_Deploy_Only_Repos2.png)

> You will be using #2 and #3 in labs.

{{% /expand%}}

1. While in 'yourpersonalgroup' (created in a prior lab) *Click* **New project** (button) and then *Click* **Import project**

2. On the 'Import project' page, *Click* **Repo by URL**

3. On the next page, for 'Git repository URL' *Paste* **https://gitlab.com/guided-explorations/gl-k8s-agent/gitops/envs/world-greetings-env-1.git**

4. In 'Project name' *Type* **World Greetings Env 1** (likely already be defaulted to this)

5. Near the bottom of the page *Click* **Create project** (button)

6. When the import is complete, you will be placed in the default landing page of the project.

7. While in 'yourpersonalgroup', *Click* **Settings => General**

8. Next to ‘Visibility, project features, permissions’, *Click* **Expand**

9. Under Project visibility, *Select* **Public**

10. *Deselect* **Users can request access**

    > The project must be made public because we’ll be using a GitLab Kubernetes Agent registered outside of our project.

11. Scroll down to locate and *Click* **Save changes**

    > For the next steps you will create a Personal Access Token so that the CI job can commit the kubernetes manifests back to its own repository. For production, if you have licensed GitLab a project level ‘Access Token’ is a better way to ensure that the automation credentials do not depend on regular user credentials.

12. In the upper right of the page *Click* **[the Avatar icon]** and then *Click* **Edit profile**

13. On the left naviagion, *Click* **Access Tokens**

14. Under ‘Add a personal access token’, for Token name, *Type* **WriteRepository**

15. Under ‘Select scopes’

    1. *Select* **read_repository**
    2. *Select* **write_repository**

16. *Click* **Create personal access token** 

17. Under ‘Your new personal access token’, to the right of the token value, *Click* **[the Clipboard Icon]**

18. **In a NEW browser tab**, open the project 'yourpersonalgroup/world-greetings-env-1' again (this time we are at the PROJECT level).

19. On the left navigation, *Click* **Settings => CI/CD**

20. To the right of ‘Variables’, *Click* **Expand**

21. *Click* **Add variable**

22. For Key, *Type* **PROJECT_COMMIT_TOKEN**

23. In the Value field *Paste* **[the Clipboard contents]**

24. Under Flags, *Deselect* **Protect variable**

25. Under Flags, *Select* **Mask variable**

26. *Click* **Add variable**

27. To add another variable, *Click* **Add variable**

28. For Key, *Type* **PROJECT_COMMIT_USER**

29. In the Value field *Type* **[your_gitlab_user_name]**

30. Under Flags, *Deselect* **Protect variable**

31. *Click* **Add variable**

    > You should now have two variables in the 'yourpersonalgroup/world-greetings-env-1' project that contains PROJECT_COMMIT_TOKEN and PROJECT_COMMIT_USER.
    >
    > These permissions are least privilege, in part, because the CI/CD Variables are only published at the project level.

32. In the left navigation, *Click* **Repository => Files** 

33. On the upper right of the Project page, *Click* **Web IDE**

34. In a new browser tab, open your ‘yourpersonalgroup/hello-world’ Project. (not the same project you are in now)

35. On the left navigation panel, *Click* **Packages & Registries => Container Registry**

36. Next to the line item ending in “/main”, *Click* **[the Clipboard icon]**

37. In your browser tabs, *Switch* back to the **[tab where you have the Web IDE opened on the World Greetings Env 1 project]**.

38. In the files list, *Click* **.gitlab-ci.yaml**

39. Under ‘variables:’ *Find* **IMAGE_NAME_TO_MONITOR**

40. In the quoted value, *Remove* **everything except ${SERVICE_NAME}/main** 

41. Paste your copied text at the start of the value and ensure it ends in /${SERVICE_NAME}/main

42. *Remove* **hello-world/main** from the pasted string.

    > The result should be something like **IMAGE_NAME_TO_MONITOR: "registry.gitlab.com/guided-explorations/gl-k8s-agent/gitops/apps/${SERVICE_NAME}/main"** but ‘registry.gitlab.com/guided-explorations/gl-k8s-agent/gitops/apps’ will be the registry path to your specific Application Project’s registry.

43. Ensure that the value **SERVICE_NAME: "hello-world"**  matches the actual path name of your Application Project (it should already match if you used the default when importing the Application Project)

44. *Click* **Create commit...**

45. *Select* **Commit to main branch** (change from “Create a new branch”)

46. *Click* **Commit**

47. In the very bottom left, immediately after the text ‘Pipeline’ *Click* **[the pipeline number which is preceeded with a \#]** (Or on the left navigation *Click* **CI/CD => Pipelines** and *Click* **[the status badge]** or [pipeline #] for the latest running pipeline)

48. Expand the Downstream pipeline with the great than arrow (`>`).

49. **[Automation wait: ~3 min]** Watch the pipeline complete through the ‘staging’ job.

50. The update-staging-manifests job should complete successfully.

51. To get back to the Web IDE, *Click* **[the browser back button]**

52. *Click* **[the browser refresh button]**

53. In the files list on the left *Click* **manifests > hello-world.staging.yaml**

54. *Search* for **- image:**

55. The image reference should be the registry pointer to your Application Project, followed by the version “1.0.0” (if you only built the Application Project twice) 

    > Be sure you refreshed the browser

56. In the files list on the left *Click* **manifests > hello-world.production.yaml**

57. *Search* for **- image:**

58. The image reference should match the old location you copied the Application Project from, followed by a version that most likely is not the same as your staging image.

59. **In a NEW browser tab**, open 'yourpersonalgroup/world-greetings-env-1' again.

    > Shortcut - right click the project heading in the left navigation and *Click* **Open Link in New Tab**) 

60. In the left navigation *Click* **CI/CD => Pipelines**

61. Find the last non-skipped pipeline and *Click* it’s **[Status badge]** or **[Pipeline \#]** to open the pipeline.

62. Expand the Downstream pipeline with the great than arrow (`>`).

63. Next to the update-production-manifests job, *Click* **[the play button]**

64. **[Automation wait: ~1 min]** Wait until the update-production-manifests job has a green check next to it.

65. *Switch* back to **[the Web IDE tab]**

66. *Click* **[the browser refresh button]**

67. In the files list on the left *Click* **manifests > hello-world.production.yaml**

68. *Search* for **- image:**

69. The image reference and version tag should match the staging manifest (hello-world.staging.yaml)

{{% notice tip %}}

The manifests are not yet monitored by the GitLab Agent, but once they are, the action of updating them in the project is all that is necessary for the GitLab Agent to find them and update the Kubernetes Cluster to match the manifest.
{{% /notice %}}

