---
title: "Lab 5.2: Merge Request and Review App (Optional)"
weight: 52
chapter: true
draft: false
description: "Security scanning in the application project."
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

# Lab 5.2: Merge Request and Review App (Optional)

> **Keyboard Time**: 10 mins, **Automation Wait Time**: 5 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

{{< admonition type=gitlab title="Merge Request: Developer’s Single Pane of Glass" open=true >}}

GitLab’s MR view of vunerability findings and code quality changes are unique among security tooling because:

- They are only for the code the developer has just changed on their branch.
- They are presented during coding iterations before merging with shared branches - when developers are much more willing to change implementations and upgrade dependencies to eliminate vulnerabilities.
- They are directly associated with the Developer who created them because the authorship of the branch and commits is automatically tracked.
- They allow developers to collaborate around findings with the full power of GitLab collaborative issues.

{{< /admonition >}}

{{< admonition type=abstract title="Target Outcomes" open=true >}}

1. Perform security scanning on the application.

2. Explore the Merge Request view of vulnerability findings.

   {{< /admonition >}}

3. Open 'yourpersonalgroup/hello-world’

4. In the left navigation *Click* **Repository => Files**

5. Near the top right of the page **Click** ***\*Web IDE\**** (button)

6. In the left file navigation *Navigate to* the file **src/microwebserver.py**

7. Around line 11, *locate* the text `<BODY style="background:`

8. Change the color after the word `background:` to `beige`

   Result: `<BODY style="background:beige">`

   > Other available color values [are listed here](https://www.w3.org/wiki/CSS/Properties/color/keywords)

9. In the left file navigation *Click* **Dockerfile**

10. Change the first line starting with FROM to `FROM python:3.6-slim-bullseye`

11. In the left file navigation *Click* **.gitlab-ci.yml**

12. *Delete* or *Comment* the line **STAGING_ENABLED: true**

    > This will speed our labs up - keep in mind the production environment is only published the container - so a review environment and production environment for integration may be enough environments for your development process.

13. *Click* **Create commit...**

14. *Select* **Create a new branch** (Should be the default)

   {{< admonition type=info title="Info" open=true >}}

   The action of creating a new branch is what creates a per-feature branch review app. Commiting to the main branch would avoid the creation of a feature branch review environment - but this also prevents the developer’s Single Pane of Glass (SPOG) assessment of their changes in the Merge Request view and prevents multiple developers working on changes to the same codebase.

   {{< /admonition >}}

11. Leave the default branch name.

12. *Check* **Start a new merge request** (should be defaulted to checked)

13. *Click* **Commit**

14. On the ‘New merge request’ page, for Title, *Type* **Updating color**

15. *Click* **Create merge request**

    > You will be put within the context of the new merge request

16. On the Merge Request page locate the tab bar containing Overview, Commits, Pipelines and Changes.

17. *Click* **Pipelines**

    > Merge Request pipelines are specifically associated with a Merge Request as well as the underlying branch.

18. To open the pipeline page, *Click* **[the Status badge]** or **[the pipeline #]**

    > The Review stage is where GitLab is creating a temporary copy of the running application so that Dynamic Application Security Testing (DAST) can be performed or any other type of QA requiring a running copy of the application.

19. **[Automation wait: ~5 min]** Wait for the Review stage to complete.

{{< admonition type=observe title="Observe" open=true >}}
The jobs are the same as when we enabled Auto DevOps and deployed to staging - but now we are deploying to a completely isolated branch.
{{< /admonition >}}

20. On the left navigation, *Click* **Deployments => Environments**

21. Next to ‘review/<your_mr_branch_name>’ *Click* **Open**.

22. This review environment should have the new background color.

23. In the browser tabs, *Click* **Environments**

24. Next to ‘production’ *Click* **Open**

25. The production environment should have the original background color.

26. In the browser tabs, *Click* **Environments**

27. In the left navigation, *Click* **Merge Requests**

28. *Click* **[your merge request (“Updating color”)]**

29. In the main page body, next to ‘Security scanning detected…’, *Click* **Expand**

30. In the left navigation, *Click* **[one of the findings links]**

{{< admonition type=observe title="Observe" open=true >}}
The vulnerability record you are viewing allows action to be initiated immediately with controls such as Dismiss vulnerability, Resolve with merge request, Download patch to resolve and Create issue.
{{< /admonition >}}

31. *Click* **[the small X in the upper right corner]**

32. Take a mental note of the total number of Critical findings.

33. In the left navigation, *Click* **Security & Compliance => Vulnerability report**

34. Hover over the description for one of the vulnerabilities and notice the branch reference. They are all from the main branch.

{{< admonition type=observe title="Observe: The Right Information at the Right Time with Known Problem ownership" open=true >}}

The Merge Request gives excellent motivational context for the formation of an active vulnerability remediation habit because:

1. All the vulnerabilities in the dashboard are from the default branch “main”, so the ones in our Merge Request are unique to the code we just changed (the old container we introduced and) we have an opportunity to resolve them. 
2. An attempt to merge without resolving the findings will cause the whole development team and the security department to pay attention to the new vulnerabilities.
3. Since we are still making decisions about what container would be best to use, it’s a great time to choose a newer one and take a little time to fix up any of our code that might be affected by the new version.
4. Our other unit and code tests should flush out if updating the container version causes functional code problems in the next push to GitLab.{{< /admonition >}}

{{< admonition type=success title="Accomplished Outcomes" open=true >}}

1. Perform security scanning on the application.

2. Explore the Merge Request view of vulnerability findings.

   {{< /admonition >}}