---
title: "Lab 5.1: Add Environments and Security Scanning to the Application Project"
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

{{< admonition type=gitlab title="GitLab Security Scanning and Automated Review Environments" open=true >}}

GitLab’s robust Security Scanning can be configured for GitOps Pull Deployment projects. This adds Security scan results to developer MRs and to GitLab Security Dashboards. Adding Dynamic Review Environments not only enables specific types of security scanning that require an active version of the application - it also enable developers to visually verify application changes with no impact to production clusters.

{{< /admonition >}}

{{< admonition type=abstract title="Target Outcomes" open=true >}}

Add security scanning and dynamic review environments. Per-branch dynamic environments are required for all scanning types that need an active copy of the application - DAST, IAST, Fuzzing and Accessibility Auditing all fall in this category. Dynamic environments can also be used for human review and testing.

1. Use the Pipeline Editor and its Lint feature.

2. Update the Application Build project to add Auto DevOps (Runner Push Deployment) with the GitLab Agent cluster connection method.

3. Track the progress of the build through security scanning, staging environment and ‘production’ environment. (Production in this case means that the Application container is production ready.)

4. Through both environments of the Environment Deployment project using the background page color.

   {{< /admonition >}}

{{< admonition type=tip title="Tip" open=true >}}
This configuration also works for any kind of **GitLab Runner Push CD** to the cluster using Helm and kubectl commands, not only Auto DevOps.
{{< /admonition >}}

In this Lab you will update the application project to have a review application and perform security scanning.

1. Open 'yourpersonalgroup/hello-world’

2. In the left navigation, *Click* **CI/CD => Editor**

   > You will be editing YAML - be careful that tabbing is properly aligned. Only removing the comment character (“#”) should result in proper tabbing.

3. Under `include:` uncomment `- template: Auto-DevOps.gitlab-ci.yml` which should make the section look like this.:
   ```yaml
   include: 
     - local: .gitlab/ci_includes/increment_semver.gitlabci.yml
     - template: Auto-DevOps.gitlab-ci.yml
   ```

4. Under `variables:` uncomment the variables indicated below at the top of the section. There will be additional variables in the section - starting with `NEXTVERSION:` - that must be left as-is.
   ```
   variables:
     BUILD_DISABLED: 'true'
     TEST_DISABLED: 'true'
     POSTGRES_ENABLED: 'false'
     CODE_QUALITY_DISABLED: 'true'
     MANUAL_PROMOTE: 'true'
     STAGING_ENABLED: 'true'
     
     NEXTVERSION: 'read-from-registry'
      ...
   ```
   
5. Immediately above `determine-version:`, uncomment the following segment. The keyword `container_scanning` should start completely to the left edge and each level indented 2 spaces.
   ```
   container_scanning:
     variables:
       GIT_STRATEGY: fetch
   ```

6. Above the editing area, *Click* the word **Lint**

   > You should have a green banner that says “Syntax is correct”

{{< admonition type=observe title="Observe" open=true >}}
You will now purposely insert a syntax error to see the linter catch it. The GitLab CI Yaml linter only exists in this CI Editor. 
{{< /admonition >}}

7. In the same top navigation area, *Click* **Edit**

8. On a **blank** line *Type* **this is an error**

9. Above the editing area, *Click* the word **Lint**

   > You should have a red banner that says “Syntax is incorrect”

10. Make a mental note of the line number and column noted in the grey box.

11. In the same navigation area, *Click* **Edit**

12. Find the line number and *Delete* **this is an error**

13. Go to the Lint tab to ensure you have clean syntax and then change back to the Edit tab.

14. *Click* **Commit changes**


{{< admonition type=observe title="Observe" open=true >}}
These simple steps enable Auto DevOps Dynamic Review Environments and Security Scanning.
{{< /admonition >}}

{{< admonition type=gitlab title="Web Based Pipeline Editor" open=true >}}

The pipeline editor runs on the GitLab server which gives it the advantages of having the production linter and the ability to create a merged YAML view that shows the final YAML with all includes processed. Other code editors do not have these capabilities because they are not integrated with the GitLab instance. The pipeline editor does not yet allow editing of includes that are in the same project.

{{< /admonition >}}

15. In the left navigation *Click* **Hello World** (The project name banner) 

16. *Click* **CI/CD => Pipelines**

17. Open the most recent non-skipped pipeline by clicking **[the pipeline Status badge]** or **[the pipeline #]**

18. **[Automation wait: ~7 min]** While you wait for the GitLab deployment to staging to complete, notice these things:

     {{< admonition type=observe title="Observe" open=true >}}

19. There are multiple security tests under the “Test” stage.

20. A special environment is deployed by the “Review” stage for DAST testing  by the “Dast” stage.

21. The “Cleanup” stage will run to tear down the dast environment even before you approve the production deployment

22. The “promote-image-to-latest-prod” job no longer runs automatically, but will require a manual play button activation.

    {{< /admonition >}}

23. *Click* **Deployments => Environments**

24. On the ‘staging’ line, to the right, *Click* **Open**

     > If SSL is not yet resolving, click the “advanced” option and open the site anyway.

25. In the browser tabs, *Click* **[the tab with environments]**

26. Return to the same pipeline (Browser Back Control may take you there)

27. For the job ‘production_manual’, *Click* **[the Play button]**


{{< admonition type=observe title="Observe" open=true >}}
Removing or commenting the STAGING_ENABLED variable will eliminate the staging environment step if it is unneeded for your process.
{{< /admonition >}}

{{< admonition type=info title="Info" open=true >}}

The environments in the Application Build Project are built using CD Push through the GitLab Agent and are only for testing and qualifying the Application Container. The environment called ‘Production’ simply means that the container is “production ready” if it passes all tests and gets into that review environment. The ‘promote-image-to-latest’ job is what actually signals downstream Environment Deployment Projects that a new version has been published. The DevOps methodology and processes you use should determine how much testing and qualification is done in this Application Build Project and how much is done in Environment Deployment Projects. There could be a lot of testing in both if downstream environments want to double check or have additional checks before deploying to actual production endpoints.

{{< /admonition >}}

24. **[Automation wait: ~3 min]** Wait for the browser_performance job to complete successfully and the Play button to appear on the ‘promote-image-to-latest’ job. 

20. **IMPORTANT:** For the job ‘promote-image-to-latest’ *Click* **[the Play button]**

21. *Click* **Packages & Registries => Container Registry**

22. *Click* **[the line ending in ‘/main’]**

23. Search for the latest-prod tag

    > It should have been built in the last 15 minutes. There should also be a new version tag with the same value for ‘Digest’

{{< admonition type=warning title="Warning" open=true >}}

This last step is what updates the image tags so that the version that was just tested is marked as ready for Environment Deployment projects to consume.

{{< /admonition >}}

{{< admonition type=success title="Accomplished Outcomes" open=true >}}

1. Use the Pipeline Editor and its Lint feature.

2. Update the Application Build project to add Auto DevOps (Runner Push Deployment) with the GitLab Agent cluster connection method.

3. Track the progress of the build through security scanning, staging environment and ‘production’ environment. (Production in this case means that the Application container is production ready.)

4. Through both environments of the Environment Deployment project using the background page color.

   {{< /admonition >}}
