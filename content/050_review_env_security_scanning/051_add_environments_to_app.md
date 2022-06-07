---
title: "Lab 5.1: Add Environments and Security Scanning to the Application Project (Optional)"
weight: 51
chapter: true
draft: false
description: "Security review apps to the application project"
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

# Lab 5.1: Add Environments and Security Scanning to the Application Project

> **Keyboard Time**: 20 mins, **Automation Wait Time**: 10 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

In this Lab you will update the application project to have a review application and perform security scanning.

1. Open 'yourpersonalgroup/hello-world’

2. Near the top right of the page *Click* **Web IDE** (button)

3. In the left side file browser, *Click* **.gitlab-ci.yml**

   > You will be editing YAML - be careful that tabbing is properly aligned. Only removing the comment character (“#”) should result in proper tabbing.

4. Under `include:` uncomment `- template: Auto-DevOps.gitlab-ci.yml` which should make the section look like this.:
   ```yaml
   include: 
     - local: .gitlab/ci_includes/increment_semver.gitlabci.yml
     - template: Auto-DevOps.gitlab-ci.yml
   ```

5. Under `variables:` uncomment the variables indicated below at the top of the section. There will be additional variables in the section - starting with `NEXTVERSION:` - that must be left as-is.
   ```
   variables:
     BUILD_DISABLED: 'true'
     TEST_DISABLED: 'true'
     POSTGRES_ENABLED: 'false'
     STAGING_ENABLED: 'true'
     
     NEXTVERSION:
      ...
   ```

6. Immediately above `determine-version:`, uncomment the following segment. The keyword `container_scanning` should start completely to the left edge and each level indented 2 spaces.
   ```
   container_scanning:
     variables:
       GIT_STRATEGY: fetch
   ```

7. *Click* **Create commit...**

8. *Select* **Commit to main branch**

9. *Click* **Commit**

{{< admonition type=info title="Info" open=true >}}

These simple steps enable Auto DevOps Dynamic Review Environments and Security Scanning.
{{< /admonition >}}

10. In the left navigation *Click* **Hello World** (The project name banner) 

11. *Click* **CI/CD => Pipelines**

12. Open the most recent non-skipped pipeline by clicking **[the pipeline Status badge]** or **[the pipeline #]**

13. **[Automation wait: ~7 min]** Wait for the GitLab deployment to staging to complete successfully.

14. *Click* **Deployments => Environments**

15. On the ‘staging’ line, to the right, *Click* **Open**

    > If SSL is not yet resolving, click the “advanced” button and open the site anyway.

16. In the browser tabs, *Click* **[the tab with environments]**

17. Return to the same pipeline (Browser Back Control may take you there)

18. For the job ‘production_manual’, *Click* **[the Play button]**

{{< admonition type=info title="Info" open=true >}}

The environments in the Application Build Project are built using CD Push through the GitLab Agent and are only for testing and qualifying the Application Container. The environment called ‘Production’ simply means that the container is “production ready” if it passes all tests and gets into that review environment. The ‘promote-image-to-latest’ job is what actually signals downstream Environment Deployment Projects that a new version has been published. The DevOps methodology and processes you use should determine how much testing and qualification is done in this Application Build Project and how much is done in Environment Deployment Projects. There could be a lot of testing in both if downstream environments want to double check or have additional checks before deploying to actual production endpoints.

{{< /admonition >}}

19. **[Automation wait: ~3 min]** Wait for the browser_performance job to complete successfully and the Play button to appear on the ‘promote-image-to-latest’ job. 

20. **IMPORTANT:** For the job ‘promote-image-to-latest’ *Click* **[the Play button]**

21. *Click* **Packages & Registries => Container Registry**

22. *Click* **[the line ending in ‘/main’]**

23. Search for the latest-prod tag

    > It should have been built moments ago. There should also be a new version tag with the same value for ‘Digest’

{{< admonition type=warning title="Warning" open=true >}}

This last step is what updates the image tags so that the version that was just tested is marked as ready for Environment Deployment projects to consume.

{{< /admonition >}}
