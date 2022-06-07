---
title: "Lab 5.3: Merge MR and Deploy to Environment (Optional)"
weight: 53
chapter: true
draft: false
description: "Security scanning in the application project."
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

# Lab 5.3: Merge MR and Deploy to Environment (Optional)

> **Keyboard Time**: 10 mins, **Automation Wait Time**: 10 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

In this Lab you will merge your MR and see the changes through deployment in the Environment Deployment Project.

1. Open 'yourpersonalgroup/hello-world’

2. On the left navigation, *Click* **Merge Requests**

3. *Click* **[your merge request (“Updating color”)]**

   > The information in the main body of the merge request is what is contstantly updated. Approvals also have to be given when they are configured.

4. We are going to pretend you’ve gotten your code running, resolved all CI checks, addressed all MR feedback and gotten the sufficient approvals to merge.

5. In the main page body, Click* **Merge** (button)

6. On the left navigation, *Click* **CI/CD => Pipelines**

   > One pipeline starts with your merge request name - when a branch is merged, the previous pipeline’s “stop_review” job is run to destroy the review environment and prevent test environment sprawl.
   >
   > The other pipeline starts with ‘Merge branch’ and is for the merge into the default branch and it will push the changes through both a staging and production environment.

7. **[Automation wait: ~1 min]** Wait for the shorter pipeline to complete.

8. On the left navigation, *Click* **Deployments => Environments**

9. Notice the  ‘review/<your_mr_branch_name>’ review app is gone - it was removed by the MR pipeline running “stop_review” when you merged to main.

10. To return to the pipeline list, *Click* **[the browser back button]**

11. To open the “Merge branch…” pipeline page, *Click* **[the Status badge]** or **[the pipeline #]**

12. **[Automation wait: ~2 min]** Wait until the pipeline starting with “Merge branch…” has a status of ‘blocked’ (meaning pressing play is required to continue) or in the pipeline details page you can see that the ‘staging’ job is complete 

13. On the left navigation, *Click* **Deployments => Environments**

14. Next to ‘staging’ *Click* **Open**

15. This environment should have the new background color.

16. In the browser tabs, *Click* **Environments**

17. Next to ‘production’ *Click* **Open**

18. The production environment should still have the original background color.

19. In the browser tabs, *Click* **Environments**

20. On the left navigation, *Click* **CI/CD => Pipelines**

21. To open the blocked pipeline starting with ‘Merge branch…’ *Click* **[the Status badge]** or **[the pipeline #]**

22. Under the ‘Production’ stage, on the ‘production_manual’ job, *Click* **[the Play button icon]**

23. **[Automation wait: ~2 min]** Wait for the GitLab deployment to production to complete successfully.

24. On the left navigation, *Click* **Deployments => Environments**

25. Next to ‘production’ *Click* **Open**

26. The production environment should have the new background color.

27. In the browser tabs, *Click* **Environments**

28. On the left navigation, *Click* **CI/CD => Pipelines**

29. To open the blocked pipeline starting with ‘Merge branch…’ *Click* **[the Status badge]** or **[the pipeline #]**

30. **[Automation wait: ~5 min]**  Wait for [the play icon] to appear on the ‘promote-image-to-latest’ job. Hitting the browser refresh button helps ensure your status display is accurate.

    > You may have to wait for the ‘browser_performance’ CI test job to complete before [the Play button] appears on the ‘promote-image-to-latest’ job.

31. **IMPORTANT:** Once it is available on the job ‘promote-image-to-latest’ *Click* **[the Play button]**

{{< admonition type=info title="Info" open=true >}}

The environments in the Application Build Project are only for testing and qualifying the build - even the one called ‘Production’. The ‘promote-image-to-latest’ is what actually signals downstream Environment Deployment Projects that a new version has been published. The DevOps methodology and processes you use should determine how much testing and qualification is done in this Application Build Project and how much is done in Environment Deployment Projects. There could be a lot of testing in both if downstream environments want to double check or have additional checks before deploying to actual production endpoints.

{{< /admonition >}}

## Part B: GitOps CD Pull Changes to Production

>**Keyboard Time**: 10 mins, **Automation Wait Time**: 5 mins
>**Scenarios:** Instructor-Led, Self-Paced

{{< admonition type=info title="Info" open=true >}}

While these lab steps completely close the loop with an end-to-end change with review environments and security scanning, since you have seen this flow already, it is optional to push this particular change all the way to production..

{{< /admonition >}}

1. Open 'yourpersonalgroup/world-greetings-env-1’

   > You will now take the change in the World Greeting Environment Deployment Project.

2. *Click* **CI/CD => Schedules**

3. On the right of CheckForNewContainerVersion *Click* **[the play button]**

   > If there are no scheduled pipelines, just navigate to **CI/C => Pipelines** and *Click* **Run pipeline** and *Click* **Run pipeline**

4. On the left navigation, *Click* **CI/CD => Pipelines**

5. On the latest pipeline *Click* **[the Status badge]** or **[the pipeline \#]**

6. Wait until the Child deploy pipeline has a status of ‘blocked’ indicated by the gear icon (meaning manual action is needed)

7. Expand the Downstream pipeline with the great than arrow (`>`).

8. **[Automation wait: ~3 min]** Wait until the update-staging-manifests job has a green check next to it

9. On the left navigation, *Click* **Deployments => Environments**

10. Next to ‘staging’ *Click* **Open**

11. The staging environment should have the new background color.

12. In the browser tabs, *Click* **Environments**

13. Next to ‘production’ *Click* **Open**

14. The production environment should still have the original background color.

15. In the browser tabs, *Click* **Environments**

16. On the left navigation, *Click* **CI/CD => Pipelines**

17. To open the blocked pipeline, *Click* **[the Status badge]** or **[the pipeline #]**

18. Expand the Downstream pipeline with the great than arrow (`>`).

19. Next to the update-production-manifests job, *Click* **[the play button]**

20. [Automation wait: ~5 min] Wait for the GitLab deployment to production to complete successfully and wait ~ 3 more minutes for the GitLab Agent to perform a GitOps CD Pull.

21. On the left navigation, *Click* **Deployments => Environments**

22. Next to ‘production’ *Click* **Open**

23. The production environment should have the new background color.

    > This is your change in the actual production environment. Keep in mind there could be many Environment Deployment Projects that consume the Application Build Project ‘hello-world’ container image. They could be using schedules, manual triggering by running pipelines or merge requests. For the supported methods in these working example projects you can read more here.

{{< admonition type=info title="Info" open=true >}}

In this scenario we had a branch review environment, then a staging and production environment for the Application Build Project. If the Application Build Project is used only by the same environment types which are also directly managed by the same DevOps team as the Application Build Project, then it may make sense to ensure there is enough testing in the Application Build Project that Environment Deployment Projects can simply ship the application to production (disable staging).

However, if the artifacts (container in this case) of the Application Build Project are used by many different types of environments and/or by Ops teams that have seperation of security or duties from the Application Build Project team, then there could be a lot of testing in downstream Environment Deployment Projects as well.

{{< /admonition >}}
